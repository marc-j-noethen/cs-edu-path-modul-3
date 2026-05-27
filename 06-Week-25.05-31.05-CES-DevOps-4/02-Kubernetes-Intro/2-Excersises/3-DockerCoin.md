# ☸️ DockerCoin (Multi-Service Cluster Orchestration)

**Course:** CES DevOps 4 – Kubernetes Intro | **Date:** 27 May 2026

---

## Task

**Objective:**  
Deploy and orchestrate a complete multi-component system (DockerCoin miner) on a local Kubernetes cluster: initialise the infrastructure, start all five microservices, and monitor mining operation via the web UI.

**Requirements:**

- Clone the lab repository from GitHub: `https://github.com/CyberstepsDE/DockerCoins`

- Create all five services (`rng`, `hasher`, `worker`, `webui`, `redis`) as deployments

- Expose `redis` as a ClusterIP service and `webui` as a NodePort service

- `kubectl get all` shows all pods, deployments, and services without errors

- Output:

    - `kubectl get all` lists all resources without `Error`/`CrashLoopBackOff`

    - Web UI accessible via `minikube service webui --url` and displays the mining graph

    - Own name visible in the web UI (configured via environment variable or ConfigMap)

---

## Solution

```bash
# Inputs
REPO_URL="https://github.com/CyberstepsDE/DockerCoins"
REPO_DIR="DockerCoins"
NAMESPACE="default"

# Main logic
# Step 1 – Clone repo & start cluster
if [ ! -d "$REPO_DIR" ]; then
    echo "ERROR: Repo not cloned. Run: git clone $REPO_URL"

# Step 2 – Deployments not yet present → deploy all services
elif ! kubectl get deployment worker &>/dev/null; then
    cd "$REPO_DIR"
    kubectl apply -f k8s/
    echo "All deployments and services from k8s/ applied."

# Step 3 – Deployments exist but pods not all ready
elif kubectl get pods | grep -vE "Running|Completed" | grep -q "0/"; then
    echo "Waiting for pod readiness..."
    kubectl wait --for=condition=Ready pods --all --timeout=120s
    kubectl get all

# Everything running → print status & open web UI
else
    echo "Cluster running. Full status:"
    kubectl get all
    minikube service webui
fi
```

**Alternative (compact):**

```bash
git clone https://github.com/CyberstepsDE/DockerCoins && cd DockerCoins \
  && minikube start \
  && kubectl apply -f k8s/ \
  && kubectl wait --for=condition=Ready pods --all --timeout=120s \
  && kubectl get all \
  && minikube service webui
```

---

## Tests

| Input 1 | Input 2 | Input 3 | Expected | Result | ✓ |
|---|---|---|---|---|---|
| `kubectl get all` | – | – | all pods `Running`, no `Error` | all `Running` | ✅ |
| `kubectl get deployments` | – | – | `rng`, `hasher`, `worker`, `webui` each `READY 1/1` | `READY 1/1` | ✅ |
| `minikube service webui --url` | Open URL in browser | – | Mining graph + name visible | Graph displayed | ✅ |

---

## Explanation / Concepts

| Concept | Description |
|---|---|
| Multi-Service Architecture | Multiple independent microservices (rng, hasher, worker, redis, webui) communicate via K8s-internal DNS names through ClusterIP services |
| `kubectl apply -f` | Declarative application of YAML manifests; K8s compares desired with current state and creates/updates resources accordingly |
| ClusterIP vs. NodePort | ClusterIP: only reachable cluster-internally (for Redis, internal APIs); NodePort: reachable from outside via node IP (for the web UI) |

---

## Rules / Logic

```
git clone <repo>             → source code & YAML manifests available locally
minikube start               → local cluster ready
kubectl apply -f k8s/        → create all manifests in one step
kubectl wait --for=Ready     → wait until all pods have started
kubectl get all              → full overview of pods/services/deployments
minikube service webui       → determine NodePort URL, open browser
```

---

## Notes

- **Concept:** Service discovery in Kubernetes – pods find each other via DNS names that K8s automatically creates for each service (e.g. `redis.default.svc.cluster.local`)

- **Syntax:** `kubectl apply -f <file-or-directory>` | `kubectl logs <pod-name> -f` (live log stream)

- **Order is important:**

    1. Deploy & expose `redis` first (all other services need Redis as a database)

    2. Then deploy `rng`, `hasher`, `worker` (worker consumes both services)

    3. Deploy & expose `webui` as NodePort last (only shows meaningful data once the worker is running)

- **Edge Cases:**

    - `worker` pod crashes immediately → Redis not reachable; check `kubectl logs worker-<hash>` for details

    - Web UI shows a flat line in the graph → worker deployment has 0 ready pods (e.g. image pull error)

    - `kubectl apply` reports `unchanged` → manifest already applied; no re-deployment necessary

- **Tip:** `kubectl logs -l app=worker -f` streams logs of all worker pods simultaneously via label selector – ideal for troubleshooting during scale-out

---

## Optional: Extensions

- Scale worker to multiple replicas and observe throughput changes in the web UI: `kubectl scale deployment worker --replicas=5`

- Validate mining output: check `kubectl logs -l app=worker` for hash rate and error rate

- Error handling: use `kubectl describe pod <name>` and `kubectl get events --sort-by='.lastTimestamp'` for cluster-wide event analysis

- Usability: inject your own name via ConfigMap (`kubectl create configmap miner-config --from-literal=MINER_NAME="YourName"`) and reference it in the webui deployment
