# Docker Images & Container Lifecycle

### Task 1: Docker Images
1. Pull the `nginx`, `ubuntu`, and `alpine` images from Docker Hub
   `docker pull <image_name>`
   <img width="664" height="110" alt="image" src="https://github.com/user-attachments/assets/6c761087-b7af-4687-857b-829d429fa558" />

2. List all images on your machine — note the sizes
    
   <img width="550" height="113" alt="image" src="https://github.com/user-attachments/assets/8d864564-b7e4-4c71-a5f8-2508d73d32c2" />

3. Compare `ubuntu` vs `alpine` — why is one much smaller?
  * Ubuntu is a full-featured, widely compatible Linux distribution ideal for complex applications, while Alpine Linux is an ultra-lightweight (5MB), security-focused distro designed for minimal container footprint.
  * Use Alpine if: You are building Docker images where image size, quick deployment, and minimal resource usage are priorities.
  * Use Ubuntu if: You need maximum compatibility, a vast library of pre-installed tools, or are less concerned with container size and more with developer familiarity.

    <img width="616" height="331" alt="image" src="https://github.com/user-attachments/assets/1974b727-7a06-45be-bcac-554ff6aaaeb4" />
 
4. Inspect an image — what information can you see?
    * Image ID
    * Image Tag
    * Created Time
    * Default Config
      * Ports
      * Environment variables
      * Entrypoint
      * CMD
    * Architecture
    * OS
    * Size
    * Graph Driver
    * Layers

5. Remove an image you no longer need
    `docker rmi <image_name>`
   <img width="741" height="177" alt="image" src="https://github.com/user-attachments/assets/ca21fff5-7d4f-4453-935e-5ad3edb1dcbe" />

---

### Task 2: Image Layers
1. Run `docker image history nginx` — what do you see?
2. Each line is a **layer**. Note how some layers show sizes and some show 0B
3. Write in your notes: What are layers and why does Docker use them?

<img width="886" height="324" alt="image" src="https://github.com/user-attachments/assets/a4e7fe1f-cbb4-4196-8cc6-084e88e6b0fc" />

 `Docker image layers are created with every changes made to the file system.
  Every instruction in dockerfile creates a separate layer (FROM, COPY, RUN, CMD etc).`
  
---

### Task 3: Container Lifecycle
Practice the full lifecycle on one container:
1. **Create** a container (without starting it)
2. **Start** the container
3. **Pause** it and check status
4. **Unpause** it
5. **Stop** it
6. **Restart** it
7. **Kill** it
8. **Remove** it

Check `docker ps -a` after each step — observe the state changes.

<img width="1132" height="458" alt="image" src="https://github.com/user-attachments/assets/d8c7b5cb-ad07-42c0-9a83-b77659154fdb" />
<img width="1147" height="330" alt="image" src="https://github.com/user-attachments/assets/73acc9e0-1668-491d-ba45-cde87388629b" />

---

### Task 4: Working with Running Containers
1. Run an Nginx container in detached mode
  <img width="1081" height="102" alt="image" src="https://github.com/user-attachments/assets/f6cdcdbf-4d83-49c6-b450-d5bd1fca8b5e" />

2. View its **logs**
  <img width="827" height="296" alt="image" src="https://github.com/user-attachments/assets/7031952d-5daf-437f-ab13-aca8e966eb5a" />

3. View **real-time logs** (follow mode)
  <img width="1345" height="375" alt="image" src="https://github.com/user-attachments/assets/0146d76c-a9a7-4d4d-b7dd-8a3d7c315173" />

4. **Exec** into the container and look around the filesystem
  <img width="1213" height="598" alt="image" src="https://github.com/user-attachments/assets/5ffa95ff-459b-4154-a72a-627ea99368b0" />

5. Run a single command inside the container without entering it
  <img width="589" height="405" alt="image" src="https://github.com/user-attachments/assets/8b89d4b6-c236-4155-9f5d-1f0953a46fe4" />

6. **Inspect** the container — find its IP address, port mappings, and mounts
  <img width="908" height="668" alt="image" src="https://github.com/user-attachments/assets/af12b376-d822-464b-9168-cb920845e6b0" />

---

### Task 5: Cleanup
1. Stop all running containers in one command
    `docker stop $(docker ps -a -q)` This inner command lists all containers (-a) and outputs only their numerical IDs (-q)
    <img width="1242" height="268" alt="image" src="https://github.com/user-attachments/assets/4e4c3a0b-dacc-49eb-8796-c1b9ae247aeb" />

2. Remove all stopped containers in one command
    `docker rm $(docker ps -a -q)` receives the IDs of all now-stopped containers and removes them
    <img width="997" height="167" alt="image" src="https://github.com/user-attachments/assets/a8d340da-0e34-4a50-bc49-0452245cca1a" />

3. Remove unused images
    <img width="739" height="571" alt="image" src="https://github.com/user-attachments/assets/12843fe8-7cd7-4b32-9742-314ec3cc6253" />

4. Check how much disk space Docker is using
    <img width="687" height="260" alt="image" src="https://github.com/user-attachments/assets/01bbde16-91a6-4978-8313-f3696ba8f449" />

---

