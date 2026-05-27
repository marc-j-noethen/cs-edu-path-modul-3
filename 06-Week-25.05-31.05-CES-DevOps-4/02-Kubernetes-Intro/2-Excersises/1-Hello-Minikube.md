# ☸️ Hello Minikube (Kubernetes Basics – Local Cluster)

**Course:** CES DevOps 4 – Kubernetes Intro | **Date:** 27 May 2026

---

## Task

**Objective:**  
Initialise a local Kubernetes environment using Minikube, deploy a test application, and verify successful operation using `kubectl get services` and the output in the browser.

**Prerequisites:**

- `minikube` installed locally and running

- `kubectl` installed and configured to point to the Minikube cluster

- Create a deployment (`hello-node`) using the official test image

- Expose the deployment externally via a `NodePort` service so that it is accessible via the browser

- Output:

    - `kubectl get services` shows `hello-node` with type `NodePort`

    - Browser displays `Hello Kubernetes!` (or agnhost response)

    - `minikube service hello-node --url` returns an accessible URL

---

## Solution

```bash
# Inputs
CLUSTER="minikube"
APP="hello-node"
IMAGE="registry.k8s.io/e2e-test-images/agnhost:2.39"

# Main logic
if ! minikube status | grep -q "Running"; then
    echo "ERROR: Minikube is not running. Start first with: minikube start"
elif ! kubectl get deployment "$APP" &>/dev/null; then
    kubectl create deployment "$APP" --image="$IMAGE" -- /agnhost netexec --http-port=8080
    kubectl expose deployment "$APP" --type=NodePort --port=8080
    echo "Deployment '$APP' created and exposed as a NodePort service."
else
    echo "Deployment '$APP' already exists – open the service in your browser."
    minikube service "$APP"
fi
```

**Alternative (compact):**

```bash
minikube start \
  && kubectl create deployment hello-node \
       --image=registry.k8s.io/e2e-test-images/agnhost:2.39 \
       -- /agnhost netexec --http-port=8080 \
  && kubectl expose deployment hello-node --type=NodePort --port=8080 \
  && minikube service hello-node
```

---

## Tests

| Input 1 | Input 2 | Input 3 | Expected | Result | ✓ |
|---|---|---|---|---|---|
| `minikube status` | – | – | `host: Running` | `host: Running` | ✅ |
| `kubectl get deployment hello-node` | – | – | `READY 1/1` | `READY 1/1` | ✅ |
| `kubectl get services hello-node` | – | – | `TYPE: NodePort` | `TYPE: NodePort` | ✅ |

---

## Explanation / Concepts

| Concept | Description |
|---|---|
| Minikube | Local single-node Kubernetes cluster for development & testing; emulates a complete K8s environment on your own computer |
| kubectl | Command-line client for Kubernetes; sends API requests to the Cluster API server |
| NodePort | Service type that opens a port on the node (VM/host) and forwards traffic to the pod; enables external access without a Cloud LoadBalancer |

---

## Rules / Logic

```
1. minikube start            → starts the VM/container node and the K8s API server
2. kubectl create deployment → creates a Deployment object → ReplicaSet → Pod (Running)
3. kubectl expose deployment → creates a Service object (NodePort) → binds port 8080
4. minikube service <name>   → determines Node-IP + NodePort → opens URL in browser
5. kubectl get services      → lists all services including ClusterIP & NodePort
```

---

## Notes

- **Concept:** Kubernetes object hierarchy: Deployment → ReplicaSet → Pod → Container

- **Syntax:** `kubectl <verb> <resource> <name> [--flags]`

- **Order is important:**

    1. First `minikube start` (the cluster must be running before kubectl commands take effect)

    2. Then `kubectl create deployment` (Pod must exist before a Service can route to it)

    3. Then `kubectl expose deployment` (only expose after deployment has completed)

- **Edge Cases:**

    - Minikube not started → all `kubectl` commands fail with a ConnectionRefused error

    - Image cannot be pulled (no internet access) → Pod remains in `ImagePullBackOff` status

    - Port 8080 already in use on the host → NodePort assignment by K8s still takes place (K8s selects range 30000–32767)

- **Tip:** `kubectl get pods -w` starts a live watch; `-w` displays status changes in real time without having to retype the command

---

## Optional: Extensions

- Build your own Docker image (`docker build`) and push it to the Minikube registry: `eval $(minikube docker-env)`

- Check deployment availability: `kubectl rollout status deployment/hello-node`

- Troubleshooting: `kubectl describe pod <pod-name>` and `kubectl logs <pod-name>` for detailed diagnostics

- Usability: Set an alias `alias k=kubectl` in your shell configuration for faster input
