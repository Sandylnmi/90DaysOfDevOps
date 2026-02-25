# Introduction to Docker


### Task 1: What is Docker?
Research and write short notes on:
- What is a container and why do we need them?
  * A container is a lightweight, standalone, and executable package of software that bundles application code with all its dependencies—libraries, runtime, and configuration files—ensuring it runs consistently across any infrastructure.
  * They are essential for modern software development because they provide portability, isolation, and speed, eliminating the "it works on my machine" problem by allowing software to run the same way on a laptop, testing, or production server.
  * We Need Containers?
      * Consistency Across Environments: They solve the "it works on my machine" problem by ensuring the application runs identically in development, testing, and production.
      * Portability: Containers can run anywhere (physical servers, VMs, public/private clouds) without modifications.
      * Lightweight & Efficient: Unlike Virtual Machines, containers share the host's OS kernel, making them faster to start and requiring less memory and CPU.
      * Isolation & Security: Containers keep applications isolated from each other, which increases security and reduces conflicts between dependencies.
      * Microservices & DevOps: They are ideal for breaking down applications into smaller, manageable services (microservices) and fit perfectly into DevOps workflows for CI/CD.
        
- Containers vs Virtual Machines — what's the real difference?
     * `Containers` share the host operating system's kernel, making them lightweight, fast, and ideal for microservices.
     * `Virtual Machines (VMs)` include a full guest OS and virtualized hardware, offering stronger isolation and flexibility for different OS types at the cost of higher resource usage.
     * Containers are smaller (MBs), while VMs are larger (GBs)

<img width="647" height="345" alt="image" src="https://github.com/user-attachments/assets/5ba68d17-6626-4f2d-9e6d-e7ffd5e31945" />

- What is the Docker architecture? (daemon, client, images, containers, registry)




<img width="595" height="323" alt="image" src="https://github.com/user-attachments/assets/b117100e-67e9-4021-8946-c6e953f88c3e" />
<img width="771" height="908" alt="image" src="https://github.com/user-attachments/assets/6e64c7c5-f19c-4103-b65a-2288e8cac375" />

Draw or describe the Docker architecture in your own words.

---

### Task 2: Install Docker
1. Install Docker on your machine (or use a cloud instance)
2. Verify the installation
3. Run the `hello-world` container
4. Read the output carefully — it explains what just happened

---

### Task 3: Run Real Containers
1. Run an **Nginx** container and access it in your browser
2. Run an **Ubuntu** container in interactive mode — explore it like a mini Linux machine
3. List all running containers
4. List all containers (including stopped ones)
5. Stop and remove a container

---

### Task 4: Explore
1. Run a container in **detached mode** — what's different?
2. Give a container a custom **name**
3. Map a **port** from the container to your host
4. Check **logs** of a running container
5. Run a command **inside** a running container
