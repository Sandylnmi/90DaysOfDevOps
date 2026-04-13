# Kubernetes StatefulSets


## Challenge Tasks

### Task 1: Understand the Problem
1. Create a Deployment with 3 replicas using nginx

<img width="348" height="367" alt="image" src="https://github.com/user-attachments/assets/1b51d2e2-6c4e-42af-bc14-d4075a2957f0" />

2. Check the pod names — they are random (`app-xyz-abc`)
3. Delete a pod and notice the replacement gets a different random name

<img width="822" height="211" alt="image" src="https://github.com/user-attachments/assets/32562704-736e-438b-a675-3bd4e08b6116" />

This is fine for web servers but not for databases where you need stable identity.

**Verify:** Why would random pod names be a problem for a database cluster?

 1. Random pod names (using Deployment hashes) break database clusters because they lack stable network identities, making it impossible for nodes to reliably reconnect, maintain persistent storage, or identify primary/secondary roles.
 2. Database clusters require consistent, ordered identities (like db-0, db-1) to manage stateful replication and configuration, whereas random names cause constant re-configuration and potential data corruption.
 3. Key Problems with Random Pod Names in Databases.
    1. Broken Replication/State: Databases (e.g., PostgreSQL, MySQL) need to know which pod is the primary and which are replicas. If pods get new random names, the cluster loses track of which node holds which data, breaking replication.
    2. Storage Mismatch: Stateful apps need to remount the same Persistent Volume (PV) to the same pod. If a pod dies and is replaced with a new random name, it can lose access to its data.
    3. Networking Configuration: If a database node uses its hostname or pod name in configuration files, random naming prevents reconnection, as the IP or hostname changes on every crash or update.
    4. Non-deterministic Ordering: When scaling down, Kubernetes deployments destroy random pods. For a database, killing the primary (leader) instead of a follower results in unnecessary downtime.

`Solution: Use StatefulSets instead of Deployments for database pods. StatefulSets provide stable network identifiers (pod-name-0, pod-name-1) and stable storage bindings.`

---

### Task 2: Create a Headless Service
1. Write a Service manifest with `clusterIP: None` — this is a Headless Service
2. Set the selector to match the labels you will use on your StatefulSet pods
3. Apply it and confirm CLUSTER-IP shows `None`

A Headless Service creates individual DNS entries for each pod instead of load-balancing to one IP. StatefulSets require this.

<img width="679" height="69" alt="image" src="https://github.com/user-attachments/assets/a7e48b82-11e5-4d40-b8e0-527e0d53e3ec" />

**Verify:** What does the CLUSTER-IP column show? - **None**

Key details about the CLUSTER-IP column:-
 1. Internal Access Only: It provides an IP address that is only reachable from within the Kubernetes cluster, not from outside.
 2. Stable Address: It is a stable, consistent endpoint for a set of backend pods, which does not change during the service lifecycle.
 3. Load Balancing: When pods communicate with this IP, Kubernetes load-balances traffic across the corresponding backend pods.
 4. Default Behavior: If the service type is not specified when creating a service, it defaults to ClusterIP, and this column will show the assigned IP.
 5. <none>: If a service does not have an assigned cluster IP (such as a Headless Service), this column might show <none>, though a dedicated ClusterIP is the standard default. 

---

### Task 3: Create a StatefulSet
1. Write a StatefulSet manifest with `serviceName` pointing to your Headless Service
2. Set replicas to 3, use the nginx image
3. Add a `volumeClaimTemplates` section requesting 100Mi of ReadWriteOnce storage
4. Apply and watch: `kubectl get pods -l <your-label> -w`

<img width="702" height="103" alt="image" src="https://github.com/user-attachments/assets/b8964370-3684-4b5a-b5d1-5f7ad3131272" />

<img width="651" height="172" alt="image" src="https://github.com/user-attachments/assets/f5552c0f-9f25-4a26-93b4-f460e58ef75d" />

Observe ordered creation — `web-0` first, then `web-1` after `web-0` is Ready, then `web-2`.

Check the PVCs: `kubectl get pvc` — you should see `web-data-web-0`, `web-data-web-1`, `web-data-web-2` (names follow the pattern `<template-name>-<pod-name>`).

<img width="1103" height="170" alt="image" src="https://github.com/user-attachments/assets/177b8eb2-fb80-4da5-8ca0-61dce48cef72" />

**Verify:** What are the exact pod names and PVC names?

>Pod Names: `statefulset-0` `statefulset-1` `statefulset-2`

>PVC Names: `volume-statefulset-0` `volume-statefulset-1` `volume-statefulset-2`

---

### Task 4: Stable Network Identity
Each StatefulSet pod gets a DNS name: `<pod-name>.<service-name>.<namespace>.svc.cluster.local`

1. Run a temporary busybox pod and use `nslookup` to resolve `web-0.<your-headless-service>.default.svc.cluster.local`
2. Do the same for `web-1` and `web-2`
3. Confirm the IPs match `kubectl get pods -o wide`

<img width="1108" height="498" alt="image" src="https://github.com/user-attachments/assets/a6ee2a37-52d0-42e1-b2d8-27733efa922c" />
<img width="1101" height="193" alt="image" src="https://github.com/user-attachments/assets/dc6bc51b-f8b2-484e-bab3-1dcd7c0e9749" />
<img width="1104" height="99" alt="image" src="https://github.com/user-attachments/assets/db27a607-9a4f-48ae-ae20-97ed18d04783" />
  
**Verify:** Does the nslookup IP match the pod IP? **Yes**

---

### Task 5: Stable Storage — Data Survives Pod Deletion
1. Write unique data to each pod: `kubectl exec web-0 -- sh -c "echo 'Data from web-0' > /usr/share/nginx/html/index.html"`
2. Delete `web-0`: `kubectl delete pod web-0`
3. Wait for it to come back, then check the data — it should still be "Data from web-0"

<img width="1106" height="183" alt="image" src="https://github.com/user-attachments/assets/6d89161a-96da-4dbe-93cd-c5402a10cfb0" />

The new pod reconnected to the same PVC.

**Verify:** Is the data identical after pod recreation? - **Yes**

---

### Task 6: Ordered Scaling
1. Scale up to 5: `kubectl scale statefulset web --replicas=5` — pods create in order (web-3, then web-4)

<img width="872" height="436" alt="image" src="https://github.com/user-attachments/assets/7c35fc35-7325-4ae5-8d05-e909af3827c9" />

2. Scale down to 3 — pods terminate in reverse order (web-4, then web-3)

<img width="1048" height="296" alt="image" src="https://github.com/user-attachments/assets/47537c61-53e3-4fa5-a29c-e747f9e192dd" />

3. Check `kubectl get pvc` — all five PVCs still exist. Kubernetes keeps them on scale-down so data is preserved if you scale back up.

<img width="1109" height="226" alt="image" src="https://github.com/user-attachments/assets/d671beb2-df22-4f8c-996e-2f83e455ef90" />

**Verify:** After scaling down, how many PVCs exist? **5 PVC's**

---

### Task 7: Clean Up
1. Delete the StatefulSet and the Headless Service
2. Check `kubectl get pvc` — PVCs are still there (safety feature)
3. Delete PVCs manually

<img width="761" height="237" alt="image" src="https://github.com/user-attachments/assets/83d45292-e17b-4d3d-984d-37aebffdb8fa" />

**Verify:** Were PVCs auto-deleted with the StatefulSet? -**No**


