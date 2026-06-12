# Hi, I'm Rashesh Patel 👋

**AI Infrastructure Engineer** — Ahmedabad, India · rasheshkumar.patel@gmail.com · [LinkedIn](https://www.linkedin.com/in/rashesh-patel-515744a3) · [Portfolio](https://rashesh91.github.io)

I build the systems that train, serve, and operate AI models in production — spanning Kubernetes AI platforms, OpenStack private cloud, and VoIP telephony.

---

## Portfolio: 6 Projects

### 01 — Voice Agentic AI Platform
**[`ml-inference-gitops`](https://github.com/rashesh91/ml-inference-gitops)**

Real-time voice assistant on Kubernetes. Browser mic to audio response in under 4 seconds — Whisper transcribes speech, Mistral 7B reasons and calls tools (weather, search, calculator), Edge TTS speaks the answer back.

```
Browser mic → WebSocket → Whisper STT → Mistral 7B ReAct → Edge TTS → Audio response
```

**Stack:** Python · FastAPI · Kubernetes · ArgoCD · vLLM · Helm · GPU

---

### 02 — LLM Fine-Tuning & Evaluation Platform
**[`ml-training-platform`](https://github.com/rashesh91/ml-training-platform)**

Self-service LoRA fine-tuning — upload a dataset, get a fine-tuned model auto-evaluated and registered in MLflow. No ML engineer handholding. 5-step Argo Workflow DAG handles the full pipeline.

```
Dataset → MinIO → KubeRay LoRA training → MLflow eval → Staging → Production
```

**Stack:** Python · KubeRay · HuggingFace PEFT · MLflow · Argo Workflows · MinIO

---

### 03 — AI Platform Ops (Production Operations Layer)
**[`ai-platform-ops`](https://github.com/rashesh91/ai-platform-ops)**

Closes all production gaps in Projects 1 & 2. Five components that separate a demo from a system that runs at 3 AM without anyone watching.

| Component | Problem Solved |
|-----------|---------------|
| **Custom K8s Operator** (kopf) | MLflow model promotion → auto-patches voice-gateway within 30s |
| **OpenTelemetry + Grafana Tempo** | Waterfall traces show STT=400ms, LLM=2800ms, TTS=250ms |
| **KEDA Scale-to-Zero** | Fixed replicas waste GPU → ~60% cost savings on Ray workers |
| **Argo Rollouts Canary** | 10% → 50% → 100% with Prometheus analysis gates, auto-abort |
| **SLO Burn Rate Alerts** | Google SRE formula: fast burn 14×/1h, slow burn 6×/6h |

**Stack:** Python · kopf · OpenTelemetry · Grafana Tempo · KEDA · Argo Rollouts · PrometheusRules

---

### 04 — Production HA OpenStack with Kolla-Ansible
**[`openstack-kolla-deploy`](https://github.com/rashesh91/openstack-kolla-deploy)**

Full private cloud deployment: 3-node HA controller cluster (Keepalived VIP + HAProxy + MariaDB Galera + RabbitMQ), 4 compute nodes, 3-node Ceph storage backend. Automated deploy, post-deploy resource provisioning, and health-check scripts.

```
Bootstrap → Prechecks → Deploy → Post-Deploy → HA OpenStack (Horizon + Keystone + Nova + Neutron + Ceph)
```

**Stack:** OpenStack 2024.1 (Caracal) · Kolla-Ansible · Ceph · HAProxy · Keepalived · Neutron OVS · Bash

---

### 05 — GPU Cloud for AI/ML Workloads
**[`openstack-ai-infra`](https://github.com/rashesh91/openstack-ai-infra)**

Extends OpenStack for self-service GPU compute. NVIDIA A100/T4 PCI passthrough via vfio-pci, Ironic bare-metal provisioning for workloads that can't tolerate hypervisor overhead, VLAN 200 AI training network (MTU 9000 for NCCL AllReduce), and Heat templates for one-command GPU instance provisioning with CUDA + PyTorch.

| Resource | Detail |
|----------|--------|
| GPU flavors | `gpu.a100.1x`, `gpu.a100.2x`, `gpu.t4.1x`, `baremetal.gpu.a100` |
| Passthrough | NVIDIA A100 (10de:20b5), T4 (10de:1eb8) via vfio-pci |
| Bare metal | Ironic IPMI driver, iPXE boot, hardware inspection |
| AI network | VLAN 200, 10.200.0.0/16, MTU 9000 |

**Stack:** OpenStack · Ironic · NVIDIA GPU · VFIO · Heat · Ansible · CUDA

---

### 06 — AI Phone Agent — FreeSWITCH + LLM
**[`freeswitch-ai-agent`](https://github.com/rashesh91/freeswitch-ai-agent)**

Bridges a SIP telephone network to the Kubernetes AI voice pipeline. A caller dials a number, FreeSWITCH handles SIP/RTP, `mod_audio_stream` pipes audio in real time to a Python bridge, WebRTC VAD detects speech end, and the same Whisper → Mistral → TTS pipeline answers the call. The AI answers phone calls.

```
SIP Caller → FreeSWITCH → mod_audio_stream → WebRTC VAD → Whisper STT → Mistral ReAct → TTS → Caller
```

**Stack:** FreeSWITCH · mod_audio_stream · SIP/RTP · WebRTC VAD · Python · FastAPI · Kubernetes

---

## How the Projects Connect

```
                    ┌─────────────────────────────────┐
                    │     ml-training-platform         │
                    │  LoRA fine-tune → MLflow register │
                    └──────────────┬──────────────────┘
                     model promoted│
                                   ▼
  ┌────────────────┐   ai-platform-ops operator   ┌──────────────────────┐
  │ freeswitch-    │ ──────────────────────────▶  │  ml-inference-gitops │
  │ ai-agent       │        auto-deploys           │  voice-gateway       │
  │ (phone channel)│ ◀─────────────────────────── │  (browser channel)   │
  └────────────────┘    same STT + LLM + TTS       └──────────────────────┘
                                   │
                    ┌──────────────▼──────────────┐
                    │       ai-platform-ops        │
                    │  OTel traces · KEDA scaling  │
                    │  Argo Rollouts · SLO alerts  │
                    └─────────────────────────────┘

  ┌──────────────────────────┐   ┌──────────────────────────┐
  │  openstack-kolla-deploy  │   │   openstack-ai-infra     │
  │  Private cloud HA stack  │──▶│   GPU compute layer      │
  │  (runs the K8s cluster)  │   │   PCI passthrough + Ironic│
  └──────────────────────────┘   └──────────────────────────┘
```

---

## Skills

**Kubernetes & GitOps** — Custom operators (kopf), KEDA, Argo Rollouts, ArgoCD App of Apps, Helm, sync waves

**Observability** — OpenTelemetry, Grafana Tempo, Prometheus, SLO burn rate alerting (Google SRE)

**ML Infrastructure** — KubeRay, vLLM, HuggingFace PEFT LoRA, MLflow, Argo Workflows, MinIO

**OpenStack & Cloud** — Kolla-Ansible, Ceph, Ironic, Neutron OVS, Heat, Keepalived + HAProxy HA

**Telephony & VoIP** — FreeSWITCH, Asterisk, SIP/RTP, mod_audio_stream, ESL, WebRTC VAD

**Languages** — Python · Bash · YAML

---

## Quick Start

| Project | Run locally with |
|---------|-----------------|
| Voice Platform | `docker compose up` — 8 GB RAM, Ollama replaces GPU |
| Training Platform | `docker compose up` — 8 GB RAM |
| AI Platform Ops | `docker compose -f docker-compose.dev.yaml up` — OTel + Tempo + Grafana |
| FreeSWITCH AI Agent | `docker compose up` — register Zoiper softphone, dial 1000 |
| Full K8s cluster | `kind` — 16 GB RAM, 8 CPU, GPU optional |
