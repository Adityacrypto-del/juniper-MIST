# Autonomous AI SRE Agent — Project Explanation

## 1. Project Overview

**Autonomous AI SRE Agent** is a closed-loop AIOps/SRE system designed to automatically detect, investigate, diagnose, remediate, and verify infrastructure failures across a deeply nested cloud environment.

The system combines:

- **Large Language Models (LLMs)** for reasoning and tool selection
- **LangChain / LangGraph ReAct agents** for agentic execution
- **Spatio-Temporal Graph Neural Networks (ST-GNNs)** for mathematical root-cause analysis
- **Prometheus** for infrastructure telemetry
- **OpenStack** as the cloud/virtualization layer
- **Kubernetes** as the application/edge layer
- **Juniper Mist** as the network/device layer
- **Chaos Engineering** for controlled fault injection
- **FastAPI** for alert ingestion and service APIs
- **Incident journaling** for auditability

The key idea is a **Dual-Brain architecture**:

> **ST-GNN = mathematical critic**  
> **LLM ReAct Agent = reasoning and execution actor**

The goal is not simply to tell an engineer what went wrong. The agent is designed to investigate the infrastructure and execute remediation actions itself, followed by verification.

---

## 2. Problem Statement

Modern infrastructure is distributed across multiple layers.

A single visible application failure may actually originate somewhere else:

```text
Physical Network
      ↓
Juniper Mist
      ↓
OpenStack
      ↓
Virtual Machines
      ↓
Kubernetes
      ↓
Pods / Services
      ↓
Application
```

For example:

```text
OpenStack Compute CPU saturation
          ↓
Kubernetes worker gets fewer CPU cycles
          ↓
Pods become Pending / slow
          ↓
Application latency increases
          ↓
Prometheus raises an alert
```

A traditional monitoring system may only report:

> "Application latency is high."

The Autonomous SRE Agent attempts to determine:

> "Why is application latency high, which infrastructure component is the root cause, what evidence supports that diagnosis, what action should be taken, and did the action actually restore the system?"

---

## 3. Main Objective

The project aims to implement a **closed-loop autonomous SRE workflow**:

```text
Sense
  ↓
Detect
  ↓
Investigate
  ↓
Diagnose
  ↓
Plan
  ↓
Act
  ↓
Verify
  ↓
Learn / Journal
```

This is fundamentally different from a simple chatbot.

The agent has access to real infrastructure tools and can execute operations against the environment.

---

# 4. High-Level Architecture

```text
                    ┌──────────────────────────┐
                    │     Infrastructure       │
                    │                          │
                    │ OpenStack / K8s / Mist  │
                    └────────────┬─────────────┘
                                 │
                                 ▼
                    ┌──────────────────────────┐
                    │       Telemetry           │
                    │ Prometheus / Mist APIs   │
                    └────────────┬─────────────┘
                                 │
                    ┌────────────┴─────────────┐
                    │                          │
                    ▼                          ▼
             ┌──────────────┐          ┌───────────────┐
             │ Alertmanager │          │ ST-GNN Critic │
             └──────┬───────┘          └───────┬───────┘
                    │                          │
                    │                    RCA prediction
                    ▼                          │
             ┌─────────────────────────────────┐
             │       AI SRE Agent              │
             │   LangChain / LangGraph ReAct   │
             └───────────────┬─────────────────┘
                             │
                    ┌────────┴─────────┐
                    │                  │
                    ▼                  ▼
             Investigation        Remediation
               Tools                 Tools
                    │                  │
                    └────────┬─────────┘
                             ▼
                    ┌─────────────────┐
                    │   Verification  │
                    └────────┬────────┘
                             ▼
                    ┌─────────────────┐
                    │ Incident Journal│
                    └─────────────────┘
```

---

# 5. Dual-Brain Architecture

## 5.1 Brain 1 — ST-GNN Critic

The ST-GNN provides the mathematical RCA component.

It combines:

- Graph Convolutional Networks (GCN)
- Long Short-Term Memory (LSTM)
- temporal windows
- normalized telemetry
- graph relationships between infrastructure components

The model receives telemetry over a rolling temporal window.

Conceptually:

```text
Telemetry t-4
Telemetry t-3
Telemetry t-2
Telemetry t-1
Telemetry t
      ↓
Temporal window
      ↓
Graph representation
      ↓
GCN
      ↓
LSTM
      ↓
Fault probabilities
```

The repository uses a 5-tick temporal window.

The documented training pipeline uses:

- 54 metric columns
- 4 node groups
- 23-dimensional padded node features
- sliding windows
- StandardScaler
- LabelEncoder
- GCN layers
- LSTM
- NLLLoss
- Adam optimizer

The project documentation reports approximately **99.2% test accuracy** for the evaluated ST-GNN experiment.

Important interview point:

> High accuracy on synthetic or controlled data does not automatically mean the same performance will occur on unseen production infrastructure.

---

# 6. Brain 2 — LLM ReAct Agent

The second brain is an LLM-powered agent.

It uses a **ReAct-style loop**:

```text
Thought
   ↓
Action
   ↓
Observation
   ↓
Reasoning
   ↓
Action
   ↓
Observation
   ↓
Final decision
```

The agent does not need to guess blindly.

It can call infrastructure tools to obtain evidence.

---

# 7. LLM Provider Architecture

The code implements a provider-routing strategy.

The documented provider chain is:

```text
Cerebras
   ↓
OpenRouter
   ↓
Groq
```

For uncertain or complex responses, the system can escalate to:

```text
Gemini
```

The routing logic checks for uncertainty/complexity indicators such as:

- unclear
- uncertain
- cascading
- cross-layer
- multiple failures
- inconclusive

This creates a basic model-routing architecture:

```text
Alert
  ↓
Fast LLM
  ↓
Is analysis uncertain?
  ├── No → Continue
  └── Yes
        ↓
   Gemini reasoning
```

The motivation is to balance latency, availability, and reasoning depth.

---

# 8. Infrastructure Tools

The agent has tools that act as its operational "hands."

## OpenStack

The agent can execute OpenStack CLI commands.

Example:

```text
openstack server list
```

Used for:

- VM inspection
- infrastructure diagnosis
- cloud-level remediation

## Kubernetes

The agent can execute `kubectl` commands through an SSH path into the nested Kubernetes environment.

Example:

```text
kubectl get pods -A
```

Used for:

- pod status
- deployment inspection
- Kubernetes remediation

## Shell

The agent can execute host-level diagnostic commands.

Examples:

```text
top
df -h
ps
systemctl status
```

## Prometheus

The agent can execute PromQL queries.

Examples include:

```text
up
```

and CPU utilization calculations based on:

```text
node_cpu_seconds_total
```

Prometheus provides quantitative evidence rather than relying only on LLM interpretation.

---

# 9. Juniper Mist Integration

The system also integrates with Juniper Mist APIs.

It can retrieve:

- device inventory
- active alarms
- SLE metrics
- Marvis recommendations

It can also perform actions such as:

```text
restart device
bounce switch port
```

This gives the agent visibility into the physical/network layer.

---

# 10. Three Alert Sources

The project processes incidents from multiple channels.

## Channel 1 — Prometheus Alertmanager

Alertmanager sends a webhook:

```text
Prometheus
    ↓
Alertmanager
    ↓
POST /alert
    ↓
AI Agent
```

## Channel 2 — Kubernetes Prometheus Poller

The agent periodically checks Kubernetes Prometheus for firing alerts.

The documented default polling interval is 60 seconds.

## Channel 3 — Mist Alarm Poller

The agent periodically checks Juniper Mist for new unacknowledged alarms.

---

# 11. Proactive ST-GNN Detection

A particularly important feature is that the ST-GNN is not limited to reacting to alerts.

The agent also has a proactive telemetry poller.

Conceptually:

```text
Prometheus telemetry
       ↓
ST-GNN
       ↓
Predicted future fault
       ↓
Probability > threshold?
       ↓
PREEMPTIVE ALERT
       ↓
LLM Agent
       ↓
Preventive action
```

The implementation uses a documented threshold of approximately **0.70** for triggering a proactive alert.

This changes the architecture from:

```text
Detect failure → Repair
```

to:

```text
Predict failure → Prevent
```

---

# 12. Agent Decision Workflow

The system prompt defines five major phases.

## Step 1 — Investigate

Collect evidence.

Examples:

```text
Prometheus metrics
OpenStack status
Kubernetes pod status
Mist alarms
Mist SLE metrics
```

## Step 2 — Diagnose

Determine the likely root cause.

The agent should consider cascading failures.

Example:

```text
Host CPU saturation
       ↓
VM resource starvation
       ↓
K8s pod scheduling problem
       ↓
Application latency
```

## Step 3 — Plan

Create a specific remediation plan.

Example:

```text
1. Confirm CPU saturation.
2. Identify affected process.
3. Stop runaway workload.
4. Restart affected Kubernetes workload.
5. Verify CPU and pod health.
```

## Step 4 — Execute

Call the appropriate infrastructure tool.

## Step 5 — Verify

Re-query telemetry and infrastructure state.

A remediation is not considered successful merely because a command returned successfully.

---

# 13. Example Incident

Consider:

```text
Compute node CPU → 98%
```

The chain could become:

```text
Compute1 CPU saturation
        ↓
K8s worker resource starvation
        ↓
Video encoder pod cannot schedule
        ↓
Application latency increases
        ↓
Prometheus alert
        ↓
AI SRE Agent
```

The agent can investigate:

```text
Prometheus → CPU
Kubernetes → pod status
OpenStack → VM/node state
```

Then it determines the likely root cause and executes remediation.

After remediation:

```text
CPU ↓
Pods → Running
Latency ↓
Application → Healthy
```

The system records the incident.

---

# 14. Incident Journaling

The agent stores incident information as JSON Lines.

The journal can contain:

- timestamp
- alert
- analysis
- actions taken
- provider information

This is important for:

- auditing
- debugging
- post-incident analysis
- evaluating the agent
- reproducing failures

The API also exposes:

```text
GET /incidents
```

---

# 15. FastAPI Endpoints

The main service exposes endpoints including:

| Endpoint | Method | Purpose |
|---|---|---|
| `/alert` | POST | Prometheus Alertmanager webhook |
| `/test` | POST | Manually trigger an alert |
| `/health` | GET | Health/status information |
| `/incidents` | GET | Incident journal |

The service runs through Uvicorn/FastAPI.

---

# 16. Chaos Engineering

The project contains a substantial chaos-engineering subsystem.

Its purpose is to intentionally create failures and test whether the autonomous SRE system can recover.

Examples include:

- CPU saturation
- pod crashes
- network degradation
- disk/storage faults
- cascading failures

A simplified experiment is:

```text
Inject fault
    ↓
Observe telemetry
    ↓
Trigger AI
    ↓
Perform RCA
    ↓
Execute recovery
    ↓
Check application
    ↓
Record results
```

This is important because an autonomous remediation system needs testing against known failure conditions.

---

# 17. Synthetic Dataset Generation

The repository includes several dataset-generation scripts.

Examples include:

```text
synthetic_telemetry_generator.py
cascading_fault_synthesizer.py
advanced_fault_synthesizer.py
fast_dataset_generator.py
dataset_sampler.py
```

The cascading synthesizer models relationships between layers.

The documented examples include relationships such as:

```text
CPU
 ↓
OpenStack API latency
 ↓
Kubernetes API latency
 ↓
Application latency
```

The purpose is to generate training/evaluation data when large real production datasets are unavailable.

---

# 18. Model Files

The repository contains trained model artifacts including:

```text
gnn_rca_model.pt
stgnn_rca_model.pt
scaler.pkl
label_encoder.pkl
```

The runtime critic loads these artifacts to perform inference.

---

# 19. Deployment Architecture

The infrastructure is based around OpenStack and nested Kubernetes.

The repository provides Vagrant/Kolla-Ansible deployment material.

The documented environment includes:

```text
OpenStack Controller
        |
   ┌────┴────┐
Compute 1  Compute 2
        |
        ▼
Nested Kubernetes
        |
  ┌─────┼─────┐
Master Worker Worker
```

The AI agent runs on the OpenStack controller in the documented deployment architecture.

---

# 20. Why the Project Is Agentic AI

A normal LLM application:

```text
User question
    ↓
LLM
    ↓
Text answer
```

This project:

```text
Infrastructure event
       ↓
Agent
       ↓
Reason
       ↓
Choose tool
       ↓
Observe result
       ↓
Reason again
       ↓
Execute action
       ↓
Verify
```

The agent has:

- a goal
- tools
- state
- observations
- reasoning
- action capability
- feedback
- verification

Therefore it is an agentic workflow rather than just an LLM-powered dashboard.

---

# 21. Important Technologies to Know

For understanding this project, the major topics are:

### AI / ML

- LLMs
- ReAct agents
- LangChain
- LangGraph
- GCN
- GNN
- ST-GNN
- LSTM
- classification
- probability
- feature scaling
- LabelEncoder
- model inference

### AIOps / SRE

- SRE
- observability
- incident management
- RCA
- MTTR
- MTTD
- alerting
- self-healing
- chaos engineering
- fault injection

### Cloud / Infrastructure

- OpenStack
- KVM / virtualization concepts
- Kubernetes
- Docker/container concepts
- SSH
- Linux
- networking
- namespaces
- Neutron
- qrouter

### Observability

- Prometheus
- PromQL
- Alertmanager
- Grafana
- telemetry
- metrics

### Backend

- Python
- FastAPI
- Uvicorn
- REST APIs
- JSON
- environment variables
- subprocess
- threading

### Networking

- IP addressing
- routing
- NAT
- network namespaces
- switches
- access points
- gateways
- WAN/Wi-Fi/Wired SLE

---

# 22. One-Minute Interview Explanation

> "My project is an Autonomous AI SRE system for closed-loop infrastructure monitoring and self-healing. The environment contains multiple layers including OpenStack, nested Kubernetes, applications, and Juniper Mist networking. Prometheus and Mist provide telemetry and alerts. We use a Spatio-Temporal Graph Neural Network as a mathematical root-cause critic that learns relationships between infrastructure components and their temporal behavior. An LLM-based ReAct agent acts as the operational brain. It investigates alerts using tools such as Prometheus, OpenStack CLI, kubectl, and Mist APIs, determines the likely root cause, executes remediation, and then verifies whether the system recovered. The project also contains chaos-engineering scenarios and synthetic telemetry generation so that the RCA and recovery loop can be evaluated under controlled failures. The overall architecture follows Sense → Diagnose → Plan → Act → Verify, moving SRE operations from reactive alert handling toward autonomous and eventually proactive remediation."

---

# 23. Key Takeaway

The strongest way to understand this project is:

```text
                AUTONOMOUS SRE
                     │
       ┌─────────────┴─────────────┐
       │                           │
   ST-GNN Critic             LLM ReAct Agent
       │                           │
   "What failed?"             "What should I do?"
       │                           │
       └─────────────┬─────────────┘
                     │
                Tool Execution
                     │
                     ▼
              Infrastructure
                     │
                     ▼
                 Verification
```

The ST-GNN provides data-driven RCA while the agent provides flexible reasoning and operational execution.

That combination is the central engineering idea of the project.
