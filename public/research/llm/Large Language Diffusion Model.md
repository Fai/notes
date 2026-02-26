เดิม เป็น autoregressive
ใหม่ เป็น LLaDaA
Pre-training
- ไม่สนใจ token ที่ไม่ได้ masked
- เทรน reverse process
- ใช้ KV caching ไม่ได้
- รองรับ variable-length data ได้
Supervised Fine-Tuning
- feed prompt + masked response ไปให้ทำนาย
Inference
- sampling process ทำ re-masking
- เจนสั้นกว่าที่สร้างได้ แต่ยาวกว่าไม่ได้
- ผลในเปเปอร์ เทียบกับ Baseline ของเค้าเอง

Block Diffusion
MMaDa: Multimodal Large Diffusion Language Models