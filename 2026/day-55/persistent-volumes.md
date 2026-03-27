# Persistent Volumes (PV) and Persistent Volume Claims (PVC)

---

## Challenge Tasks

### Task 1: See the Problem — Data Lost on Pod Deletion
1. Write a Pod manifest that uses an `emptyDir` volume and writes a timestamped message to `/data/message.txt`
2. Apply it, verify the data exists with `kubectl exec`
3. Delete the Pod, recreate it, check the file again — the old message is gone

<img width="452" height="368" alt="image" src="https://github.com/user-attachments/assets/f649d154-8948-45f4-ac78-57c9bf903501" />
<img width="598" height="418" alt="image" src="https://github.com/user-attachments/assets/53386fe9-8a06-4638-81b0-92f33ee903ef" />

**Verify:** Is the timestamp the same or different after recreation? - **DIFFERENT**

---

### Task 2: Create a PersistentVolume (Static Provisioning)
1. Write a PV manifest with `capacity: 1Gi`, `accessModes: ReadWriteOnce`, `persistentVolumeReclaimPolicy: Retain`, and `hostPath` pointing to `/tmp/k8s-pv-data`
2. Apply it and check `kubectl get pv` — status should be `Available`

<img width="367" height="306" alt="image" src="https://github.com/user-attachments/assets/8fe71212-e695-4c81-8fdf-752adc3bb056" />
<img width="1043" height="71" alt="image" src="https://github.com/user-attachments/assets/46ef958f-3b42-42b5-a3b6-249ac6cd2fb5" />

Access modes to know:
- `ReadWriteOnce (RWO)` — read-write by a single node
- `ReadOnlyMany (ROX)` — read-only by many nodes
- `ReadWriteMany (RWX)` — read-write by many nodes

`hostPath` is fine for learning, not for production.

**Verify:** What is the STATUS of the PV? -**AVAILABLE**

---

### Task 3: Create a PersistentVolumeClaim
1. Write a PVC manifest requesting `500Mi` of storage with `ReadWriteOnce` access
2. Apply it and check both `kubectl get pvc` and `kubectl get pv`
3. Both should show `Bound` — Kubernetes matched them by capacity and access mode

**Verify:** What does the VOLUME column in `kubectl get pvc` show? - _The VOLUME column in the output of kubectl get pvc shows the name of the specific Persistent Volume (PV) that the Persistent Volume Claim (PVC) is bound to._

<img width="1084" height="149" alt="image" src="https://github.com/user-attachments/assets/f2b49364-2b67-49f7-948d-7d3c06701c74" />

---

### Task 4: Use the PVC in a Pod — Data That Survives
1. Write a Pod manifest that mounts the PVC at `/data` using `persistentVolumeClaim.claimName`
2. Write data to `/data/message.txt`, then delete and recreate the Pod
3. Check the file — it should contain data from both Pods

**Verify:** Does the file contain data from both the first and second Pod?

<img width="824" height="291" alt="image" src="https://github.com/user-attachments/assets/44af99d0-bf3e-43a9-8179-a94c2cc025f9" />
<img width="597" height="529" alt="image" src="https://github.com/user-attachments/assets/b8306f1a-c908-43fb-8a3a-7ad7ae17556e" />

---

### Task 5: StorageClasses and Dynamic Provisioning
1. Run `kubectl get storageclass` and `kubectl describe storageclass`
2. Note the provisioner, reclaim policy, and volume binding mode
3. With dynamic provisioning, developers only create PVCs — the StorageClass handles PV creation automatically

**Verify:** What is the default StorageClass in your cluster?

---

### Task 6: Dynamic Provisioning
1. Write a PVC manifest that includes `storageClassName: standard` (or your cluster's default)
2. Apply it — a PV should appear automatically in `kubectl get pv`
3. Use this PVC in a Pod, write data, verify it works

**Verify:** How many PVs exist now? Which was manual, which was dynamic?

---

### Task 7: Clean Up
1. Delete all pods first
2. Delete PVCs — check `kubectl get pv` to see what happened
3. The dynamic PV is gone (Delete reclaim policy). The manual PV shows `Released` (Retain policy).
4. Delete the remaining PV manually

**Verify:** Which PV was auto-deleted and which was retained? Why?

---

## Hints
- PVs are cluster-wide (not namespaced), PVCs are namespaced
- PV status: `Available` -> `Bound` -> `Released`
- If a PVC stays `Pending`, check for matching capacity and access modes
- `hostPath` data is lost if the Pod moves to a different node
- `storageClassName: ""` disables dynamic provisioning
- Reclaim policies: `Retain` (keep data) vs `Delete` (remove data)

---

## Documentation
Create `day-55-persistent-volumes.md` with:
- Why containers need persistent storage
- What PVs and PVCs are and how they relate
- Static vs dynamic provisioning
- Access modes and reclaim policies

---
