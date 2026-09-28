# Autonomous AI SRE Agent — Prerequisites & Interview Questions

## 1. Prerequisites

To understand, run, explain, or defend this project effectively, prepare the following topics.

---

# 2. Programming Prerequisites

## Python

Know:

- functions
- classes
- decorators
- exception handling
- dictionaries/lists
- JSON
- file handling
- environment variables
- subprocess
- threading
- HTTP requests
- async basics
- type hints

Important project concepts:

```python
@tool
def run_shell_command(command: str):
    ...
```

and:

```python
subprocess.run(...)
```

You should understand what decorators and subprocess execution do.

### Possible questions

1. Why was Python used?
2. What is a decorator?
3. What does `@tool` do?
4. Why use `subprocess`?
5. Difference between synchronous and asynchronous execution?
6. Why are background threads used?
7. How are exceptions handled?
8. What is an environment variable?
9. Why should API keys not be hardcoded?
10. What is JSONL and why is it useful for incident logs?

---

# 3. Linux Prerequisites

Know:

- processes
- CPU/memory/disk
- shell commands
- SSH
- systemd
- permissions
- networking commands
- namespaces
- `ps`
- `top`
- `df`
- `curl`
- `grep`
- `kill`
- `pkill`

### Questions

1. What is a Linux process?
2. What is the difference between a process and a thread?
3. What does `kill` do?
4. What is `systemd`?
5. Why is the agent deployed as a systemd service?
6. What happens if the agent process crashes?
7. What is SSH?
8. Why does the project use SSH to reach Kubernetes?
9. What is a Linux network namespace?
10. What is `ip netns exec`?
11. Why is a qrouter namespace used?

---

# 4. Networking Prerequisites

Understand:

- IP addresses
- subnet
- gateway
- routing
- NAT
- TCP/UDP
- HTTP
- REST APIs
- ports
- DNS
- network namespaces
- virtual networks
- switches
- access points
- firewalls

You should understand the difference between:

```text
Application
    ↓
Kubernetes Service
    ↓
Pod
    ↓
Kubernetes Node
    ↓
VM
    ↓
OpenStack Network
    ↓
Physical Network
```

### Questions

1. What is an IP address?
2. What is a subnet?
3. What is a gateway?
4. What is NAT?
5. What is a port?
6. What is the difference between TCP and UDP?
7. What is REST?
8. What is an API endpoint?
9. What is a network namespace?
10. What is a virtual network?
11. What is a qrouter in OpenStack Neutron?
12. Why does the Kubernetes traffic need to pass through the qrouter namespace?
13. What happens if the network layer fails?
14. How can a network fault cascade into Kubernetes?
15. How can you distinguish network failure from application failure?

---

# 5. OpenStack Prerequisites

Know the major OpenStack services:

- Nova
- Neutron
- Keystone
- Cinder
- Glance
- Horizon
- HAProxy

Understand:

```text
Controller
Compute Nodes
Network
VMs
```

### Questions

1. What is OpenStack?
2. Why is OpenStack used in the project?
3. What is Nova?
4. What is Neutron?
5. What is Keystone?
6. What is Cinder?
7. What is Glance?
8. What is Horizon?
9. What is a compute node?
10. What is a controller node?
11. What is a floating IP?
12. What is a virtual machine?
13. How can OpenStack CPU exhaustion affect Kubernetes?
14. How would you investigate an unhealthy OpenStack VM?
15. Why use Kolla-Ansible?

---

# 6. Kubernetes Prerequisites

Know:

- cluster
- control plane
- worker node
- pod
- deployment
- service
- replica
- namespace
- scheduler
- kubelet
- API server
- CrashLoopBackOff
- Pending
- readiness/liveness probes

Important commands:

```bash
kubectl get pods -A
kubectl get nodes
kubectl describe pod <pod>
kubectl get deployments
kubectl rollout restart deployment <name>
kubectl scale deployment <name> --replicas=2
```

### Questions

1. What is Kubernetes?
2. What is a pod?
3. What is a deployment?
4. What is a service?
5. What is a worker node?
6. What is the Kubernetes control plane?
7. What does the scheduler do?
8. What is CrashLoopBackOff?
9. Why can a pod become Pending?
10. How do you debug a failed pod?
11. How can infrastructure problems cause pod failures?
12. How would the agent recover a failed deployment?
13. What is the difference between restarting a pod and restarting a deployment?
14. What is horizontal scaling?
15. How does Kubernetes provide resilience?

---

# 7. Docker / Container Prerequisites

Know:

- container
- image
- Dockerfile
- container runtime
- registry
- port mapping
- volume
- environment variables

### Questions

1. What is a container?
2. Container vs virtual machine?
3. Why are containers useful?
4. What is a Docker image?
5. What is a Dockerfile?
6. What is container orchestration?
7. Why use Kubernetes instead of manually running containers?
8. How does a container failure affect an application?

---

# 8. Observability Prerequisites

This is one of the most important sections.

Understand the three major observability signals:

```text
Metrics
Logs
Traces
```

This project primarily relies on **metrics**, especially Prometheus.

Know:

- Prometheus
- PromQL
- Alertmanager
- Grafana
- exporters
- scraping
- time series
- labels

### Questions

1. What is observability?
2. Monitoring vs observability?
3. What are metrics?
4. What is Prometheus?
5. How does Prometheus collect metrics?
6. What is PromQL?
7. What is Alertmanager?
8. Why is Alertmanager needed?
9. What is a time-series database?
10. What is a Prometheus label?
11. How do you calculate CPU utilization using PromQL?
12. What happens when a Prometheus alert fires?
13. Why does the project use both polling and webhooks?
14. What is the difference between an alert and telemetry?
15. How can telemetry be used for predictive maintenance?

---

# 9. SRE Prerequisites

Know the core SRE concepts:

- reliability
- availability
- latency
- incident
- incident response
- RCA
- MTTR
- MTTD
- SLO
- SLA
- SLI
- error budget
- automation
- self-healing

### Questions

1. What is SRE?
2. SRE vs DevOps?
3. What is an incident?
4. What is RCA?
5. What is MTTD?
6. What is MTTR?
7. Why is reducing MTTR important?
8. What is an SLI?
9. What is an SLO?
10. What is an SLA?
11. What is an error budget?
12. What is self-healing infrastructure?
13. What is autonomous remediation?
14. How does this project reduce manual SRE work?
15. What are the risks of autonomous remediation?

---

# 10. AIOps Prerequisites

Understand:

```text
Monitoring
   ↓
Data collection
   ↓
Anomaly detection
   ↓
RCA
   ↓
Prediction
   ↓
Remediation
```

### Questions

1. What is AIOps?
2. How is AIOps different from traditional monitoring?
3. What is automated RCA?
4. What is predictive AIOps?
5. What is event correlation?
6. What is anomaly detection?
7. Why are graph models useful in AIOps?
8. How can AIOps reduce alert fatigue?
9. What is closed-loop automation?

---

# 11. LLM Prerequisites

Understand:

- LLM
- tokens
- context
- temperature
- inference
- prompting
- tool calling
- function calling
- structured output
- hallucination
- model routing

### Questions

1. What is an LLM?
2. Why use an LLM in an SRE system?
3. Why not use only an LLM?
4. What is hallucination?
5. Why is hallucination dangerous for infrastructure automation?
6. How does tool calling work?
7. What is function calling?
8. What is prompt engineering?
9. Why use multiple LLM providers?
10. What is model fallback?
11. What is model escalation?
12. Why would a fast model be used for normal incidents and a deeper model for complex incidents?

---

# 12. ReAct Agent Prerequisites

This is one of the most important interview areas.

ReAct means:

> **Reason + Act**

The conceptual loop is:

```text
Thought
  ↓
Action
  ↓
Observation
  ↓
Thought
  ↓
Action
  ↓
Observation
  ↓
Final Answer
```

### Questions

1. What is a ReAct agent?
2. Why use ReAct?
3. Agent vs chatbot?
4. What is a tool?
5. How does the agent choose a tool?
6. What is an observation?
7. What is an agent loop?
8. What happens if a tool fails?
9. How do you prevent infinite loops?
10. Why is a maximum iteration limit useful?
11. Why should the agent investigate before executing?
12. How do you verify an autonomous action?
13. What is the difference between planning and acting?

---

# 13. LangChain / LangGraph

The project uses LangChain/LangGraph components to build the agent.

### Questions

1. What is LangChain?
2. What is LangGraph?
3. Why use an agent framework?
4. What is a LangChain tool?
5. What is `create_react_agent`?
6. How are tools exposed to an LLM?
7. How does the framework maintain the agent loop?
8. Why use LangGraph instead of writing the loop completely manually?
9. What are the limitations of agent frameworks?

---

# 14. GNN Prerequisites

Understand:

- graph
- node
- edge
- adjacency matrix
- node features
- message passing
- GCN
- graph classification

Example infrastructure graph:

```text
OpenStack Host
      │
      ▼
     VM
      │
      ▼
 K8s Worker
      │
      ▼
    Pod
      │
      ▼
Application
```

### Questions

1. What is a graph neural network?
2. Why use a GNN for infrastructure?
3. What is a node?
4. What is an edge?
5. What is an adjacency matrix?
6. What is message passing?
7. What is GCN?
8. Why might a GNN be better than a normal tabular model for infrastructure RCA?
9. How does topology information help RCA?
10. What does the GCN learn?

---

# 15. ST-GNN Prerequisites

ST-GNN means:

> **Spatio-Temporal Graph Neural Network**

It combines:

```text
Spatial relationships
       +
Temporal relationships
       ↓
ST-GNN
```

In this project:

```text
Infrastructure graph
       +
5 telemetry time steps
       ↓
GCN
       ↓
LSTM
       ↓
Fault classification
```

### Questions

1. What is an ST-GNN?
2. Why is time important in infrastructure failures?
3. Why is graph structure important?
4. Why combine GCN and LSTM?
5. What does the GCN capture?
6. What does the LSTM capture?
7. Why use a rolling window?
8. Why use five time steps?
9. What happens if the window is too small?
10. What happens if the window is too large?
11. What is the model's output?
12. How is the predicted fault converted into a probability?
13. Why is StandardScaler used?
14. Why is LabelEncoder used?
15. How would you evaluate an ST-GNN beyond accuracy?

---

# 16. ML Prerequisites

Know:

- supervised learning
- classification
- train/test split
- features
- labels
- normalization
- StandardScaler
- overfitting
- underfitting
- precision
- recall
- F1-score
- confusion matrix
- cross-validation

### Questions

1. Is the ST-GNN supervised or unsupervised?
2. What are the features?
3. What are the labels?
4. Why normalize features?
5. What is overfitting?
6. How can synthetic data cause overfitting?
7. Why is accuracy potentially misleading?
8. Why use precision and recall?
9. What is a confusion matrix?
10. How would you validate the model on real infrastructure data?
11. How would you handle class imbalance?
12. How would you detect data drift?

---

# 17. Synthetic Data Prerequisites

The repository contains synthetic telemetry and fault-generation code.

### Questions

1. Why generate synthetic telemetry?
2. What are the advantages of synthetic data?
3. What are the disadvantages?
4. What is a cascading fault?
5. How would you simulate CPU exhaustion?
6. How would you simulate network degradation?
7. How can synthetic data create unrealistic correlations?
8. How would you combine synthetic and real telemetry?
9. How would you prove that a model trained on synthetic data generalizes to production?

---

# 18. Chaos Engineering

Understand:

- fault injection
- blast radius
- controlled experiments
- recovery
- resilience
- failure scenarios

### Questions

1. What is chaos engineering?
2. Why is chaos engineering used here?
3. Why intentionally break infrastructure?
4. What is a fault injection?
5. What is a cascading failure?
6. How do you safely perform chaos experiments?
7. How do you measure recovery?
8. What happens if the recovery agent itself fails?
9. How would you prevent a chaos experiment from affecting production?

---

# 19. Juniper Mist / Network Layer

Understand:

- access point
- switch
- gateway
- WAN
- Wi-Fi
- Wired
- SLE
- alarms
- device inventory
- REST API
- Marvis

### Questions

1. Why integrate Juniper Mist?
2. What information does device inventory provide?
3. What are SLE metrics?
4. What is Marvis?
5. How can a network fault affect Kubernetes?
6. Why would the agent need network-layer telemetry?
7. What does a port bounce do?
8. When would restarting a device be dangerous?
9. Why should the agent investigate before restarting a device?

---

# 20. Security Questions

This is a very important interview section because the agent can execute commands.

### Questions

1. What are the security risks of giving an LLM shell access?
2. How do you prevent destructive commands?
3. How should secrets be stored?
4. Why use environment variables?
5. How would you implement RBAC?
6. How would you restrict the agent's permissions?
7. Should every action require human approval?
8. How would you implement approval gates?
9. How would you audit agent actions?
10. How would you prevent prompt injection?
11. What happens if malicious telemetry contains instructions?
12. How would you sandbox tool execution?
13. How would you limit the blast radius of an autonomous agent?
14. How would you secure SSH credentials?
15. How would you rotate API tokens?

---

# 21. Architecture Questions

### Questions

1. Explain the complete architecture.
2. Why did you choose a Dual-Brain design?
3. Why not use only an LLM?
4. Why not use only the GNN?
5. How do the GNN and LLM complement each other?
6. Where does telemetry enter the system?
7. Where does RCA happen?
8. Where does remediation happen?
9. How is verification performed?
10. How does the system handle cross-layer failures?
11. What happens when the LLM provider is unavailable?
12. What happens when Prometheus is unavailable?
13. What happens when the GNN fails?
14. What happens when remediation fails?
15. How do you prevent endless remediation loops?

---

# 22. Deep Technical Questions

## Q1. Why not use an LLM alone for RCA?

A strong answer:

> LLMs are flexible reasoning systems but can hallucinate or misinterpret numerical telemetry. The ST-GNN provides a learned mathematical signal based on infrastructure topology and temporal telemetry. The LLM can then use this signal together with live operational evidence to make a more grounded decision.

---

## Q2. Why use GNN instead of XGBoost?

Possible answer:

> Infrastructure is naturally represented as a graph because components have relationships. XGBoost can work very well on tabular telemetry but does not inherently model topology. A GNN can propagate information between connected infrastructure nodes and therefore capture relationships that are important for root-cause analysis.

---

## Q3. Why combine GCN with LSTM?

Answer:

> GCN captures spatial relationships between infrastructure components, while LSTM captures how telemetry evolves over time. Infrastructure failures are often both topological and temporal, so combining the two helps model cascading behavior.

---

## Q4. What is the role of the LLM?

Answer:

> The LLM is responsible for flexible investigation, reasoning, tool selection, planning, and execution. It can inspect metrics, query Kubernetes, inspect OpenStack, inspect network alarms, and choose appropriate remediation tools.

---

## Q5. What is the role of the ST-GNN?

Answer:

> It acts as a mathematical RCA and prediction component. It processes a temporal sequence of telemetry over an infrastructure graph and produces fault probabilities.

---

## Q6. Why do we need verification?

Answer:

> A successful command does not mean the infrastructure has recovered. Verification checks whether the original symptoms actually disappeared, such as CPU returning to normal, pods becoming healthy, and application latency decreasing.

---

# 23. Scenario-Based Questions

## Scenario 1 — High CPU

**Question:**

A Kubernetes application suddenly becomes slow. How would your system investigate it?

Expected reasoning:

```text
Alert
 ↓
Prometheus
 ↓
Check application latency
 ↓
Check K8s pods
 ↓
Check worker resources
 ↓
Check OpenStack host
 ↓
Check CPU
 ↓
ST-GNN RCA
 ↓
LLM remediation
 ↓
Verify
```

---

## Scenario 2 — Pod CrashLoopBackOff

**Question:**

A pod is repeatedly restarting. What would you check?

Possible investigation:

```text
kubectl get pods
kubectl describe pod
kubectl logs
Prometheus metrics
Node health
CPU / memory
Network
Underlying VM
```

---

## Scenario 3 — Network Failure

**Question:**

A Kubernetes worker suddenly becomes unreachable.

Investigate:

```text
Kubernetes node status
        ↓
VM status
        ↓
OpenStack network
        ↓
Neutron
        ↓
Mist gateway/switch
        ↓
Mist alarms
        ↓
SLE metrics
```

---

## Scenario 4 — GNN says one thing, LLM says another

**Question:**

What happens if the ST-GNN predicts CPU exhaustion but the LLM believes the network is the root cause?

A good answer should discuss:

- gathering additional evidence
- checking telemetry
- comparing independent signals
- avoiding blind remediation
- confidence thresholds
- human approval for high-risk actions
- recording disagreement for evaluation

The system should not blindly trust either component.

---

## Scenario 5 — LLM makes a wrong decision

**Question:**

How do you make autonomous remediation safe?

Discuss:

```text
RBAC
Least privilege
Tool allowlists
Command validation
Approval gates
Dry runs
Blast-radius limits
Rate limits
Verification
Rollback
Audit logs
```

---

# 24. Performance Questions

1. What is the inference latency of the ST-GNN?
2. How fast can an LLM respond?
3. What is MTTD?
4. What is MTTR?
5. How would you reduce agent latency?
6. Why use a fast model first?
7. Why use model fallback?
8. How would you scale the agent?
9. What happens if 100 alerts arrive simultaneously?
10. How would you implement a queue?

---

# 25. Evaluation Questions

The project contains evaluation and execution logs.

Possible evaluation metrics include:

- RCA accuracy
- precision
- recall
- F1-score
- confusion matrix
- MTTD
- MTTR
- recovery success rate
- false positive rate
- false negative rate
- agent tool success rate

### Questions

1. How did you evaluate the GNN?
2. How did you evaluate the agent?
3. How do you measure RCA accuracy?
4. How do you measure recovery success?
5. What is MTTD?
6. What is MTTR?
7. What is the difference between model accuracy and system reliability?
8. How would you perform an ablation study?
9. How would you compare:
   - baseline
   - GNN
   - ST-GNN
   - LLM-only
   - ST-GNN + LLM?

---

# 26. Design Improvement Questions

Interviewers may ask what you would improve.

Good areas to discuss:

### 1. Better governance

Add:

```text
Risk classifier
 ↓
Low risk → autonomous
Medium risk → approval
High risk → human only
```

### 2. Better memory

Store:

- previous incidents
- successful remediation
- failed remediation
- infrastructure topology
- known failure patterns

### 3. Better observability

Add:

- logs
- traces
- distributed tracing
- service dependency graphs

### 4. Better model validation

Use:

- real production telemetry
- temporal validation
- cross-environment validation
- drift monitoring

### 5. Better security

Add:

- RBAC
- command allowlists
- sandboxing
- signed actions
- approval workflows

### 6. Better agent architecture

Separate:

```text
Planner
   ↓
Investigator
   ↓
RCA
   ↓
Remediation
   ↓
Verifier
```

rather than allowing one general agent to perform everything.

---

# 27. Very Likely Interview Questions

Prepare these especially well:

1. Explain your project in 60 seconds.
2. What problem does it solve?
3. What is SRE?
4. What is AIOps?
5. Why did you use an LLM?
6. Why did you use an ST-GNN?
7. Why combine GCN and LSTM?
8. Explain ReAct.
9. What tools can your agent use?
10. How does the agent receive alerts?
11. How does Prometheus work?
12. What is Alertmanager?
13. How does Kubernetes fit into the architecture?
14. How does OpenStack fit into the architecture?
15. How can a failure cascade across layers?
16. How does the system determine root cause?
17. How does the system perform remediation?
18. How does it verify recovery?
19. What happens if the LLM is wrong?
20. What happens if the GNN is wrong?
21. What happens if an API is unavailable?
22. Why use multiple LLM providers?
23. Why use synthetic data?
24. What are the limitations?
25. What are the security risks?
26. How would you deploy this in production?
27. How would you prevent the agent from executing dangerous commands?
28. How would you evaluate the system?
29. What would you improve?
30. What part of the project did you personally implement?

---

# 28. Topics Checklist

Use this as a final preparation checklist.

### AI

- [ ] LLM
- [ ] Prompt engineering
- [ ] Tool calling
- [ ] Function calling
- [ ] ReAct
- [ ] Agentic AI
- [ ] Model routing
- [ ] Hallucination

### ML

- [ ] Classification
- [ ] Feature scaling
- [ ] StandardScaler
- [ ] LabelEncoder
- [ ] Train/test split
- [ ] Accuracy
- [ ] Precision
- [ ] Recall
- [ ] F1
- [ ] Confusion matrix
- [ ] Overfitting

### Deep Learning

- [ ] GNN
- [ ] GCN
- [ ] Message passing
- [ ] LSTM
- [ ] ST-GNN
- [ ] Temporal window
- [ ] Graph representation

### Agent Frameworks

- [ ] LangChain
- [ ] LangGraph
- [ ] Tools
- [ ] AgentExecutor / agent loop concepts
- [ ] State
- [ ] Observations

### SRE

- [ ] SRE
- [ ] AIOps
- [ ] RCA
- [ ] MTTD
- [ ] MTTR
- [ ] SLI
- [ ] SLO
- [ ] SLA
- [ ] Error budget
- [ ] Self-healing

### Infrastructure

- [ ] Linux
- [ ] OpenStack
- [ ] Kubernetes
- [ ] Docker
- [ ] SSH
- [ ] systemd
- [ ] Networking
- [ ] Network namespaces

### Observability

- [ ] Prometheus
- [ ] PromQL
- [ ] Alertmanager
- [ ] Grafana
- [ ] Metrics
- [ ] Logs
- [ ] Traces

### Security

- [ ] RBAC
- [ ] Least privilege
- [ ] Secrets
- [ ] API security
- [ ] Prompt injection
- [ ] Command injection
- [ ] Sandboxing
- [ ] Approval workflows

### Reliability

- [ ] Chaos engineering
- [ ] Fault injection
- [ ] Cascading failures
- [ ] Recovery
- [ ] Verification
- [ ] Rollback
- [ ] Blast radius

---

# 29. Final Interview Strategy

Do not memorize only the technology names.

Be able to explain this chain:

```text
Why?
 ↓
Problem
 ↓
Architecture
 ↓
Telemetry
 ↓
ST-GNN
 ↓
LLM Agent
 ↓
Tools
 ↓
Remediation
 ↓
Verification
 ↓
Evaluation
 ↓
Security
```

The strongest technical explanation connects every technology to a reason.

For example:

> "We use Prometheus because the agent needs real-time quantitative telemetry."

> "We use ST-GNN because infrastructure has both topology and temporal behavior."

> "We use an LLM ReAct agent because remediation requires flexible investigation and tool selection."

> "We use chaos engineering because autonomous recovery needs controlled failure scenarios for validation."

> "We verify after remediation because successful command execution does not guarantee service recovery."

That reasoning chain is more important in an interview than simply listing frameworks.
