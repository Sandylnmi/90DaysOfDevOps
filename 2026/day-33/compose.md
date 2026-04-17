# Docker Compose: Multi-Container Basics

## Challenge Tasks

### Task 1: Install & Verify
1. Check if Docker Compose is available on your machine
2. Verify the version

<img width="746" height="90" alt="image" src="https://github.com/user-attachments/assets/07f8bf9c-9b2c-4735-b9f7-44b71567c846" />

---

### Task 2: Your First Compose File
1. Create a folder `compose-basics`
2. Write a `docker-compose.yml` that runs a single **Nginx** container with port mapping
3. Start it with `docker compose up`
4. Access it in your browser
5. Stop it with `docker compose down`

---

### Task 3: Two-Container Setup
Write a `docker-compose.yml` that runs:
- A **WordPress** container
- A **MySQL** container

They should:
- Be on the same network (Compose does this automatically)
- MySQL should have a named volume for data persistence
- WordPress should connect to MySQL using the service name

<img width="1109" height="510" alt="image" src="https://github.com/user-attachments/assets/35b4d19b-d634-40ad-8561-37d64a67784a" />
<img width="1105" height="425" alt="image" src="https://github.com/user-attachments/assets/d7ac66e4-55db-42d0-9184-099111ba8317" />

Start it, access WordPress in your browser, and set it up.

<img width="1366" height="609" alt="image" src="https://github.com/user-attachments/assets/c81103ab-8abf-4b56-a585-6173c4fb963a" />
<img width="1364" height="611" alt="image" src="https://github.com/user-attachments/assets/3ce63d1d-ac2c-42fe-be2e-d2553d72bf59" />

**Verify:** Stop and restart with `docker compose down` and `docker compose up` — is your WordPress data still there?

<img width="1104" height="185" alt="image" src="https://github.com/user-attachments/assets/378a8ec9-7343-4055-8628-6e4f8c5e91cf" />

---

### Task 4: Compose Commands
Practice and document these:
1. Start services in **detached mode**

<img width="1108" height="105" alt="image" src="https://github.com/user-attachments/assets/1818b447-a7b5-4e69-93eb-6d64f1463c24" />

2. View running services

<img width="1106" height="109" alt="image" src="https://github.com/user-attachments/assets/04df2de2-ef71-4562-9421-cfe1c5ef72e6" />

3. View **logs** of all services

<img width="1106" height="444" alt="image" src="https://github.com/user-attachments/assets/f1687fbc-12ac-4784-8846-5b91a2f3b7f1" />

4. View logs of a **specific** service

<img width="1101" height="455" alt="image" src="https://github.com/user-attachments/assets/ed4c0fd1-274c-4541-9b82-cc520eff4087" />

5. **Stop** services without removing

<img width="1105" height="91" alt="image" src="https://github.com/user-attachments/assets/c286d4a7-aec9-48df-a98d-5b1d18486513" />

6. **Remove** everything (containers, networks)

<img width="1103" height="289" alt="image" src="https://github.com/user-attachments/assets/b42a3b41-8259-4c27-a5f8-6f2075ba732d" />

7. **Rebuild** images if you make a change

<img width="1101" height="110" alt="image" src="https://github.com/user-attachments/assets/53f23d4f-6cd8-4c4b-a620-dcf468ffb6f2" />

---

### Task 5: Environment Variables
1. Add environment variables directly in your `docker-compose.yml`
2. Create a `.env` file and reference variables from it in your compose file
3. Verify the variables are being picked up

---

## Hints
- Start: `docker compose up -d`
- Stop: `docker compose down`
- Logs: `docker compose logs -f`
- Compose creates a default network for all services automatically
- Service names in compose are the DNS names containers use to talk to each other

Happy Learning!
**TrainWithShubham**
