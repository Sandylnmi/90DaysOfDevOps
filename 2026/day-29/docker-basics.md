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
   * `Docker Daemon:` This is the background service (dockerd) running on the host machine that manages all Docker objects like images, containers, networks, and storage volumes. It listens for API requests from the client and executes the necessary actions.
   * `Docker Client:` This is the primary way users interact with Docker, typically through a command-line interface (CLI). The client sends commands, such as docker run or docker pull, to the Docker daemon.
   * `Docker Images:` Images are read-only templates that contain the application code, libraries, dependencies, and configuration files required to run an application. They are built from a Dockerfile and serve as the blueprint for containers. Images are layered, making them efficient to store and transfer.
   * `Docker Containers:` A container is a runnable, isolated instance of a Docker image. It includes everything needed to run the application and shares the host machine's operating system kernel, making it lightweight and fast. You can create, start, stop, and delete containers using the Docker API or CLI.
   * `Docker Registry:` This is a stateless, scalable storage and distribution system for Docker images. The default public registry is Docker Hub. When a user runs docker pull or docker run, the daemon retrieves the required image from the registry. The docker push command uploads an image to a registry.

Workflow
 * Pulling: When you run docker pull, the client tells the daemon to fetch a specific image from a registry.
 * Building: Using docker build, the client sends a Dockerfile (a text file with instructions) to the daemon, which assembles a new image layer by layer.
 * Running: When you execute docker run, the daemon checks for the image locally, pulls it if necessary, and then creates and starts a container.
   
<img width="647" height="645" alt="image" src="https://github.com/user-attachments/assets/6e64c7c5-f19c-4103-b65a-2288e8cac375" />

---

### Task 2: Install Docker
1. Install Docker on your machine (or use a cloud instance)
2. Verify the installation
3. Run the `hello-world` container
4. Read the output carefully — it explains what just happened

<img width="593" height="468" alt="image" src="https://github.com/user-attachments/assets/2b97a886-057d-4e99-a71f-ef93bcf5aba7" />

---

### Task 3: Run Real Containers
1. Run an **Nginx** container and access it in your browser
<img width="786" height="261" alt="image" src="https://github.com/user-attachments/assets/89d5531f-ff6a-4f34-86c7-415182376801" />
<img width="918" height="679" alt="image" src="https://github.com/user-attachments/assets/bdc88127-09cf-4ba0-8b9c-5d978d72e88d" />

2. Run an **Ubuntu** container in interactive mode — explore it like a mini Linux machine
<img width="626" height="83" alt="image" src="https://github.com/user-attachments/assets/45248d15-4734-408e-8d48-559925218a42" />

3. List all running containers
4. List all containers (including stopped ones)
<img width="794" height="148" alt="image" src="https://github.com/user-attachments/assets/311855e7-4d34-4ada-9526-1766dd642e33" />

5. Stop and remove a container
<img width="436" height="57" alt="image" src="https://github.com/user-attachments/assets/92efcf66-5668-4462-86f3-84a92818489c" />

<img width="754" height="330" alt="image" src="https://github.com/user-attachments/assets/cc360f2e-e820-4e90-a205-963248ed5a39" />

---

### Task 4: Explore
1. Run a container in **detached mode** — what's different?
  Running a Docker container in detached mode (-d or --detach) runs it in the background, releasing your terminal immediately.
  Unlike the default foreground mode, it doesn't display container output, allowing continued terminal use.
  Key Differences When Running Detached (-d)
   * Background Operation: The container runs independently of your terminal session, allowing you to continue using the command line.
   * No Terminal Output: The container's stdout and stderr are not displayed, but you can view them later using the docker logs command.
   * Non-interactive: You cannot send input to the container (stdin).
   * Fast Return to Prompt: Instead of locking your screen with logs, Docker returns the container ID immediately.
   * Independent Lifecycle: Closing the terminal does not stop the container.
   * Signal Handling: Pressing Ctrl+c in the terminal will not stop the container, whereas in attached mode it will (by sending SIGINT).

 How to Manage Detached Containers.
   * View Running: Use docker ps to see the container.
   * View Logs: Use docker logs -f <container_id> to watch logs in real-time.
   * Reattach: Use docker attach <container_id to bring it back to the foreground.
   * Stop: Use docker stop <container_id>. 

2. Give a container a custom **name**
   To give a container a custom name in Docker, use the `--name` flag when running or creating the container. We can also rename an existing container using the `docker rename` command. 
   `Eg:- docker run --name <custom_name> <image_name>`
   
3. Map a **port** from the container to your host
   <img width="722" height="448" alt="image" src="https://github.com/user-attachments/assets/d71dc2b1-5e52-43c9-8846-8944a572487f" />
   <img width="956" height="90" alt="image" src="https://github.com/user-attachments/assets/a2b12c87-6b5e-420d-bd81-e0b332380e6b" />

4. Check **logs** of a running container
   <img width="715" height="300" alt="image" src="https://github.com/user-attachments/assets/d1eee643-dcf0-461b-9bf8-0204c8170d6a" />

5. Run a command **inside** a running container
  <img width="795" height="298" alt="image" src="https://github.com/user-attachments/assets/e90fd98e-84bb-45d3-9220-60a4fbe7fbf9" />


   
