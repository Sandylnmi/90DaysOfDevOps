# Kubernetes Namespaces and Deployments

## Expected Output
- At least 2 namespaces created and used
- A Deployment running with multiple replicas
- A scaled Deployment and a rolling update performed
- A markdown file: `day-52-namespaces-deployments.md`
- Screenshot of `kubectl get deployments` and `kubectl get pods` across namespaces

---

## Challenge Tasks

### Task 1: Explore Default Namespaces
Kubernetes comes with built-in namespaces. List them:

```bash
kubectl get namespaces
```
<img width="613" height="135" alt="image" src="https://github.com/user-attachments/assets/54b698d5-10fd-4acc-ab4b-51f1a0f14c7d" />

You should see at least:
- `default` — where your resources go if you do not specify a namespace
- `kube-system` — Kubernetes internal components (API server, scheduler, etc.)
- `kube-public` — publicly readable resources
- `kube-node-lease` — node heartbeat tracking

Check what is running inside `kube-system`:
```bash
kubectl get pods -n kube-system
```
<img width="841" height="237" alt="image" src="https://github.com/user-attachments/assets/f37fc405-89c9-4da5-b039-8fe5c21ba946" />

These are the control plane components keeping your cluster alive. Do not touch them.

**Verify:** How many pods are running in `kube-system`-**12**

---

### Task 2: Create and Use Custom Namespaces
Create two namespaces — one for a development environment and one for staging:

```bash
kubectl create namespace dev
kubectl create namespace staging
```

Verify they exist:
```bash
kubectl get namespaces
```
<img width="647" height="223" alt="image" src="https://github.com/user-attachments/assets/741bb1d5-ca32-46c8-8f94-d7114890c965" />

You can also create a namespace from a manifest:
```yaml
# namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: production
```

```bash
kubectl apply -f namespace.yaml
```
<img width="643" height="226" alt="image" src="https://github.com/user-attachments/assets/d5a64542-4409-471e-8aa4-f6a40feab6a4" />

Now run a pod in a specific namespace:
```bash
kubectl run nginx-dev --image=nginx:latest -n dev
kubectl run nginx-staging --image=nginx:latest -n staging
```

List pods across all namespaces:
```bash
kubectl get pods -A
```
<img width="991" height="403" alt="image" src="https://github.com/user-attachments/assets/f876e24f-cb5a-4168-9e54-7e04f463ef00" />
<img width="628" height="115" alt="image" src="https://github.com/user-attachments/assets/3137d655-bdfe-4aa6-8603-870156a08076" />

Notice that `kubectl get pods` without `-n` only shows the `default` namespace. You must specify `-n <namespace>` or use `-A` to see everything.

**Verify:** Does `kubectl get pods` show these pods? What about `kubectl get pods -A`?

<img width="1013" height="373" alt="image" src="https://github.com/user-attachments/assets/e5ab706e-5a96-47f5-8230-59ce88cb797c" />

---

### Task 3: Create Your First Deployment
A Deployment tells Kubernetes: "I want X replicas of this Pod running at all times." If a Pod crashes, the Deployment controller recreates it automatically.

Create a file `nginx-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: dev
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
        ports:
        - containerPort: 80
```

Key differences from a standalone Pod:
- `kind: Deployment` instead of `kind: Pod`
- `apiVersion: apps/v1` instead of `v1`
- `replicas: 3` tells Kubernetes to maintain 3 identical pods
- `selector.matchLabels` connects the Deployment to its Pods
- `template` is the Pod template — the Deployment creates Pods using this blueprint

Apply it:
```bash
kubectl apply -f nginx-deployment.yaml
```

Check the result:
```bash
kubectl get deployments -n dev
kubectl get pods -n dev
```
<img width="890" height="361" alt="image" src="https://github.com/user-attachments/assets/1ee07c43-fce6-4de1-89f5-980281630d95" />

You should see 3 pods with names like `nginx-deployment-xxxxx-yyyyy`.

**Verify:** What do the READY, UP-TO-DATE, and AVAILABLE columns mean in the deployment output? 

The READY, UP-TO-DATE, and AVAILABLE columns in a Kubernetes `kubectl get deployments` output indicate the status of your application's rollout and health. 

---

### Task 4: Self-Healing — Delete a Pod and Watch It Come Back
This is the key difference between a Deployment and a standalone Pod.

```bash
# List pods
kubectl get pods -n dev

# Delete one of the deployment's pods (use an actual pod name from your output)
kubectl delete pod <pod-name> -n dev

# Immediately check again
kubectl get pods -n dev
```
<img width="850" height="323" alt="image" src="https://github.com/user-attachments/assets/d2efcaaa-abe1-469b-a239-2f6dcf3e03a1" />

The Deployment controller detects that only 2 of 3 desired replicas exist and immediately creates a new one. The deleted pod is replaced within seconds.

**Verify:** Is the replacement pod's name the same as the one you deleted, or different? - **New or replaced pod name is different**

---

### Task 5: Scale the Deployment
Change the number of replicas:

```bash
# Scale up to 5
kubectl scale deployment nginx-deployment --replicas=5 -n dev
kubectl get pods -n dev
```
<img width="890" height="293" alt="image" src="https://github.com/user-attachments/assets/5aa759f1-97a0-47d3-97b5-72c146e5b5be" />

```
# Scale down to 2
kubectl scale deployment nginx-deployment --replicas=2 -n dev
kubectl get pods -n dev
```

<img width="869" height="248" alt="image" src="https://github.com/user-attachments/assets/24d2eb4e-48e6-4ba0-abc2-c5ab80ffae6a" />

Watch how Kubernetes creates or terminates pods to match the desired count.

You can also scale by editing the manifest — change `replicas: 4` in your YAML file and run `kubectl apply -f nginx-deployment.yaml` again.

**Verify:** When you scaled down from 5 to 2, what happened to the extra pods?

Extra pods are terminated, kubernetes only keeps desired number of replicas - it deletes or creates pods to match desired state.

---

### Task 6: Rolling Update
Update the Nginx image version to trigger a rolling update:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.25 -n dev
```

Watch the rollout in real time:
```bash
kubectl rollout status deployment/nginx-deployment -n dev
```

Kubernetes replaces pods one by one — old pods are terminated only after new ones are healthy. This means zero downtime.

<img width="989" height="186" alt="image" src="https://github.com/user-attachments/assets/d4b0ecd6-b518-4179-ab2d-980fe5f81551" />

Check the rollout history:
```bash
kubectl rollout history deployment/nginx-deployment -n dev
```

Now roll back to the previous version:
```bash
kubectl rollout undo deployment/nginx-deployment -n dev
kubectl rollout status deployment/nginx-deployment -n dev
```
<img width="954" height="141" alt="image" src="https://github.com/user-attachments/assets/2565add4-200b-4346-9bb8-0a9fa9f4f40c" />

Verify the image is back to the previous version:
```bash
kubectl describe deployment nginx-deployment -n dev | grep Image
```
<img width="829" height="53" alt="image" src="https://github.com/user-attachments/assets/c0121542-24e0-43ad-aa51-4a75724f601e" />
<img width="870" height="57" alt="image" src="https://github.com/user-attachments/assets/a069ebed-0c92-4468-a7f1-bbe570792e35" />

**Verify:** What image version is running after the rollback?

---

### Task 7: Clean Up
```bash
kubectl delete deployment nginx-deployment -n dev
kubectl delete pod nginx-dev -n dev
kubectl delete pod nginx-staging -n staging
kubectl delete namespace dev staging production
```
<img width="819" height="171" alt="image" src="https://github.com/user-attachments/assets/3ae7ee89-0f66-4a8f-bc39-cfbe9bfbc5d0" />

Deleting a namespace removes everything inside it. Be very careful with this in production.

```bash
kubectl get namespaces
kubectl get pods -A
```
<img width="1004" height="381" alt="image" src="https://github.com/user-attachments/assets/c00f0585-3e22-4452-b5a2-77a51359d2f0" />

**Verify:** Are all your resources gone?

---

## Hints
- `kubectl get <resource> -n <namespace>` — target a specific namespace
- `kubectl get <resource> -A` — list resources across all namespaces
- `selector.matchLabels` in a Deployment must match `template.metadata.labels` — if they do not match, the Deployment will not manage the Pods
- `kubectl scale deployment <name> --replicas=N` — quick way to scale
- `kubectl set image` updates a container image without editing the YAML
- `kubectl rollout undo` rolls back to the previous revision
- `kubectl rollout history` shows past revisions of a Deployment
- Deployments create ReplicaSets behind the scenes — you can see them with `kubectl get replicasets -n <namespace>`

---

## Documentation

1. What namespaces are and why you would use them?
  
  - Namespaces are used to organize and isolate deployemnts/pods.
  - Namespaces are containers that organize related code (classes, functions, constants) or system resources into distinct, named groups to prevent naming conflicts. Similar to file folders, they allow the same name to be used for different entities in a project, improving code organization, maintainability, and security.
  - Namespaces in Kubernetes are virtual clusters within a single physical cluster, providing a mechanism to isolate, organize, and manage groups of resources (pods, services, deployments).
  - They enable multiple teams, projects, or environments (e.g., dev, test, prod) to share a single cluster without interfering with one another.
  - Namespaces cannot be nested.
  - Nodes and PersistentVolumes are not namespaced; they are cluster-wide resources.
  - We use the -n or --namespace flag to interact with resources in a specific namespace.

  <img width="579" height="204" alt="image" src="https://github.com/user-attachments/assets/661850eb-1858-44b4-a256-d13f72b89ced" />

2. Your Deployment manifest and an explanation of each section
 
 * `apiVersion`: `apps/v1`
 * `kind`: `deployment`
 * `metadata` - contains (name, namespace, labels)
 * `spec` - deployments specifications
 * `repicas` - how many replicas to create
 * `selector` - matches pods with label `app: nginx`
 * `template.metadata` - contains pods labels, must match the selector (app: nginx)
 * `template.spec` - pods specification (containers,name,image,port)

3. What happens when you delete a Pod managed by a Deployment vs a standalone Pod
  
  - Deleting a Pod managed by a Deployment causes Kubernetes to automatically create a new, identical Pod to maintain the desired replica count.
  - Deleting a standalone Pod (not managed by a Deployment/ReplicaSet) terminates the Pod permanently, and it is not rescheduled or recreated, leading to a permanent loss.

4. How scaling works (both imperative and declarative)
  
 - Scaling works through two primary paradigms: imperative (how to do it) and declarative (what to do).
 - In imperative scaling, we are responsible for the exact steps to scale the system. This approach provides granular control over the process.

  <img width="668" height="227" alt="image" src="https://github.com/user-attachments/assets/fb53b36d-4153-4f92-b7e9-1844c4859cff" />

 - Declarative scaling focuses on the end result. We define the required state, and the system works to achieve that state

  <img width="633" height="234" alt="image" src="https://github.com/user-attachments/assets/001cc2d2-7d1d-438e-a3c8-766925390c72" />

5. How rolling updates and rollbacks work
   
   - Rolling updates and rollbacks are deployment strategies that ensure application updates occur with zero downtime. Rolling updates incrementally replace old instances with new ones in batches.
   - Rollbacks quickly reverse these changes to the previous stable state if issues arise, both managed by orchestrators like Kubernetes or Docker Swarm.  
 
    <img width="671" height="276" alt="image" src="https://github.com/user-attachments/assets/58921a04-5ef3-4a15-840c-c0f3a308b47c" />
    <img width="688" height="286" alt="image" src="https://github.com/user-attachments/assets/6b3369a1-6294-42c7-88e4-73dc70cf36ae" />

 6. Screenshot of your Deployment and Pods running
  
---


