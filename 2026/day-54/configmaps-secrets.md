# Kubernetes ConfigMaps and Secrets

## Task
Your application needs configuration — database URLs, feature flags, API keys. Hardcoding these into container images means rebuilding every time a value changes. Kubernetes solves this with ConfigMaps for non-sensitive config and Secrets for sensitive data.

---


## Challenge Tasks

### Task 1: Create a ConfigMap from Literals
1. Use `kubectl create configmap` with `--from-literal` to create a ConfigMap called `app-config` with keys `APP_ENV=production`, `APP_DEBUG=false`, and `APP_PORT=8080`
2. Inspect it with `kubectl describe configmap app-config` and `kubectl get configmap app-config -o yaml`
3. Notice the data is stored as plain text — no encoding, no encryption

**Verify:** Can you see all three key-value pairs?

<img width="1106" height="428" alt="image" src="https://github.com/user-attachments/assets/867b86a5-4746-4fec-ab31-75bc46e4085e" />
<img width="1110" height="285" alt="image" src="https://github.com/user-attachments/assets/2802e172-bbe7-4c49-90c0-e86b0a00f083" />

---

### Task 2: Create a ConfigMap from a File
1. Write a custom Nginx config file that adds a `/health` endpoint returning "healthy"
2. Create a ConfigMap from this file using `kubectl create configmap nginx-config --from-file=default.conf=<your-file>`
3. The key name (`default.conf`) becomes the filename when mounted into a Pod

<img width="653" height="218" alt="image" src="https://github.com/user-attachments/assets/8cc8314a-e0b1-4d62-94cf-adec58b60297" />

**Verify:** Does `kubectl get configmap nginx-config -o yaml` show the file contents?

<img width="1046" height="493" alt="image" src="https://github.com/user-attachments/assets/91c25b63-36d1-452e-8a32-fed5c79255b5" />

---

### Task 3: Use ConfigMaps in a Pod
1. Write a Pod manifest that uses `envFrom` with `configMapRef` to inject all keys from `app-config` as environment variables. Use a busybox container that prints the values.
2. Write a second Pod manifest that mounts `nginx-config` as a volume at `/etc/nginx/conf.d`. Use the nginx image.
3. Test that the mounted config works: `kubectl exec <pod> -- curl -s http://localhost/health`

<img width="753" height="239" alt="image" src="https://github.com/user-attachments/assets/ed20fef1-83fd-436b-85aa-ef5cf8dd89e3" />
<img width="1045" height="155" alt="image" src="https://github.com/user-attachments/assets/cb8c655e-f6d3-4877-b745-49cd118d7825" />

<img width="599" height="326" alt="image" src="https://github.com/user-attachments/assets/bf257e12-03b7-4f04-935b-4e3fd0e2a7f4" />
<img width="759" height="107" alt="image" src="https://github.com/user-attachments/assets/586e4365-b1d4-44d1-b0c3-ecf29d1c35ef" />

Use environment variables for simple key-value settings. Use volume mounts for full config files.

**Verify:** Does the `/health` endpoint respond? **YES**

---

### Task 4: Create a Secret
1. Use `kubectl create secret generic db-credentials` with `--from-literal` to store `DB_USER=admin` and `DB_PASSWORD=s3cureP@ssw0rd`
2. Inspect with `kubectl get secret db-credentials -o yaml` — the values are base64-encoded
3. Decode a value: `echo '<base64-value>' | base64 --decode`

<img width="1110" height="269" alt="image" src="https://github.com/user-attachments/assets/120a4989-ac5e-4074-a13d-1b31bb9c8b90" />

**base64 is encoding, not encryption.** Anyone with cluster access can decode Secrets. The real advantages are RBAC separation, tmpfs storage on nodes, and optional encryption at rest.

**Verify:** Can you decode the password back to plaintext? - **YES**

---

### Task 5: Use Secrets in a Pod
1. Write a Pod manifest that injects `DB_USER` as an environment variable using `secretKeyRef`
2. In the same Pod, mount the entire `db-credentials` Secret as a volume at `/etc/db-credentials` with `readOnly: true`
3. Verify: each Secret key becomes a file, and the content is the decoded plaintext value

<img width="779" height="270" alt="image" src="https://github.com/user-attachments/assets/4e45ad8b-28a0-49ec-86a5-27d2e44568e4" />

**Verify:** Are the mounted file values plaintext or base64? **Plain Text**

---

### Task 6: Update a ConfigMap and Observe Propagation
1. Create a ConfigMap `live-config` with a key `message=hello`
2. Write a Pod that mounts this ConfigMap as a volume and reads the file in a loop every 5 seconds
3. Update the ConfigMap: `kubectl patch configmap live-config --type merge -p '{"data":{"message":"world"}}'`
4. Wait 30-60 seconds — the volume-mounted value updates automatically
5. Environment variables from earlier tasks do NOT update — they are set at pod startup only

<img width="860" height="151" alt="image" src="https://github.com/user-attachments/assets/6d1c499e-5b47-4550-bbcf-9a58dcdeb739" />
<img width="858" height="213" alt="image" src="https://github.com/user-attachments/assets/2c318f0f-859d-4275-b164-13dfa00018cf" />
<img width="1027" height="538" alt="image" src="https://github.com/user-attachments/assets/809acd67-53bf-48e1-9b56-f8847800df81" />

**Verify:** Did the volume-mounted value change without a pod restart? **YES**

---

### Task 7: Clean Up
Delete all pods, ConfigMaps, and Secrets you created.

<img width="773" height="176" alt="image" src="https://github.com/user-attachments/assets/0e212c53-7d56-44b8-a3fd-c3ddd11ab52e" />

---

## Hints
- `--from-literal=KEY=VALUE` for command-line values, `--from-file=key=filename` for file contents
- `envFrom` injects all keys; `env` with `valueFrom` injects individual keys
- `echo -n 'value' | base64` — always use `-n` to avoid encoding a trailing newline
- Volume-mounted ConfigMaps/Secrets auto-update; environment variables do not
- `kubectl get secret <name> -o jsonpath='{.data.KEY}' | base64 --decode` extracts and decodes a value

---

## Documentation
Create `day-54-configmaps-secrets.md` with:
- What ConfigMaps and Secrets are and when to use each
- The difference between environment variables and volume mounts
- Why base64 is encoding, not encryption
- How ConfigMap updates propagate to volumes but not env vars

---
