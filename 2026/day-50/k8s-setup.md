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

<img width="1098" height="574" alt="image" src="https://github.com/user-attachments/assets/3e4e6207-cc3e-4697-b19c-289553e77158" />

A **Kubernetes cluster** is built on a client-server architecture consisting of a Control Plane `(the "brain")` that manages a set of Worker Nodes `(the "hands")`. The user interacts with the control plane, which then orchestrates the deployment and management of containerized applications on the worker nodes.

1. `The Control Plane (Master Node)`
  The Control Plane makes global decisions about the cluster and detects and responds to cluster events. Its core components include.
  - kube-apiserver: The "front door" for the cluster. All internal and external communications (like kubectl commands) pass through this API.
  - etcd: A highly available key-value store that acts as the cluster's "memory," storing all configuration data and the cluster state.
  - kube-scheduler: The "decision maker" that assigns newly created Pods to available worker nodes based on resource requirements and constraints.
  - kube-controller-manager: The "automation engine" that runs control loops to maintain the desired state (e.g., ensuring the right number of Pods are running).
  - cloud-controller-manager: An optional component that links your cluster to a cloud provider's API (e.g., AWS, GCP, or Azure). 

2. `Worker Nodes`
Worker nodes are the machines (physical or virtual) where your applications actually run. Every node contains.
  - kubelet: An agent that ensures containers are running in a Pod as specified by the Control Plane
  - kube-proxy: A network proxy that manages network rules on the node, allowing communication to your Pods from inside or outside the cluster.
  - Container Runtime: The software responsible for running containers. Modern Kubernetes clusters typically use containerd or CRI-O rather than Docker. 

3. `Key Workload Objects`
  - Pods: The smallest deployable unit in Kubernetes, typically containing a single container.
  - Services: Provides a stable IP address and DNS name to access a group of Pods, enabling load balancing.
  - Deployments: Describes the "desired state" of your application, allowing for automated rollouts and self-healing. 

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
1. `What happens when you run `kubectl apply -f pod.yaml`? Trace the request through each component.`
  When we run `kubectl apply -f pod.yaml`, Kubernetes performs a declarative update to bring the cluster's actual state in line with the "desired state" defined in your file. Unlike kubectl create, which strictly attempts to make a new resource and fails if it already exists, apply is idempotent: it creates the resource if it’s missing or updates it if it’s already there.

**The Core Mechanism: Three-Way Merge**
To determine exactly what needs to change, Kubernetes uses a "three-way merge" process: 
  - Local Configuration: The YAML file you just provided.
  - Live Object Configuration: The current state of the resource as it exists in the cluster.
  - Last Applied Configuration: A record of the YAML used the last time you ran apply. This is stored as a JSON string in an annotation called kubectl.kubernetes.io/last-applied-configuration on the live object

2. `What happens if the API server goes down?`
When an API server goes down, the primary consequence is a service disruption where dependent applications and users can no longer   communicate with the backend service to exchange data or perform tasks.
The exact impact depends on the API's function and how the failure is handled by client applications and system design.
  - Immediate Technical Impacts : Failed Requests: Clients attempting to interact with the API will receive error responses, commonly a timeout or an HTTP 500 Internal Server Error.
  - Service Unavailability: If the API is for a critical function (e.g., payments, user authentication), the entire service or dependent features may become completely unavailable.
  - Partial Failures: For applications that rely on multiple APIs, only the features dependent on the downed server will fail, leading to an inconsistent user experience.
  - System Instability: In complex systems like Kubernetes clusters, the API server is a central control point. Its failure means crucial components like the scheduler, node manager, and autoscaler stop functioning correctly, affecting the entire cluster management process.
  - Loss of Monitoring Access: External monitoring and logging systems may lose access to the server's data, making it harder to detect and diagnose the problem. 

  **Business and User Experience Impacts**
    - Revenue Loss: Businesses can lose significant revenue if customers are unable to access products, complete purchases, or use paid services.
    - Negative User Experience (UX): Users will encounter broken features, error messages, or slow performance, leading to frustration.
    - Erosion of Trust and Reputation: Frequent or prolonged outages can damage a company's reputation and lead to customer churn as users look for more reliable alternatives.
    - Operational Delays: Internal business processes that rely on the API for data exchange (e.g., inventory management, supply chain logistics) can face significant bottlenecks.

  **Mitigation and Best Practices**
  Organizations employ several strategies to minimize the impact of API server downtime:
  - Timeouts and User Notification: Clients can implement request timeouts and notify the user with a friendly message (e.g., "Something went wrong, please try again later") rather than letting the application hang indefinitely.
  - Redundancy and Failover: Running multiple instances of the API server across different locations (active-active failover) ensures that if one fails, traffic is seamlessly rerouted to a healthy instance.
  - Load Balancing and Health Checks: Load balancers distribute incoming requests and perform continuous health checks to automatically avoid sending traffic to failed servers.
  - Rate Limiting and Throttling: These mechanisms prevent a single client from overwhelming the server with too many requests, which can cause an outage for all users.
  - Automated Monitoring and Recovery: Robust monitoring systems can detect issues early and automatically trigger recovery actions, such as restarting processes or scaling resources, often without human intervention.

3. `What happens if a worker node goes down?`
  When a **worker node** goes down in a Kubernetes cluster, the control plane detects the failure and automatically works to maintain the desired state of the application by rescheduling workloads.
  The specific behavior depends on the configuration, but generally follows a defined process of detection, marking, and eviction.
<img width="662" height="231" alt="image" src="https://github.com/user-attachments/assets/ec3a8b4a-ccb5-4ef8-899f-1c2e692190ec" />

1. Detection and Status Change
  - Heartbeat Failure: The master node (control plane) stops receiving heartbeats from the worker node's kubelet.
  - NotReady Status: After roughly 1 minute of silence, the node is marked as NotReady in the cluster.
  - Pod Status: After about 5 minutes (default pod-eviction-timeout), pods on that node are marked as Unknown or NodeLost.
    
2. Workload Rescheduling (Self-Healing)
  - Eviction: The control plane begins terminating the pods on the unreachable node.
  - Rescheduling: Kubernetes attempts to create new instances of these pods on other healthy worker nodes. 
DaemonSets & StatefulSets: 
  - DaemonSets will continue to run one pod on all other remaining nodes.
  - StatefulSets are treated carefully; the system will not move them until it is certain the old pod is truly gone, to prevent data corruption.

3. Impact on Applications
  - Temporary Downtime: Applications running on the failed node will be unavailable until their replacements are running on other nodes.
  - Traffic Shift: If you are using a Service, it will stop routing traffic to the pods on the broken node and redirect it to the newly scheduled pods.
  - Storage Issues: If a pod uses Read-Write-Once (RWO) persistent volumes, the new pod may get stuck in ContainerCreating because the
  - volume is still "attached" to the failed node. The system usually fixes this after a 6-minute timeout. 

4. Recovery
  - Node Restart: If the node is rebooted, kubelet will reconnect, the NotReady status will be removed, and it will rejoin the cluster.
  - Automatic Replacement: In cloud environments (EKS, GKE, AKS), if the node does not recover, the auto-scaler will typically terminate the broken node and provision a new one to maintain the desired capacity. 

---

### Task 3: Install kubectl
`kubectl` is the CLI tool you will use to talk to your Kubernetes cluster.

Install it:
```
# Linux (amd64)
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

<img width="850" height="105" alt="image" src="https://github.com/user-attachments/assets/ac0d3975-afed-4fba-aa74-dac9990373c6" />

Verify:
```bash
kubectl version --client
```
<img width="850" height="105" alt="image" src="https://github.com/user-attachments/assets/4fd00a5b-297c-4e6b-aa20-f54e997e79ff" />

---

### Task 4: Set Up Your Local Cluster
```
# Linux
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

<img width="736" height="72" alt="image" src="https://github.com/user-attachments/assets/6f05fb39-3f36-4212-b44f-9c80f4725ad8" />

```
# Create a cluster
kind create cluster --name devops-cluster
```
<img width="955" height="233" alt="image" src="https://github.com/user-attachments/assets/955317e7-77df-4a04-9766-6622d5c0afa9" />
<img width="955" height="99" alt="image" src="https://github.com/user-attachments/assets/8d4dc0fe-3ca8-4a06-83f7-916d416d4ff7" />

```
# Verify
kubectl cluster-info
kubectl get nodes
```
<img width="1045" height="178" alt="image" src="https://github.com/user-attachments/assets/656177a0-bbdd-48b5-a32e-7886cd370a14" />


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
<img width="955" height="233" alt="image" src="https://github.com/user-attachments/assets/955317e7-77df-4a04-9766-6622d5c0afa9" />
<img width="955" height="99" alt="image" src="https://github.com/user-attachments/assets/8d4dc0fe-3ca8-4a06-83f7-916d416d4ff7" />
<img width="1101" height="147" alt="image" src="https://github.com/user-attachments/assets/505c6f79-3cd0-4c7f-8c0b-851b9034cda0" />
<img width="996" height="563" alt="image" src="https://github.com/user-attachments/assets/7f63204c-570d-4e85-8f52-51cb4456bccc" />

<img width="843" height="160" alt="image" src="https://github.com/user-attachments/assets/219b4a70-494f-49e8-9184-6255339bd7d4" />
<img width="1102" height="452" alt="image" src="https://github.com/user-attachments/assets/8f0187c2-0ec3-4d4c-b596-16d2b02d33ce" />
<img width="1100" height="471" alt="image" src="https://github.com/user-attachments/assets/b82ab8e7-9b73-4b86-8452-e922bf3eebbb" />
<img width="1104" height="561" alt="image" src="https://github.com/user-attachments/assets/2460637a-d00b-4816-9698-6e03e6d7fe74" />
<img width="1087" height="362" alt="image" src="https://github.com/user-attachments/assets/33b5b86f-5db3-4ac5-98fb-54841556cc76" />

Look at the pods running in the `kube-system` namespace:
```bash
kubectl get pods -n kube-system
```
<img width="1088" height="234" alt="image" src="https://github.com/user-attachments/assets/34438bdd-9a56-4bb2-ac6f-ca5f8cb8dcc2" />

You should see pods like `etcd`, `kube-apiserver`, `kube-scheduler`, `kube-controller-manager`, `coredns`, and `kube-proxy`. These are the architecture components you drew in Task 2 — running as pods inside the cluster.

**Verify:** Can you match each running pod in `kube-system` to a component in your architecture diagram? - **YES**

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
<img width="1108" height="324" alt="image" src="https://github.com/user-attachments/assets/5fb3b1ac-31dc-4e96-b3f1-b06d5a1bfda2" />
<img width="1101" height="289" alt="image" src="https://github.com/user-attachments/assets/e6fc7c73-d349-40ff-908b-ad618cc06bf2" />

Try these useful commands:
```bash
# Check which cluster kubectl is connected to
kubectl config current-context

# List all available contexts (clusters)
kubectl config get-contexts

# See the full kubeconfig
kubectl config view
```
<img width="1103" height="382" alt="image" src="https://github.com/user-attachments/assets/21a5c59f-6b96-41aa-b980-51d2eb2940f0" />

`Write down: What is a kubeconfig? Where is it stored on your machine?`

- A kubeconfig is a YAML-formatted file that stores authentication and connection information (clusters, users, contexts) for kubectl to interact with Kubernetes clusters. 
- It acts as a "connection string," allowing you to securely manage one or more clusters.
- By default, it is stored in the hidden directory ~/.kube/config (Linux/macOS) or %USERPROFILE%\.kube\config (Windows).

---




