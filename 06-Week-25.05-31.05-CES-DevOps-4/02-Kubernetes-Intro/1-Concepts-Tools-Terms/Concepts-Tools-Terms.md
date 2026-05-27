
# Kubernetes-Intro

### 1. Most Important Concept

**Kubernetes (K8s) is a container orchestration system** – it automatically manages where and how many containers run, restarts crashed containers, and scales on demand. It complements Docker: Docker _builds_ the package, Kubernetes _operates_ it at scale.

---

### 2. Step-by-Step: Core Process (Desired State Workflow)

1. You write a **YAML file** (manifest) that describes what you want – e.g. "5 replicas of my app on port 80".
2. You send the file to the **Control Plane** using `kubectl apply -f file.yaml`.
3. Kubernetes runs a **reconciliation loop**: actual state == desired state?
4. If the actual state deviates (e.g. a pod crashes), **Kubernetes self-heals** – new pods are started automatically.

---

### 3. Interactive Mode – Cluster in Action

```bash
# Start the cluster
minikube start

# Check nodes (should show "minikube - Ready")
kubectl get nodes

# Create first deployment (nginx web server)
kubectl create deployment hello-k8s --image=nginx

# Check running pods
kubectl get pods

# Delete a pod (Kubernetes will automatically recreate it!)
kubectl delete pod [POD_NAME]

# Check pods again – new pod was self-healed
kubectl get pods
```

> 💡 **Why does Kubernetes recreate the pod?** Because the **Deployment** is still active and enforces its desired state (1 replica).

---

### 4. Key Concepts with Code Examples

#### Control Plane vs. Node

|Component|Role|Analogy|
|---|---|---|
|**Control Plane**|Makes decisions, monitors the cluster|The brain|
|**Node**|Actually runs the containers|The muscles|
|**Kubelet**|Agent on every node, receives instructions from the Control Plane|The foreman|

#### Pod

The smallest unit in K8s. Containers in the same pod share a network and `localhost`.

```yaml
# A pod rarely runs alone – it is managed by a Deployment
```

#### Deployment

Manages pods. Ensures the desired number is always running.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: secure-frontend      # Name of the Deployment
spec:
  replicas: 4                # Kubernetes keeps exactly 4 pods alive
  template:
    spec:
      containers:
      - name: nginx-server
        image: nginx:1.25.3  # Exact image version → reproducibility
```

> ❓ **Think about it (from the material):** How many replicas? → **4** / Which image? → **nginx:1.25.3**

#### Service

Stable IP address / entry point for pods. Pods come and go – the Service stays.

```
[User] → [Service (stable IP)] → [Pod 1]
                               → [Pod 2]
                               → [Pod 3]
```

---

### 5. Docker vs. Kubernetes

|Feature|Docker / Docker Compose|Kubernetes (K8s)|
|---|---|---|
|**Scope**|Single machine|Server cluster|
|**Scaling**|Manual|Automatic (based on load)|
|**Self-healing**|Limited|Full|
|**Configuration**|`docker-compose.yml`|YAML manifests|
|**Analogy**|The shipping container|The cargo ship + crane system|

---

### 6. Why This Matters / Benefits

- **Fault tolerance:** If a server goes down, K8s automatically moves containers to other nodes.
- **Scalability:** Traffic spikes 10x overnight? K8s automatically starts more pods.
- **Declarative over imperative:** You describe _what_ you want, not _how_ it should happen – fewer human errors.
- **Industry standard:** Every major cloud platform (AWS EKS, Azure AKS, GCP GKE) is built on Kubernetes.

---

## Quick-Start Checklist

☐ `kubectl` installed (`kubectl version --client` returns a version)  
☐ `minikube` installed (`minikube version` returns a version)  
☐ `minikube start` executed, cluster is running  
☐ `kubectl get nodes` shows one node with status `Ready`  
☐ First deployment created with `kubectl create deployment hello-k8s --image=nginx`  
☐ Pod verified with `kubectl get pods`  
☐ Pod deleted and self-healing observed with `kubectl get pods`  
☐ Able to explain the difference between Pod / Deployment / Service in your own words

---

**Key Takeaway**

> Docker builds the container – Kubernetes is the conductor that ensures exactly as many containers are running as needed, regardless of server failures or traffic spikes.

---

## Table 1: Tools Used

|Tool|Meaning|
|---|---|
|`minikube`|Local Kubernetes cluster for development and practice (runs inside a VM or container)|
|`kubectl`|CLI for communicating with the Kubernetes cluster (deployments, logs, scaling)|
|`Docker` / `containerd`|Container runtime environment on each node|
|`nginx`|Web server used as the example image in this lesson (`nginx:1.25.3`)|

---

## Table 2: Technical Terms

|Term|Meaning|
|---|---|
|**Cluster**|Group of machines (nodes) managed together by Kubernetes|
|**Control Plane**|The brain of the cluster – makes decisions, monitors state|
|**Node**|A single machine (physical or virtual) in the cluster where pods run|
|**Kubelet**|Agent on every node, receives and executes instructions from the Control Plane|
|**Pod**|Smallest deployable unit in K8s – a wrapper around one or more containers|
|**Deployment**|K8s object that manages pods and enforces the desired replica count|
|**Service**|Stable network entry point for pods (fixed IP despite constantly changing pods)|
|**Desired State**|The target state declared in YAML – K8s always works towards it|
|**Manifest**|YAML file that describes the desired state of a K8s resource|
|**Reconciliation Loop**|Continuous loop where K8s compares actual and desired state and corrects deviations|
|**Self-healing**|Automatic restarting/rescheduling of pods when failures occur|
|**Replicas**|Number of identical running pod instances of an application|
|**Orchestration**|Automated management of container lifecycles across multiple servers|

---

## Table 3: Key Vocabulary

|Term|Meaning|
|---|---|
|`kubectl apply -f`|Send / apply a YAML manifest to the cluster|
|`kubectl get pods`|Show all running pods and their status|
|`kubectl get nodes`|Show all nodes in the cluster and their status|
|`kubectl create deployment`|Imperatively create a new deployment (without YAML)|
|`kubectl delete pod`|Delete a pod (will be recreated by the Deployment)|
|`minikube start`|Start the local Kubernetes cluster|
|`replicas:`|YAML key for the desired number of running pods|
|`image:`|YAML key for the container image including version|
|`kind: Deployment`|YAML key that defines the type of K8s resource|
|`K8s`|Abbreviation for Kubernetes (8 letters between K and s)|