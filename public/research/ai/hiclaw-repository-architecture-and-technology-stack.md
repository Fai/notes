---
title: HiClaw Repository Architecture and Technology Stack
tags: [research, ai, multi-agent, architecture, hiclaw]
created: 2026-03-27
status: complete
---

# HiClaw Repository Architecture and Technology Stack

> Research based on the local repository at `/home/ubuntu/code/experiment/hiclaw`.

This note captures the two most useful architecture views for understanding HiClaw:

- The **runtime architecture** used during normal Manager and Worker operation
- The **declarative control plane** used for YAML and ZIP driven reconciliation

## Runtime Architecture

```mermaid
flowchart LR
    Human["Human Admin"]

    subgraph Manager["Manager Container"]
        direction TB
        Element["Element Web"]
        Tuwunel["Tuwunel<br/>Matrix Homeserver"]
        Higress["Higress<br/>AI Gateway + MCP Host"]
        MinIO["MinIO<br/>Object Storage"]
        ManagerAgent["Manager Agent<br/>OpenClaw"]
        Scripts["Bash + jq<br/>automation scripts"]
        MCManager["mc mirror"]
    end

    subgraph OpenClawWorker["OpenClaw Worker Container"]
        direction TB
        OW["OpenClaw Worker"]
        MCW["mc"]
        MCPW["mcporter"]
    end

    subgraph CoPawWorker["CoPaw Worker Container"]
        direction TB
        CW["CoPaw Worker"]
        MatrixChannel["MatrixChannel<br/>matrix-nio adapter"]
        MCC["mc"]
        MCPC["mcporter"]
    end

    LLM["LLM Provider APIs"]
    MCP["GitHub / External APIs<br/>via MCP"]

    Human --> Element
    Human -->|all conversations visible| Tuwunel
    Element -->|Matrix client API| Tuwunel

    ManagerAgent <--> Tuwunel
    Scripts <--> ManagerAgent
    Scripts <--> Higress
    ManagerAgent --> MCManager --> MinIO

    OW <--> Tuwunel
    OW --> MCW --> MinIO
    OW --> MCPW --> Higress

    CW --> MatrixChannel --> Tuwunel
    CW --> MCC --> MinIO
    CW --> MCPC --> Higress

    Higress --> LLM
    Higress --> MCP
```

## Declarative Control Plane

```mermaid
flowchart TB
    Spec["Worker / Team / Human YAML<br/>or ZIP package"]
    MinIO["MinIO<br/>hiclaw-config/"]
    MCMirror["mc mirror"]
    Watcher["fsnotify File Watcher"]
    KubeAPI["Embedded kube-apiserver"]
    Kine["kine<br/>SQLite-backed etcd-compatible store"]
    Controller["hiclaw-controller"]
    Reconcilers["Reconcilers<br/>Worker / Team / Human"]
    Shell["Shell automation<br/>create-worker.sh / create-team.sh / create-human.sh"]
    Tuwunel["Tuwunel"]
    Higress["Higress"]
    Runtime["Docker / Podman<br/>or Cloud runtime"]

    Spec --> MinIO
    MinIO --> MCMirror --> Watcher --> KubeAPI
    KubeAPI <--> Kine
    KubeAPI --> Controller --> Reconcilers --> Shell

    Shell --> Tuwunel
    Shell --> Higress
    Shell --> Runtime
    Shell --> MinIO
```

## Interpretation

The runtime path is chat-first: humans interact through Matrix, the Manager coordinates Workers, MinIO holds shared state, and Higress gates all LLM and MCP access.

The declarative path is control-plane-first: resource files land in `hiclaw-config`, the embedded controller stack turns them into CRD-style objects, and the same shell scripts reconcile real workers, teams, permissions, and runtime state.
