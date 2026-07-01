# On-Premises AI Toolkit

A collection of offline, single-file tools for on-premises AI infrastructure planning and operations. No installation, no server, no internet connection required — download and open in any modern browser.

---

## Tools

| Tool | Description | File |
|------|-------------|------|
| [GPU Requirements Calculator](#gpu-requirements-calculator) | Size GPU infrastructure for on-premises AI deployments | `gpu-calculator.html` |
| [AI Business Case Builder](#ai-business-case-builder) | Build financial business cases for AI projects with ROI, payback period, and printable PDF | `business-case-builder.html` |
| [Model Deployment Advisor](#model-deployment-advisor) | End-to-end deployment guide for open-source AI models on Kubernetes — air-gapped ready | `model-deployment-advisor.html` |

---

## GPU Requirements Calculator

A production-grade tool for calculating GPU, memory, power, and storage requirements for on-premises AI workloads. Designed for both technical engineers and non-technical executives.

### Quick Start

**Use it instantly (no download needed):**
👉 [nik1907.github.io/onprem-ai-toolkit](https://nik1907.github.io/onprem-ai-toolkit/)

**Or download for offline use:**
1. Download `gpu-calculator.html`
2. Open it in Chrome, Edge, Firefox, or Safari
3. No installation, no internet connection, no server required

### Who It's For

- **Infrastructure architects** sizing servers for AI deployments
- **Procurement teams** generating RFQ/RFP documents for vendor quotes
- **Executives** getting a plain-English estimate before committing budget
- **IT managers** planning on-premises AI rollouts

---

### Modes

#### Guided Mode (Executive)
Step-by-step flow for non-technical users. Select a use case, answer three questions, and get an instant result in plain English — GPU count, cost range, procurement timeline, and a vendor summary you can copy and send.

Use cases covered:
- Customer Support Bot
- Voice Assistant
- Document Search & Q&A
- Image Generator
- Code Assistant
- Data Analytics AI
- Full AI Agent (all-in-one)

#### Expert Mode (Engineer)
Full control over every parameter. Build a stack of AI models, set precision, context length, concurrent users, and serving framework. See a detailed VRAM breakdown per model with the full formula shown.

---

### Supported AI Workload Types

| Type | Label | Example Models |
|------|-------|----------------|
| Large Language Model | LLM | Llama 3.1 8B/70B/405B, Qwen2.5 7B–72B, Mistral 7B, DeepSeek-R1, Phi-4, Gemma 2 |
| Speech to Text | STT | Whisper Tiny/Base/Small/Medium/Large-v3, faster-whisper |
| Text to Speech | TTS | Kokoro 82M, MeloTTS, StyleTTS2, Coqui XTTS v2, Bark |
| Image Generation | IMG | Stable Diffusion 1.5/XL/3.5, Flux.1 Schnell/Dev |
| Embedding | EMB | BGE-small/large, E5-large, nomic-embed, Jina v3 |
| Vision / Object Detection | Vision | YOLOv8 Nano–XL, YOLOv11, RT-DETR, SAM Base/Large, SAM 2 |
| Reranker | Reranker | BGE Reranker v2-m3/Large, ms-marco MiniLM L6/L12, Jina v2 |
| Vision-Language Model | VLM | LLaVA 1.6 7B/13B, Qwen2-VL 7B/72B, InternVL2 8B/26B, Llama 3.2 Vision, Phi-4 Vision |
| Video Generation | Video | Wan2.1 1.3B/14B, CogVideoX 2B/5B, HunyuanVideo 13B, AnimateDiff |

Any combination of these types can be stacked — the calculator sums VRAM requirements across the full stack.

---

### Calculation Engine

The calculator uses production-grade formulas, not simplified estimates.

**LLM / VLM VRAM formula:**
```
Weights    = params × precision_bytes
KV Cache   = layers × kv_heads × head_dim × 2 × context_tokens × concurrent_users × precision_bytes / 1e9
Workload   = params × workload_multiplier  (0 inference, 0.6 LoRA, 14.0 full training)
Activations = 0.5 GB (inference) or params × 0.5 (training)
VLM extra  = vision encoder overhead (CLIP/SigLIP, 1–2 GB)
─────────────────────────────────────────────────────────
Raw subtotal
× runtime spike buffer (10–30%, default 15%)
× safety buffer (1.20)
+ CUDA overhead (1.5 GB per GPU)
+ serving framework overhead (vLLM 3 GB, TGI 2 GB, Ollama 1.5 GB)
= Total VRAM per model
```

**Advanced options:**
- **Flash Attention** — reduces KV cache peak by ~40%
- **Serving framework** — vLLM, HuggingFace TGI, Ollama, or None
- **High Availability (N+1)** — adds one spare GPU to the count
- **Runtime spike buffer** — 10 / 15 / 20 / 30%

**Confidence intervals** — every result shows three scenarios:
- P25 Optimistic: Flash Attention on, lighter load, no HA
- P50 Baseline: current settings
- P90 Conservative: peak load, full overhead, N+1 HA

---

### Output — What You Get

**VRAM & GPU Count**
- Total VRAM required across the full model stack
- Number of GPUs needed (with confidence interval P25–P90)
- VRAM utilisation percentage

**Per-Model Breakdown**
- VRAM per model with a proportional bar chart
- Architecture details: layers, KV heads, head dimension, precision
- "Show math" button expands the full substituted formula for each model

**Procurement & Cost**
- GPU unit price estimate (market rate)
- Total hardware cost range
- Delivery timeline — with regional estimates for UAE, US, EU, India, Singapore, and other regions
- UAE-specific BIS export control warning for H100/H200 orders

**Server Infrastructure**
- CPU cores and system RAM
- Network interface (25 GbE / 100 GbE / NDR InfiniBand)
- Rack units
- Cooling requirement in BTU/hr
- PSU redundancy recommendation
- OS and CUDA stack recommendation

**Storage**
- Model storage on disk
- Log storage estimate
- Vector database storage (for RAG systems)

**Interpretation Card**
Plain-English explanation of why the result requires that many GPUs — weights vs KV cache breakdown, overhead percentage, confidence range, and utilisation health.

**Warnings**
- Multi-GPU clustering required
- NVLink / NVSwitch interconnect required
- Power and cooling threshold exceeded
- Hardware scale limit (single node)
- UAE BIS export control (H100/H200)

---

### PDF Export — RFQ and RFP

Click **Print RFQ** or **Print RFP** in the header to generate a formatted procurement document via the browser's print dialog. Save as PDF directly from the print dialog.

- **RFQ (Request for Quotation)** — hardware spec, quantity, cost estimate, delivery terms, acceptance criteria
- **RFP (Request for Proposal)** — background, 14 technical requirements, three-tier scope (minimum/target/preferred), 4-criterion vendor evaluation matrix, submission instructions

Both documents work fully offline — no external services or fonts required.

---

### Supported GPU Hardware

| GPU | VRAM | TDP |
|-----|------|-----|
| NVIDIA RTX 4090 | 24 GB | 450 W |
| NVIDIA RTX 6000 Ada | 48 GB | 300 W |
| NVIDIA L40S | 48 GB | 350 W |
| NVIDIA A100 40 GB | 40 GB | 400 W |
| NVIDIA A100 80 GB | 80 GB | 400 W |
| NVIDIA H100 SXM | 80 GB | 700 W |
| NVIDIA H100 NVL | 94 GB | 400 W |
| NVIDIA H200 SXM | 141 GB | 700 W |
| NVIDIA B200 | 192 GB | 1000 W |
| AMD MI300X | 192 GB | 750 W |

---

### Glossary

Click **Glossary** in the header to open a searchable reference panel with 30 terms covering VRAM, KV cache, Flash Attention, quantisation, MoE, GQA, NVLink, BIS export controls, RAG, vLLM, tensor parallelism, and more.

---

### Regional Delivery Estimates

The procurement card includes a region dropdown with sourced lead-time data:

| Region | Source |
|--------|--------|
| UAE / Abu Dhabi | Venom Gaming UAE, Ingram Micro Gulf, BIS Tier 2 compliance data |
| United States | Dell, Supermicro, HPE direct OEM data |
| Europe | EU distribution chain estimates |
| India | Local VAR and import timeline data |
| Singapore | APAC hub estimates |
| Other | Conservative global baseline |

UAE orders for H100/H200 trigger a BIS export control notice with channel recommendations (Ingram Micro Gulf, Redington Gulf) and a note to begin VEU compliance 6 months ahead.

---

### Browser Compatibility

| Browser | Support |
|---------|---------|
| Chrome 90+ | Full |
| Edge 90+ | Full |
| Firefox 88+ | Full |
| Safari 15+ | Full |

---

### Privacy

The calculator runs entirely in your browser. No data is sent to any server. API keys (used only for the optional AI Advisor feature) are stored in `sessionStorage` only — they are never written to `localStorage` or transmitted beyond the API call you initiate.

---

## Repo Structure

```
onprem-ai-toolkit/
├── gpu-calculator.html             # GPU Requirements Calculator
├── business-case-builder.html      # AI Business Case Builder
├── model-deployment-advisor.html   # Model Deployment Advisor
├── index.html                      # Platform homepage
└── README.md
```

More tools will be added to this repository over time.

---

---

## AI Business Case Builder

An offline tool for building financial business cases for AI projects. Designed for AI adoption teams presenting to management.

### Quick Start

**Use it instantly:** [nik1907.github.io/onprem-ai-toolkit/business-case-builder.html](https://nik1907.github.io/onprem-ai-toolkit/business-case-builder.html)

Or download `business-case-builder.html` and open in any browser.

### What It Does

Fill in four sections — use case details, how the process works today, what impact you expect from AI, and which UAE AI Strategy goals it supports. The tool calculates ROI, payback period, staff hours saved, and 3-year net savings. Output is a printable PDF business case in English with an Arabic executive summary.

### Automation vs Agentic AI Mode

Toggle between two modes:

**Automation mode** — for AI that replaces repetitive manual steps. Measures cost savings, FTE reduction, processing speed, and error reduction.

**Agentic AI mode** — for AI agents that make decisions, handle exceptions, and orchestrate across multiple systems. Adds decision accuracy rate, autonomy rate, exception handling rate, cross-system coordination value, 24/7 availability value, and a governance framework with risk level and required oversight model.

### Output

- Annual benefit (AED)
- Payback period (months)
- 3-year ROI (%)
- 3-year net saving (AED)
- Savings breakdown by category
- Strategic alignment with UAE National AI Strategy pillars
- Plain-English "Why approve this project?" summary
- Printable PDF with Arabic executive summary (ملخص تنفيذي)
- Governance & risk card (Agentic mode)

---

---

## Model Deployment Advisor

An offline deployment guide that takes you from "we have GPUs and a model name" to a fully running service on Kubernetes — covering every step an air-gapped team needs.

### Quick Start

**Use it instantly:** [nik1907.github.io/onprem-ai-toolkit/model-deployment-advisor.html](https://nik1907.github.io/onprem-ai-toolkit/model-deployment-advisor.html)

Or download `model-deployment-advisor.html` and open in any browser.

### What It Does

Select your deployment goal (chat, RAG, code, STT, TTS, VLM, embedding, agentic), choose a model, and configure your GPU type, count, and Kubernetes environment. The advisor generates a complete deployment package across six tabs.

### Models Covered (open source only, no API keys)

| Category | Models |
|----------|--------|
| LLM | Llama 3.1 8B/70B, Qwen2.5 7B/72B, Mistral 7B, Phi-4, DeepSeek-R1 Distill 7B |
| Code | Qwen2.5 Coder 7B/32B |
| Embedding | BGE-M3 (multilingual/Arabic), BGE Large EN |
| Reranker | BGE Reranker v2-m3 |
| STT | Whisper Large v3, Whisper Medium |
| TTS | Kokoro 82M, Coqui XTTS v2 |
| VLM | Qwen2-VL 7B, Phi-4 Vision |

### Output — Six Deployment Tabs

**Tab 1 — Pre-Download Checklist**
Everything to gather before going air-gapped: exact `huggingface-cli` download commands, Docker image save/load commands for the internal registry, model file list, total download size, and offline environment variable configuration.

**Tab 2 — Prerequisites**
Cluster readiness checklist with verification commands: NVIDIA driver version, CUDA compatibility, nvidia-container-toolkit, Kubernetes GPU device plugin, node labels, namespace setup, and NFS PersistentVolume configuration.

**Tab 3 — Configuration**
Exact serving command with all parameters computed from your inputs — tensor parallel size, context window, GPU memory utilisation, dtype, concurrent requests, and served model name. Includes Docker run alternative and parameter explanation.

**Tab 4 — Kubernetes Manifest**
Copy-paste ready YAML: Namespace, PersistentVolumeClaim, Deployment (with GPU resource limits, node selector, liveness/readiness probes, offline env vars), and ClusterIP Service. Adapted per model type (vLLM for LLMs, TEI for embeddings, faster-whisper for STT).

**Tab 5 — Validate**
Step-by-step test commands: health check, model list endpoint, inference test, streaming test, performance benchmark, and GPU utilisation monitoring.

**Tab 6 — Troubleshoot**
Collapsible answers to the most common deployment failures: pod stuck Pending, CUDA OOM, slow startup, low throughput, image pull errors, model not found, low GPU utilisation, driver version mismatch.

### Supported Infrastructure

- GPU types: RTX 4090, RTX 6000 Ada, L40S, A100 40/80 GB, H100 SXM/NVL, H200
- Serving frameworks: vLLM (production), Ollama (dev), HuggingFace TGI, TEI (embeddings), faster-whisper (STT)
- Deployment target: Kubernetes / Rancher with NFS storage
- Fully air-gapped: all commands use internal registry and offline HuggingFace mode

---

## Need Help With a Deployment?

The calculator tells you what to buy. Actually procuring, standing up, and running an on-premises AI cluster — selecting vendors, navigating BIS export compliance, configuring vLLM in production, capacity planning — is where most teams get stuck.

If you're planning a serious GPU deployment and want a second opinion or hands-on support, get in touch:

- **Email:** gnikki.reddy@gmail.com
- **GitHub:** [github.com/nik1907](https://github.com/nik1907)

---

## Contributing

Issues and pull requests are welcome. Each tool in this repo follows the same principles:
- Single HTML file, no build step
- No CDN dependencies — works fully offline
- No data collection or external calls (except optional user-initiated API features)

---

## License

MIT — free to use, modify, and distribute.
