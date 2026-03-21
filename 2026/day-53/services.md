# Kubernetes Services


## Why Services?

Every Pod gets its own IP address. But there are two problems:
1. Pod IPs are **not stable** — when a Pod restarts or gets replaced, it gets a new IP
2. A Deployment runs **multiple Pods** — which IP do you connect to?

A Service solves both problems. It provides:
- A **stable IP and DNS name** that never changes
- **Load balancing** across all Pods that match its selector

```
[Client] --> [Service (stable IP)] --> [Pod 1]
                                   --> [Pod 2]
                                   --> [Pod 3]
```

---

## Challenge Tasks

### Task 1: Deploy the Application
First, create a Deployment that you will expose with Services. Create `app-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
  labels:
    app: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: nginx
        image: nginx:1.25
        ports:
        - containerPort: 80
```

```bash
kubectl apply -f app-deployment.yaml
kubectl get pods -o wide
```
<img width="1110" height="388" alt="image" src="https://github.com/user-attachments/assets/2b3eebd2-a98d-4162-b9fe-7e31eb55ea67" />

Note the individual Pod IPs. These will change if pods restart — that is the problem Services fix.

**Verify:** Are all 3 pods running? Note down their IP addresses.

<img width="604" height="131" alt="image" src="https://github.com/user-attachments/assets/51171009-d9d4-4710-a75b-20f7ff9da0e1" />

---

### Task 2: ClusterIP Service (Internal Access)
ClusterIP is the default Service type. It gives your Pods a stable internal IP that is only reachable from within the cluster.

Create `clusterip-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app-clusterip
spec:
  type: ClusterIP
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 80
```

Key fields:
- `selector.app: web-app` — this Service routes traffic to all Pods with the label `app: web-app`
- `port: 80` — the port the Service listens on
- `targetPort: 80` — the port on the Pod to forward traffic to

```bash
kubectl apply -f clusterip-service.yaml
kubectl get services
```
<img width="797" height="124" alt="image" src="https://github.com/user-attachments/assets/3407256c-651a-4368-8929-f7a7b6df1135" />

You should see `web-app-clusterip` with a CLUSTER-IP address. This IP is stable — it will not change even if Pods restart.

Now test it from inside the cluster:
```bash
# Run a temporary pod to test connectivity
kubectl run test-client --image=busybox:latest --rm -it --restart=Never -- sh

# Inside the test pod, run:
wget -qO- http://web-app-clusterip
exit
```
<img width="1109" height="509" alt="image" src="https://github.com/user-attachments/assets/36ea61d4-1198-4ecb-adba-4edd4f485c65" />

You should see the Nginx welcome page. The Service load-balanced your request to one of the 3 Pods.

**Verify:** Does the Service respond? Try running the wget command multiple times — the Service distributes traffic across all healthy Pods.

---

### Task 3: Discover Services with DNS
Kubernetes has a built-in DNS server. Every Service gets a DNS entry automatically:

```
<service-name>.<namespace>.svc.cluster.local
```

Test this:
```bash
kubectl run dns-test --image=busybox:latest --rm -it --restart=Never -- sh

# Inside the pod:
# Short name (works within the same namespace)
wget -qO- http://web-app-clusterip

# Full DNS name
wget -qO- http://web-app-clusterip.default.svc.cluster.local

# Look up the DNS entry
nslookup web-app-clusterip
exit
```
<img width="1106" height="424" alt="image" src="https://github.com/user-attachments/assets/cec5aeb9-172b-4986-adf9-1e06a321bee5" />
<img width="1103" height="367" alt="image" src="https://github.com/user-attachments/assets/ff8e8eb5-f172-4e81-b68f-293394221fe2" />
<img width="1100" height="265" alt="image" src="https://github.com/user-attachments/assets/e305e5fe-ca0b-4de2-a862-d468b0aab0d3" />

Both the short name and the full DNS name resolve to the same ClusterIP. In practice, you use the short name when communicating within the same namespace and the full name when reaching across namespaces.

**Verify:** What IP does `nslookup` return? Does it match the CLUSTER-IP from `kubectl get services`?

- Yes, when we run nslookup on a Kubernetes service name from within the cluster (specifically from a Pod), it resolves to the exact same CLUSTER-IP that is listed in the output of kubectl get services.
- The nslookup command, when used inside a Pod with the appropriate DNS configuration, queries the cluster's DNS service (like CoreDNS or kube-dns) and returns the A record for the service.

---

### Task 4: NodePort Service (External Access via Node)
A NodePort Service exposes your application on a port on every node in the cluster. This lets you access the Service from outside the cluster.

Create `nodeport-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app-nodeport
spec:
  type: NodePort
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080
```

- `nodePort: 30080` — the port opened on every node (must be in range 30000-32767)
- Traffic flow: `<NodeIP>:30080` -> Service -> Pod:80

```bash
kubectl apply -f nodeport-service.yaml
kubectl get services
```
<img width="1107" height="285" alt="image" src="https://github.com/user-attachments/assets/cae36f05-a3e0-4a60-a26e-82259014ea34" />

Access the service:
```bash
# If using Minikube
minikube service web-app-nodeport --url

# If using Kind, get the node IP first
kubectl get nodes -o wide
# Then curl <node-internal-ip>:30080

# If using Docker Desktop
curl http://localhost:30080
```

**Verify:** Can you see the Nginx welcome page from your browser or terminal using the NodePort?

---

### Task 5: LoadBalancer Service (Cloud External Access)
In a cloud environment (AWS, GCP, Azure), a LoadBalancer Service provisions a real external load balancer that routes traffic to your nodes.

Create `loadbalancer-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app-loadbalancer
spec:
  type: LoadBalancer
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 80
```

```bash
kubectl apply -f loadbalancer-service.yaml
kubectl get services
```
<img width="1080" height="154" alt="image" src="https://github.com/user-attachments/assets/33c3ebd2-e0d0-4a44-815d-b03830c71287" />

On a local cluster (Minikube, Kind, Docker Desktop), the EXTERNAL-IP will show `<pending>` because there is no cloud provider to create a real load balancer. This is expected.

If you are using Minikube:
```bash
# Minikube can simulate a LoadBalancer
minikube tunnel
# In another terminal, check again:
kubectl get services
```

In a real cloud cluster, the EXTERNAL-IP would be a public IP address or hostname provisioned by the cloud provider.

**Verify:** What does the EXTERNAL-IP column show? Why is it `<pending>` on a local cluster?

  - The EXTERNAL-IP column shows the public or external IP address assigned to a Kubernetes service, allowing external traffic to reach it. It shows <pending> on a local cluster (like Minikube or Kind) because there is no cloud provider load balancer available to automatically provision and assign an external IP address.
  - Why <pending> on a Local Cluster?
      - No Load Balancer Provider: Services of type LoadBalancer depend on external infrastructure, such as cloud providers (AWS, GCP, Azure), to assign an IP. Local environments lack this component by default.
      - Missing Controller: Local clusters lack a "cloud controller manager" to manage the external IP request.
  - NodePort: Use type: NodePort instead of LoadBalancer to expose the service on a specific port on your node IP.
  - Port-Forwarding: Use kubectl port-forward to access the service directly.
    
---

### Task 6: Understand the Service Types Side by Side
Check all three services:

```bash
kubectl get services -o wide
```
<img width="954" height="118" alt="image" src="https://github.com/user-attachments/assets/aa908b4e-0c84-4f09-8d53-8a98258c2ec1" />

Compare them:

| Type | Accessible From | Use Case |
|------|----------------|----------|
| ClusterIP | Inside the cluster only | Internal communication between services |
| NodePort | Outside via `<NodeIP>:<NodePort>` | Development, testing, direct node access |
| LoadBalancer | Outside via cloud load balancer | Production traffic in cloud environments |

Each type builds on the previous one:
- LoadBalancer creates a NodePort, which creates a ClusterIP
- So a LoadBalancer service also has a ClusterIP and a NodePort

Verify this:
```bash
kubectl describe service web-app-loadbalancer
```
<img width="890" height="315" alt="image" src="https://github.com/user-attachments/assets/fe579e7e-60d7-4cb8-9904-c0b691197b18" />

You should see all three: a ClusterIP, a NodePort, and the LoadBalancer configuration.

**Verify:** Does the LoadBalancer service also have a ClusterIP and NodePort assigned?

  - Yes, a Kubernetes LoadBalancer service automatically has both a ClusterIP and a NodePort assigned to it.
  - Kubernetes service types build upon each other
      - `ClusterIP`is the base, providing a stable internal IP address for intra-cluster communication.
      - `NodePort` builds on ClusterIP, exposing the service on a static port across all nodes' IP addresses, in addition to its ClusterIP.
      - `LoadBalancer` builds on both, provisioning an external cloud provider load balancer (e.g., AWS ELB, Azure LB, GCP Load Balancer) that routes external traffic to the service via its NodePort and ClusterIP, and ultimately to the pods.
  - The traffic flow for a LoadBalancer service is `External Client → Cloud Load Balancer → Node IP:NodePort → ClusterIP → Pod`
     
---

### Task 7: Clean Up
```bash
kubectl delete -f app-deployment.yaml
kubectl delete -f clusterip-service.yaml
kubectl delete -f nodeport-service.yaml
kubectl delete -f loadbalancer-service.yaml

kubectl get pods
kubectl get services
```
<img width="741" height="254" alt="image" src="https://github.com/user-attachments/assets/0b3b6f00-9e9e-4b9b-a288-a6223b67f443" />

Only the built-in `kubernetes` service in the default namespace should remain.

**Verify:** Is everything cleaned up? **YES**

---

## Hints
- `selector` in a Service must match `labels` on the Pods — if they do not match, the Service routes traffic to nothing
- `kubectl get endpoints <service-name>` shows which Pod IPs a Service is currently routing to
- `port` is what the Service listens on; `targetPort` is what the Pod listens on — they do not have to be the same number
- NodePort range is 30000-32767; if you do not specify `nodePort`, Kubernetes picks one automatically
- Use `kubectl describe service <name>` to see the full configuration including Endpoints
- `kubectl get services -o wide` shows the selector each service uses
- To test ClusterIP services, you must test from inside the cluster (use a temporary pod)

---

## Documentation
Create `day-53-services.md` with:
- What problem Services solve and how they relate to Pods and Deployments
- Your three Service manifests with an explanation of each type
- The difference between ClusterIP, NodePort, and LoadBalancer
- How Kubernetes DNS works for service discovery
- What Endpoints are and how to inspect them
- Screenshot of your services and the test output

---

## Submission
1. Add `day-53-services.md` and your YAML files to `2026/day-53/`
2. Commit and push to your fork

---

## Learn in Public
Share on LinkedIn: "Learned Kubernetes Services today — ClusterIP for internal traffic, NodePort for node-level access, and LoadBalancer for production. Services give Pods a stable identity and load balancing."

`#90DaysOfDevOps` `#DevOpsKaJosh` `#TrainWithShubham`

Happy Learning!
**TrainWithShubham**
