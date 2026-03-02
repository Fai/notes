---
title: Self-Hosted LLM on AWS — Best Practices
tags: [aws, llm, gpu, vllm, self-hosted, best-practices]
created: 2026-03-02
updated: 2026-03-02
---

# Self-Hosted LLM on AWS — Best Practices

How to deploy, serve, and operate open-weight LLMs on AWS GPU instances.

---

## GPU Instance Selection

### Instance Families

| Family | GPU | VRAM | Best For |
|--------|-----|------|----------|
| g6 | NVIDIA L4 | 24 GB | Inference (7B–14B models). Best price/perf for serving |
| g6e | NVIDIA L40S | 48 GB | Inference (14B–70B). Double VRAM of g6 |
| g5 | NVIDIA A10G | 24 GB | Inference. Previous gen, being replaced by g6 |
| p5 | NVIDIA H100 | 80 GB | Training + large inference (70B+) |
| p4d | NVIDIA A100 | 40 GB | Training + inference. Previous gen |

### Sizing Guide

| Model Size | Quantization | VRAM Needed | Recommended Instance |
|------------|-------------|-------------|---------------------|
| 7B | FP16 | ~14 GB | g6.xlarge (1x L4) |
| 7B | INT8/AWQ | ~7 GB | g6.xlarge |
| 14B | FP16 | ~28 GB | g6e.xlarge (1x L40S) |
| 14B | INT8/AWQ | ~14 GB | g6.xlarge (tight) or g6.2xlarge |
| 7B + 7B-VL | INT8 both | ~15 GB | g6.2xlarge (multi-model) |
| 70B | INT4/GPTQ | ~35 GB | g6e.2xlarge or p4d.24xlarge |
| 70B | FP16 | ~140 GB | 2x p4d or p5 |

### Region Availability (APAC, as of Mar 2026)

| Region | G5 | G6 | G6e | P5 |
|--------|----|----|-----|-----|
| ap-southeast-1 (Singapore) | ❌ | ❌ | ❌ | ❌ |
| ap-southeast-7 (Thailand) | ❌ | ❌ | ❌ | ❌ |
| ap-northeast-1 (Tokyo) | ✅ | ✅ | ✅ | ✅ |
| ap-northeast-2 (Seoul) | ✅ | ✅ | ✅ | ✅ |
| ap-southeast-2 (Sydney) | ✅ | ✅ | ❌ | ✅ |
| ap-south-1 (Mumbai) | ✅ | ✅ | ❌ | ✅ |

> ⚠️ **Singapore and Thailand have NO GPU instances.** Use Tokyo for lowest latency to SEA.

### Cost Optimization

- **1yr Reserved Instance** — ~40% savings vs on-demand
- **Spot instances** — 60-70% savings but can be interrupted. Only for batch/non-critical
- **Right-size** — Don't use g6.4xlarge if g6.2xlarge fits your model
- **Schedule** — If not 24/7, stop instances during off-hours via EventBridge

---

## Serving Framework: vLLM

vLLM is the standard for production LLM serving. Key features:
- PagedAttention (efficient KV cache memory)
- Continuous batching (high throughput under concurrent load)
- Tensor parallelism (multi-GPU)
- OpenAI-compatible API server

### Deployment

```bash
# Install
pip install vllm

# Serve a model (basic)
vllm serve Qwen/Qwen3-7B-Instruct \
  --dtype auto \
  --max-model-len 4096 \
  --gpu-memory-utilization 0.90 \
  --port 8000

# Serve with quantization (INT8)
vllm serve Qwen/Qwen3-7B-Instruct-AWQ \
  --quantization awq \
  --max-model-len 4096 \
  --port 8000

# Multi-model (separate ports)
vllm serve Qwen/Qwen3-7B-Instruct-AWQ --port 8000 &
vllm serve Qwen/Qwen3-VL-7B-Instruct-AWQ --port 8001 &
```

### Key Parameters

| Parameter | Recommended | Notes |
|-----------|-------------|-------|
| `--gpu-memory-utilization` | 0.85–0.92 | Leave headroom for KV cache growth |
| `--max-model-len` | 4096–8192 | Longer = more VRAM for KV cache |
| `--dtype` | `auto` or `half` | FP16 default, use `bfloat16` for newer GPUs |
| `--quantization` | `awq` or `gptq` | 50% VRAM reduction, <2% quality loss |
| `--max-num-seqs` | 64–256 | Max concurrent sequences. Tune for your load |
| `--enforce-eager` | (flag) | Disable CUDA graphs if OOM. Slower but stable |

### Health Check

```bash
curl http://localhost:8000/health
# Returns 200 if model is loaded and ready
```

### OpenAI-Compatible API

vLLM exposes `/v1/chat/completions` and `/v1/completions`:

```bash
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Qwen/Qwen3-7B-Instruct",
    "messages": [{"role": "user", "content": "สวัสดี"}],
    "max_tokens": 512
  }'
```

---

## Concurrency, KV Cache & RAG Impact

### How Concurrent Requests Use VRAM

Each active request holds a KV cache in VRAM. Total VRAM = model weights + (KV cache × concurrent requests).

**KV cache size per request (approximate, 7B model):**

| Context Length | KV Cache / Request |
|---------------|-------------------|
| 1K tokens | ~0.1 GB |
| 2K tokens | ~0.2 GB |
| 4K tokens | ~0.4 GB |
| 8K tokens | ~0.8 GB |
| 32K tokens | ~3.2 GB |

### Concurrency Calculator

**Formula:**
```
Max concurrent = (Total VRAM - Model weights) × gpu_memory_utilization / KV cache per request
```

**Example: g6.2xlarge (24GB), Qwen3-7B INT8 (7GB), 90% utilization:**

| Context | Available VRAM | KV/Req | Max Concurrent | Est. Throughput |
|---------|---------------|--------|----------------|-----------------|
| 1K (no RAG) | ~15 GB | 0.1 GB | ~75 | 40-75 req/s |
| 2K (light RAG) | ~15 GB | 0.2 GB | ~40 | 20-40 req/s |
| 4K (heavy RAG) | ~15 GB | 0.4 GB | ~20 | 10-20 req/s |
| 8K (long RAG) | ~15 GB | 0.8 GB | ~10 | 5-10 req/s |

### How RAG Kills GPU Concurrency

Without RAG:
```
System prompt: ~500 tokens + User message: ~100 tokens + Response: ~300 tokens
Total: ~900 tokens → small KV cache → high concurrency
```

With RAG:
```
System prompt: ~500 + Retrieved chunks: ~2,000-4,000 + User: ~100 + Response: ~300
Total: ~3,000-5,000 tokens → 3-5× more KV cache → 3-5× fewer concurrent requests
```

### Multi-Model Impact

Running two models (e.g., text + vision) on one GPU further reduces available VRAM:

| Setup | Weights | Available for KV | Max Concurrent (4K ctx) |
|-------|---------|-----------------|------------------------|
| Single 7B (INT8) | 7 GB | ~15 GB | ~37 |
| Dual 7B + 7B-VL (INT8) | 15 GB | ~8 GB | ~20 |
| Single 14B (INT8) | 14 GB | ~8 GB | ~20 |

### Best Practices

1. **Route RAG queries to Bedrock API** — API models have no VRAM constraint; keep GPU for short-context high-throughput queries
2. **Limit RAG chunk size** — If self-hosted RAG is required, cap retrieved context to 1-2K tokens
3. **Single model per GPU** — Don't run text + vision on same GPU in production; use Bedrock for vision
4. **Set `--max-model-len`** — Cap context length in vLLM to prevent one long request from starving others
5. **Monitor KV cache utilization** — vLLM exposes `gpu_cache_usage_perc` metric; alert at >85%

---

## Production Checklist

### Infrastructure

- [ ] GPU instance in correct region (check availability table above)
- [ ] Reserved Instance purchased (1yr minimum for 24/7 workloads)
- [ ] EBS gp3 volume for model weights (100GB+ for multiple models)
- [ ] Security group: only allow inbound from your VPC/LB (not 0.0.0.0/0)
- [ ] IAM role with minimal permissions (no admin)
- [ ] CloudWatch agent installed for GPU metrics

### Model Deployment

- [ ] Model weights pre-downloaded to EBS (not downloaded at boot)
- [ ] Quantized model (AWQ/GPTQ) if VRAM is tight
- [ ] vLLM started via systemd service (auto-restart on crash)
- [ ] Health check endpoint verified
- [ ] Max concurrent sequences tuned for expected load

### Monitoring

- [ ] GPU utilization (CloudWatch custom metric or DCGM)
- [ ] GPU memory usage
- [ ] Request latency (p50, p95, p99)
- [ ] Tokens per second (throughput)
- [ ] Error rate (5xx responses)
- [ ] Queue depth (requests waiting)

### High Availability

- [ ] Bedrock as automatic fallback when self-hosted is down
- [ ] CloudWatch alarm on health check failure → SNS notification
- [ ] Auto-restart via systemd + watchdog
- [ ] For critical workloads: 2 instances behind ALB (active-active)

### Security

- [ ] Model served behind ALB/NLB (not exposed to internet)
- [ ] TLS termination at load balancer
- [ ] VPC endpoints for S3 (model download) and CloudWatch
- [ ] No secrets in user data or environment variables (use Secrets Manager)
- [ ] Audit logging enabled (CloudTrail)

### Cost Control

- [ ] Reserved Instance for 24/7 workloads
- [ ] Spot for batch/non-critical inference
- [ ] Budget alarm set in AWS Budgets
- [ ] Right-sized instance (don't over-provision)
- [ ] Stop dev/staging GPU instances outside business hours

---

## Quantization Guide

| Method | VRAM Savings | Quality Impact | Speed Impact |
|--------|-------------|----------------|-------------|
| FP16 (baseline) | 0% | None | Baseline |
| INT8 (W8A8) | ~50% | <1% degradation | ~Same or faster |
| AWQ (INT4) | ~75% | 1-3% degradation | Faster (less memory bandwidth) |
| GPTQ (INT4) | ~75% | 1-3% degradation | Similar to AWQ |
| GGUF (llama.cpp) | Variable | Variable | CPU-friendly, not for vLLM |

**Recommendation:** Use AWQ quantized models for production. HuggingFace has pre-quantized versions for most popular models (search for `-AWQ` suffix).

---

## Thai Language Model Options (Mar 2026)

| Model | Size | Thai Quality | License | Notes |
|-------|------|-------------|---------|-------|
| Qwen3 | 7B/14B/32B | Good | Apache 2.0 | Best open-weight multilingual |
| Qwen3-VL | 7B | Good | Apache 2.0 | Vision + Thai text |
| Typhoon2 | 7B | Excellent | Research | SCB10X, best Thai-specific |
| Llama 4 | 8B/70B | Moderate | Llama license | Improving Thai support |
| OpenThaiGPT | 7B–72B | Good | Apache 2.0 | Thai-focused fine-tune |

> For Thai chatbot workloads, Qwen3 offers the best balance of Thai quality, license freedom, and model ecosystem (including VL variant).

---

## Bedrock API Best Practices

### Model Selection

| Use Case | Model | Cost ($/1M output) |
|----------|-------|-------------------|
| Simple classification | Nova Micro | $0.14 |
| General Q&A | Nova Pro | $3.20 |
| Conversations | Claude 3.5 Haiku | $4.00 |
| Complex reasoning | Claude 3.5/3.7 Sonnet | $15.00 |
| Long document analysis | Claude 3.7 Sonnet | $15.00 |

### Cost Optimization

1. **Prompt caching** — Cache system prompts for 90% input cost reduction on repeated context
2. **Batch inference** — 50% off for async workloads (24hr turnaround)
3. **Start cheap** — Benchmark Nova Micro first, escalate only if quality insufficient
4. **Set MaxTokens** — Always cap output length to prevent runaway generation
5. **Compress prompts** — Every unnecessary token costs money on every call

### Cross-Region Inference

Bedrock supports automatic cross-region routing at no extra cost. Enable it for:
- Higher availability (automatic failover)
- Capacity overflow handling
- No pricing penalty (source region pricing applies)

---

## Hybrid Architecture Pattern

```
                    ┌─────────────────┐
                    │ Semantic Router  │
                    └────┬────────┬───┘
                         │        │
              Self-hosted│        │Bedrock API
              (sensitive,│        │(complex,
               high-vol) │        │ general)
                         ▼        ▼
                    ┌─────┐  ┌────────┐
                    │ GPU │  │Bedrock │
                    │vLLM │  │Claude/ │
                    │     │  │Nova    │
                    └──┬──┘  └───┬────┘
                       │         │
                       └────┬────┘
                            ▼
                      ┌──────────┐
                      │ Response │
                      │ + Reply  │
                      └──────────┘
```

**When to route to self-hosted:**
- Query contains sensitive/PII data
- Image/document input (VL model)
- High-frequency FAQ (cheaper at scale)
- Consistent latency needed (no cold start)

**When to route to Bedrock:**
- Complex multi-step reasoning
- Diverse/unpredictable queries
- Overflow when GPU is at capacity
- GPU instance is down (fallback)

---

## See Also

- [Bedrock Knowledge Bases](bedrock-knowledge-bases.md) — RAG with Bedrock
- [Bedrock Cost Control](bedrock-cost-control.md) — Token limits and budgets
- [Bedrock Guardrails](bedrock-guardrails.md) — Content filtering
- [AI/ML Governance](ai-ml-governance.md) — Governance framework

---

## References

- [vLLM docs](https://docs.vllm.ai/)
- [AWS EC2 GPU instances](https://aws.amazon.com/ec2/instance-types/#Accelerated_Computing)
- [EC2 instance types by region](https://docs.aws.amazon.com/ec2/latest/instancetypes/ec2-instance-regions.html)
- [Bedrock pricing](https://aws.amazon.com/bedrock/pricing/)
- [Qwen3 models](https://huggingface.co/Qwen)
