
# Resource Requests, Limits, and Probes

### Task 1: Resource Requests and Limits
1. Write a Pod manifest with `resources.requests` (cpu: 100m, memory: 128Mi) and `resources.limits` (cpu: 250m, memory: 256Mi)
2. Apply and inspect with `kubectl describe pod` — look for the Requests, Limits, and QoS Class sections
3. Since requests and limits differ, the QoS class is `Burstable`. If equal, it would be `Guaranteed`. If missing, `BestEffort`.

<img width="1035" height="566" alt="image" src="https://github.com/user-attachments/assets/805b63ed-fe47-44be-b9b9-51fbfd37fe62" />
<img width="1103" height="467" alt="image" src="https://github.com/user-attachments/assets/17a5b9ac-4224-417f-a315-9f660701ef35" />

CPU is in millicores: `100m` = 0.1 CPU. Memory is in mebibytes: `128Mi`.

**Requests** = guaranteed minimum (scheduler uses this for placement). **Limits** = maximum allowed (kubelet enforces at runtime).

**Verify:** What QoS class does your Pod have? - **Brustable**

---

### Task 2: OOMKilled — Exceeding Memory Limits
1. Write a Pod manifest using the `polinux/stress` image with a memory limit of `100Mi`
2. Set the stress command to allocate 200M of memory: `command: ["stress"] args: ["--vm", "1", "--vm-bytes", "200M", "--vm-hang", "1"]`
3. Apply and watch — the container gets killed immediately

<img width="649" height="179" alt="image" src="https://github.com/user-attachments/assets/fd94b029-6b08-49d6-b3ef-c8d6b63d34a8" />

CPU is throttled when over limit. Memory is killed — no mercy.

Check `kubectl describe pod` for `Reason: OOMKilled` and `Exit Code: 137` (128 + SIGKILL).

<img width="1048" height="559" alt="image" src="https://github.com/user-attachments/assets/187067bd-3116-4334-98ee-526ebf2ff09e" />
<img width="1014" height="564" alt="image" src="https://github.com/user-attachments/assets/f36a06c2-3c6e-4870-aad0-a946e583e853" />

**Verify:** What exit code does an OOMKilled container have? **Exit Code - 137**

---

### Task 3: Pending Pod — Requesting Too Much
1. Write a Pod manifest requesting `cpu: 100` and `memory: 128Gi`
2. Apply and check — STATUS stays `Pending` forever

<img width="629" height="180" alt="image" src="https://github.com/user-attachments/assets/9120a1f6-f587-44a0-8d79-004ec227a3c9" />

3. Run `kubectl describe pod` and read the Events — the scheduler says exactly why: insufficient resources

<img width="950" height="499" alt="image" src="https://github.com/user-attachments/assets/3410b09e-ee7d-4ac1-b626-6ab45f164f0a" />
<img width="1103" height="169" alt="image" src="https://github.com/user-attachments/assets/524ee7bd-bb95-479c-9485-05226c72d621" />
  
**Verify:** What event message does the scheduler produce?

<img width="1105" height="112" alt="image" src="https://github.com/user-attachments/assets/a565b5e4-8702-4ed2-b69b-dbdb3443e327" />

---

### Task 4: Liveness Probe
A liveness probe detects stuck containers. If it fails, Kubernetes restarts the container.

1. Write a Pod manifest with a busybox container that creates `/tmp/healthy` on startup, then deletes it after 30 seconds
2. Add a liveness probe using `exec` that runs `cat /tmp/healthy`, with `periodSeconds: 5` and `failureThreshold: 3`

<img width="622" height="202" alt="image" src="https://github.com/user-attachments/assets/90696046-2167-48e5-a4b5-7d1e641fa9f3" />

3. After the file is deleted, 3 consecutive failures trigger a restart. Watch with `kubectl get pod -w`

<img width="611" height="124" alt="image" src="https://github.com/user-attachments/assets/97173f3c-a70a-4b69-8bba-379d09fbd525" />
<img width="1103" height="444" alt="image" src="https://github.com/user-attachments/assets/97a76753-b914-4b36-98bd-5d4735270ba6" />

**Verify:** How many times has the container restarted?

<img width="611" height="124" alt="image" src="https://github.com/user-attachments/assets/ceb6e908-2370-47b8-981a-e6d3f6908a42" />

---

### Task 5: Readiness Probe
A readiness probe controls traffic. Failure removes the Pod from Service endpoints but does NOT restart it.

1. Write a Pod manifest with nginx and a `readinessProbe` using `httpGet` on path `/` port `80`
2. Expose it as a Service: `kubectl expose pod <name> --port=80 --name=readiness-svc`
3. Check `kubectl get endpoints readiness-svc` — the Pod IP is listed
4. Break the probe: `kubectl exec <pod> -- rm /usr/share/nginx/html/index.html`
5. Wait 15 seconds — Pod shows `0/1` READY, endpoints are empty, but the container is NOT restarted

<img width="948" height="502" alt="image" src="https://github.com/user-attachments/assets/041f18d4-d1ab-49b4-8a4c-4f84767074ea" />
<img width="771" height="92" alt="image" src="https://github.com/user-attachments/assets/0f28df86-6073-4439-9d86-f36734e912a4" />

**Verify:** When readiness failed, was the container restarted? - **No Conainer**

---

### Task 6: Startup Probe
A startup probe gives slow-starting containers extra time. While it runs, liveness and readiness probes are disabled.

1. Write a Pod manifest where the container takes 20 seconds to start (e.g., `sleep 20 && touch /tmp/started`)
2. Add a `startupProbe` checking for `/tmp/started` with `periodSeconds: 5` and `failureThreshold: 12` (60 second budget)
3. Add a `livenessProbe` that checks the same file — it only kicks in after startup succeeds

<img width="1061" height="539" alt="image" src="https://github.com/user-attachments/assets/7996c127-464d-4496-8b11-6d7ecde28569" />
<img width="1104" height="355" alt="image" src="https://github.com/user-attachments/assets/73bc27fb-3ade-4062-a63f-9a72702a4d11" />
<img width="1104" height="350" alt="image" src="https://github.com/user-attachments/assets/805dc507-8dd2-4459-9016-cd09bf2104e9" />

**Verify:** What would happen if `failureThreshold` were 2 instead of 12?

<img width="669" height="106" alt="image" src="https://github.com/user-attachments/assets/f7f11fbe-7db2-45d0-826f-91c1f86752bb" />

---

### Task 7: Clean Up
Delete all pods and services you created.

<img width="716" height="313" alt="image" src="https://github.com/user-attachments/assets/0807c5b5-df56-4e37-b6b1-f4e1671a54f0" />

---

## Hints
- CPU is compressible (throttled); memory is incompressible (OOMKilled)
- CPU: `1` = 1 core = `1000m`. Memory: `Mi` (mebibytes), `Gi` (gibibytes)
- QoS: Guaranteed (requests == limits), Burstable (requests < limits), BestEffort (none set)
- Probe types: `httpGet`, `exec`, `tcpSocket`
- Liveness failure = restart. Readiness failure = remove from endpoints. Startup failure = kill.
- `initialDelaySeconds`, `periodSeconds`, `failureThreshold` control probe timing
- Exit code 137 = OOMKilled (128 + SIGKILL)


