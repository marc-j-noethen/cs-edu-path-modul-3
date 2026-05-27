# ☸️ The Miracle (Kubernetes Core Management – 6 Modules)

**Course:** CES DevOps 4 – Kubernetes Intro | **Date:** 27 May 2026

---

## Task

**Objective:**  
Master the six core pillars of Kubernetes management: create a cluster, deploy an application, inspect pod state, establish network access, scale horizontally, and perform a rolling update.

**Requirements:**

- Complete all 6 modules of the official Kubernetes Basics tutorial in full

- Verify the cluster state via `kubectl` after each module

- Successfully demonstrate scaling from 1 to at least 4 replicas

- Perform a rolling update to a new image version without downtime

- Output:

    - `kubectl get pods` shows all pods with status `Running`

    - `kubectl get deployments` shows `READY 4/4` (after scaling)

    - `kubectl rollout status deployment/kubernetes-bootcamp` shows `successfully rolled out`

---

## Solution

```bash
# Inputs
CLUSTER="minikube"
APP="kubernetes-bootcamp"
IMAGE_V1="docker.io/jocatalin/kubernetes-bootcamp:v1"
IMAGE_V2="docker.io/jocatalin/kubernetes-bootcamp:v2"

# Main logic
# Module 1 – Create cluster
if ! minikube status | grep -q "Running"; then
    echo "ERROR: Minikube not started – run 'minikube start'"

# Module 2 – Deploy app
elif ! kubectl get deployment "$APP" &>/dev/null; then
    kubectl create deployment "$APP" --image="$IMAGE_V1"
    echo "Deployment '$APP' v1 created."

# Module 3–4 – Inspect & expose
elif ! kubectl get service "$APP" &>/dev/null; then
    kubectl expose deployment "$APP" --type=NodePort --port=8080
    echo "Service created. Pods:"
    kubectl get pods
    kubectl describe deployment "$APP"

# Module 5 – Scale
elif [ "$(kubectl get deployment "$APP" -o jsonpath='{.spec.replicas}')" -lt 4 ]; then
    kubectl scale deployment "$APP" --replicas=4
    echo "Scaled to 4 replicas."
    kubectl get pods

# Module 6 – Rolling Update
else
    kubectl set image deployment/"$APP" kubernetes-bootcamp="$IMAGE_V2"
    kubectl rollout status deployment/"$APP"
    echo "Rolling update to v2 complete."
fi
```

**Alternative (compact):**

```bash
# All 6 steps in sequence (complete run)
minikube start \
  && kubectl create deployment kubernetes-bootcamp \
       --image=docker.io/jocatalin/kubernetes-bootcamp:v1 \
  && kubectl expose deployment kubernetes-bootcamp --type=NodePort --port=8080 \
  && kubectl scale deployment kubernetes-bootcamp --replicas=4 \
  && kubectl set image deployment/kubernetes-bootcamp \
       kubernetes-bootcamp=docker.io/jocatalin/kubernetes-bootcamp:v2 \
  && kubectl rollout status deployment/kubernetes-bootcamp
```

---

## Tests

| Input 1 | Input 2 | Input 3 | Expected | Result | ✓ |
|---|---|---|---|---|---|
| `kubectl get deployments` | – | – | `READY 4/4` | `READY 4/4` | ✅ |
| `kubectl get pods` | – | – | all pods `Running` | all `Running` | ✅ |
| `kubectl rollout status deployment/kubernetes-bootcamp` | – | – | `successfully rolled out` | `successfully rolled out` | ✅ |

---

## Explanation / Concepts

| Concept | Description |
|---|---|
| Deployment | Declarative K8s object that describes the desired state; the controller ensures actual state = desired state |
| ReplicaSet | Ensures the desired number of identical pods is always running; automatically managed by the Deployment |
| Rolling Update | Strategy for a version change without interruption: new pods start before old ones are stopped – no downtime |

---

## Rules / Logic

```
Module 1: minikube start                              → Cluster ready
Module 2: kubectl create deployment <name> --image=  → Deployment → ReplicaSet → 1 Pod
Module 3: kubectl describe / kubectl exec / proxy    → Inspect pod state
Module 4: kubectl expose --type=NodePort             → Service → external access
Module 5: kubectl scale --replicas=4                 → ReplicaSet adjusts pod count
Module 6: kubectl set image / kubectl rollout        → Image update without downtime
```

---

## Notes

- **Concept:** Kubernetes reconciliation loop – the controller continuously compares desired state (Spec) with actual state (Status) and automatically corrects any deviations

- **Syntax:** `kubectl scale deployment <name> --replicas=<N>` | `kubectl set image deployment/<name> <container>=<image>:<tag>`

- **Order is important:**

    1. Create deployment (pods must be running before a service can route to them)

    2. Expose service (establish network access before scaling)

    3. Scale first, then rolling update (so replicas are always available during the update)

- **Edge Cases:**

    - Rollout hangs → `kubectl rollout undo deployment/kubernetes-bootcamp` reverts the update

    - After `scale --replicas=0` the app is unreachable (intentional: "freeze the deployment")

    - `kubectl exec` fails if the pod is not yet `Running` → wait briefly or use `-w`

- **Tip:** `kubectl get pods,deployments,services` on a single line gives a complete cluster overview at a glance

---

## Optional: Extensions

- Add Liveness & Readiness probes to the deployment to enable automatic health checks

- Check rollout history: `kubectl rollout history deployment/kubernetes-bootcamp`

- Error handling: automatically roll back a failed rolling update via `kubectl rollout undo`

- Usability: use namespace `--namespace=dev` for clean environment separation
