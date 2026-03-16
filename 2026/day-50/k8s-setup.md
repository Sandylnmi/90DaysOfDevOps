# Kubernetes Architecture and Cluster Setup

---

## Challenge Tasks

### Task 1: Recall the Kubernetes Story
Before touching a terminal, write down from memory:

1. Why was Kubernetes created? What problem does it solve that Docker alone cannot?

`Kubernetes` was created by Google to solve the challenges of managing and scaling containerized applications in large-scale production environments. While Docker is excellent for packaging and running individual containers, it lacks the necessary built-in features for orchestrating complex, distributed systems across multiple machines.

**Problems Kubernetes Solves that Docker Alone Cannot**
Docker alone is primarily a tool for building and running individual containers on a single host. When applications moved from a single server to large, distributed production systems, new challenges emerged that required an orchestration layer.

**Kubernetes addresses these operational challenges with features such as**
`Automated Scaling:` Kubernetes can automatically scale the number of container instances (pods) up or down based on resource usage (like CPU or memory) or demand, something Docker requires manual intervention or custom scripting to achieve. 

`Self-Healing:` Kubernetes constantly monitors the health of containers. If a container or the node it runs on fails, Kubernetes automatically restarts or replaces the failed container on a healthy node, ensuring application availability without human intervention. 

`Service Discovery and Load Balancing:` In a multi-container environment, containers need to find and communicate with each other. Kubernetes provides built-in mechanisms for service discovery and load balancing, distributing network traffic evenly across healthy container replicas. 

`Multi-Host Networking and Management:` Docker is primarily focused on a single host. Kubernetes is designed from the ground up to manage a cluster of machines (nodes) and seamlessly deploy applications across them, abstracting the underlying infrastructure. 

`Automated Rollouts and Rollbacks:` Kubernetes manages application updates in a controlled manner, allowing new versions to be deployed without downtime. If something goes wrong, it can automatically roll back the changes to a stable version. 

`Configuration and Secrets Management:` Kubernetes offers a secure way to manage sensitive information (secrets) like passwords and API keys, as well as application configurations, keeping them separate from the container images themselves. 

**Problems Kubernetes Solves that Docker Alone Cannot**
Docker provides the "bricks" (containers) for building applications, while Kubernetes acts as the "construction crew" that builds a resilient, scalable skyscraper. Here are the key problems Kubernetes solves: 

`Orchestration Across Multiple Hosts:` Docker is primarily focused on running containers on a single host. Kubernetes is designed to orchestrate containers across a cluster of machines (nodes), dynamically scheduling and distributing workloads based on resource requirements and availability. 

`Automated Scaling:` Docker alone cannot automatically adjust the number of running containers based on demand or CPU/memory usage. Kubernetes offers built-in mechanisms like the Horizontal Pod Autoscaler (HPA) to automatically scale applications up or down as needed. 

`Self-Healing and High Availability:` If a container or an entire node fails, Docker doesn't automatically restart or replace it. Kubernetes constantly monitors container health, automatically restarting failed containers, replacing pods, and rescheduling them to healthy nodes to ensure minimal downtime and high availability. 

`Service Discovery and Load Balancing:` In a large deployment, containers are dynamic and have changing IP addresses. Kubernetes provides stable IP addresses and a single DNS name for a set of pods (the smallest deployable units), and automatically load-balances network traffic across them, making communication between services seamless. 

`Automated Rollouts and Rollbacks:` Deploying new versions of an application with Docker can be a manual and error-prone process. Kubernetes handles controlled, rolling updates without downtime and can automatically roll back changes if something goes wrong during the deployment. 

`Configuration and Secrets Management:` Hardcoding sensitive information (like passwords and API keys) into Dockerfiles is a security risk. Kubernetes provides secure mechanisms for managing Secrets and ConfigMaps separately from the container image itself. 

`Storage Orchestration:` Kubernetes can automatically mount and manage the appropriate storage systems (local, cloud-provider specific, or network storage) required by containers, which is critical for stateful applications. 

2. Who created Kubernetes and what was it inspired by?

Kubernetes was created by a team of engineers at Google and was heavily inspired by Google's internal cluster management system called Borg.
The primary inspiration for Kubernetes was Google's internal cluster management system, Borg, which was developed in the early 2000s and used to run virtually all of Google's extensive internet services, including Gmail, Google Search, and YouTube. 

**Key concepts from Borg that were incorporated into Kubernetes include**
`Pods:` The foundational unit of scheduling that groups one or more containers together on the same machine.
`Services and Labels:` Abstractions for networking and organizing resources.
`Automatic management:` Features like replication, load balancing, health checking, and self-healing systems

3. What does the name "Kubernetes" mean?

- `Kubernetes (often abbreviated as K8s)` is a Greek word meaning "helmsman" or "pilot," signifying its role in steering, managing, and automating containerized applications. 
Originally developed by Google as an open-source platform, it functions as a "captain" for container clusters, ensuring they operate efficiently.

- Kubernetes is a portable, extensible, open source platform for managing containerized workloads and services that facilitate both declarative configuration and automation. It has a large, rapidly growing ecosystem. Kubernetes services, support, and tools are widely available.


---

### Task 2: Draw the Kubernetes Architecture
From memory, draw or describe the Kubernetes architecture. Your diagram should include:

https://miro.medium.com/v2/resize:fit:875/1*130HH_eLLwyiKSr2SlZSCA.png

**Control Plane (Master Node):**
- API Server — the front door to the cluster, every command goes through it
- etcd — the database that stores all cluster state
- Scheduler — decides which node a new pod should run on
- Controller Manager — watches the cluster and makes sure the desired state matches reality

**Worker Node:**
- kubelet — the agent on each node that talks to the API server and manages pods
- kube-proxy — handles networking rules so pods can communicate
- Container Runtime — the engine that actually runs containers (containerd, CRI-O)

After drawing, verify your understanding:
- What happens when you run `kubectl apply -f pod.yaml`? Trace the request through each component.
- What happens if the API server goes down?
- What happens if a worker node goes down?

---

### Task 3: Install kubectl
`kubectl` is the CLI tool you will use to talk to your Kubernetes cluster.

Install it:
```bash
# macOS
brew install kubectl

# Linux (amd64)
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

# Windows (with chocolatey)
choco install kubernetes-cli
```

Verify:
```bash
kubectl version --client
```

---

### Task 4: Set Up Your Local Cluster
Choose **one** of the following. Both give you a fully functional Kubernetes cluster on your machine.

**Option A: kind (Kubernetes in Docker)**
```bash
# Install kind
# macOS
brew install kind

# Linux
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# Create a cluster
kind create cluster --name devops-cluster

# Verify
kubectl cluster-info
kubectl get nodes
```

**Option B: minikube**
```bash
# Install minikube
# macOS
brew install minikube

# Linux
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Start a cluster
minikube start

# Verify
kubectl cluster-info
kubectl get nodes
```

Write down: Which one did you choose and why?

---

### Task 5: Explore Your Cluster
Now that your cluster is running, explore it:

```bash
# See cluster info
kubectl cluster-info

# List all nodes
kubectl get nodes

# Get detailed info about your node
kubectl describe node <node-name>

# List all namespaces
kubectl get namespaces

# See ALL pods running in the cluster (across all namespaces)
kubectl get pods -A
```

Look at the pods running in the `kube-system` namespace:
```bash
kubectl get pods -n kube-system
```

You should see pods like `etcd`, `kube-apiserver`, `kube-scheduler`, `kube-controller-manager`, `coredns`, and `kube-proxy`. These are the architecture components you drew in Task 2 — running as pods inside the cluster.

**Verify:** Can you match each running pod in `kube-system` to a component in your architecture diagram?

---

### Task 6: Practice Cluster Lifecycle
Build muscle memory with cluster operations:

```bash
# Delete your cluster
kind delete cluster --name devops-cluster
# (or: minikube delete)

# Recreate it
kind create cluster --name devops-cluster
# (or: minikube start)

# Verify it is back
kubectl get nodes
```

Try these useful commands:
```bash
# Check which cluster kubectl is connected to
kubectl config current-context

# List all available contexts (clusters)
kubectl config get-contexts

# See the full kubeconfig
kubectl config view
```

Write down: What is a kubeconfig? Where is it stored on your machine?

---

## Hints
- kind requires Docker to be running (it creates clusters using containers)
- minikube can use Docker, VirtualBox, or other drivers
- The default kubeconfig file is at `~/.kube/config`
- `kubectl get pods -A` is short for `kubectl get pods --all-namespaces`
- If `kubectl` cannot connect, check if your cluster is running: `kind get clusters` or `minikube status`
- `-o wide` flag gives extra details: `kubectl get nodes -o wide`

---

## Documentation
Create `day-50-k8s-setup.md` with:
- Kubernetes history in your own words (3-4 sentences)
- Your architecture diagram (text-based or image)
- Which tool you chose (kind/minikube) and why
- Screenshot of `kubectl get nodes` and `kubectl get pods -n kube-system`
- What each kube-system pod does

---


