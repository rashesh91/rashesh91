# Hi, I'm Rashesh 👋

**AI Infrastructure Engineer** — I build the systems that train, serve, and operate AI models in production.

---

## Portfolio: End-to-End AI Infrastructure Stack

Three connected projects that demonstrate the full lifecycle of production AI infrastructure — from voice serving to model training to production operations.

---

### Project 1 — Voice Agentic AI Platform
**[`ml-inference-gitops`](https://github.com/rashesh91/ml-inference-gitops)**

Real-time voice assistant on Kubernetes: browser microphone → Whisper STT → Mistral ReAct agent (tools: weather, search, calculator) → Edge TTS → audio response.

```
Browser mic → WebSocket → Whisper STT → Mistral 7B ReAct → Edge TTS → Audio
```

**Stack:** Python · FastAPI · Kubernetes · ArgoCD GitOps · vLLM · Helm · GPU workloads

---

### Project 2 — LLM Fine-Tuning & Evaluation Platform
**[`ml-training-platform`](https://github.com/rashesh91/ml-training-platform)**

Self-service platform: upload a dataset, get a fine-tuned model auto-evaluated and registered — no ML engineer handholding. 5-step Argo Workflow DAG handles the full pipeline.

```
Dataset upload → MinIO → KubeRay LoRA training → MLflow eval → Staging → Production
```

**Stack:** Python · KubeRay · HuggingFace PEFT · MLflow · Argo Workflows · MinIO · FastAPI dashboard

---

### Project 3 — AI Platform Ops (Production Operations Layer)
**[`ai-platform-ops`](https://github.com/rashesh91/ai-platform-ops)**

Connects and hardens both projects with the five things that separate a demo from a production system:

| Component | Problem Solved |
|-----------|---------------|
| **Custom K8s Operator** (kopf) | MLflow "Deploy" button did nothing → auto-patches voice-gateway within 30s |
| **OpenTelemetry + Grafana Tempo** | "Pipeline is slow" → waterfall trace shows STT=400ms, LLM=2800ms, TTS=250ms |
| **KEDA Autoscaling** | Fixed replicas waste GPU → scale-to-zero Ray workers saves ~60% GPU cost |
| **Argo Rollouts Canary** | New models go live to 100% instantly → 10%→50%→100% with auto-abort gates |
| **SLO Burn Rate Alerts** | 3–4 false-positive pages/week → Google SRE formula, zero false positives |

**Stack:** Python · kopf · OpenTelemetry · Grafana Tempo · KEDA · Argo Rollouts · PrometheusRules

---

## How the Three Projects Connect

```
┌──────────────────────┐   trains model    ┌─────────────────────────┐
│  ml-training-        │ ────────────────▶ │  ml-inference-gitops    │
│  platform            │                   │  (voice-gateway)        │
│  MLflow → Production │                   │  serves users 24/7      │
└──────────┬───────────┘                   └────────────┬────────────┘
           │                                            │
           └─────────────────┬──────────────────────────┘
                             │
                  ┌──────────▼───────────┐
                  │   ai-platform-ops    │
                  │  operator · tracing  │
                  │  autoscaling · canary│
                  │  SLOs · dashboards   │
                  └──────────────────────┘
```

---

## Skills

**Kubernetes & GitOps** — Operators (kopf), KEDA, Argo Rollouts, ArgoCD App of Apps, Helm, sync waves, ignoreDifferences

**Observability** — OpenTelemetry SDK, Grafana Tempo, Prometheus, ServiceMonitors, SLO burn rate alerting

**ML Infrastructure** — KubeRay, vLLM, HuggingFace PEFT LoRA, MLflow, Argo Workflows, MinIO

**Languages** — Python · YAML · Bash

---

## Minimum Setup to Run Locally

| Project | Requirements |
|---------|-------------|
| Voice Platform (dev mode) | Docker, 8 GB RAM — Ollama replaces GPU |
| Training Platform (dev mode) | Docker, 8 GB RAM — local fine-tuner service |
| Full observability stack | Docker — OTel + Tempo + Grafana, no Kubernetes needed |
| Full cluster demo | kind, 16 GB RAM, 8 CPU — GPU optional |
