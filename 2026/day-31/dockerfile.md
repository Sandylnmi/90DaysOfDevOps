# Dockerfile: Build Your Own Images



### Task 1: Your First Dockerfile
1. Create a folder called `my-first-image`
2. Inside it, create a `Dockerfile` that:
   - Uses `ubuntu` as the base image
   - Installs `curl`
   - Sets a default command to print `"Hello from my custom image!"`
3. Build the image and tag it `my-ubuntu:v1`
4. Run a container from your image

  <img width="811" height="497" alt="image" src="https://github.com/user-attachments/assets/1257a94a-6688-46f2-a3b9-52bc428b0daa" />
  <img width="606" height="208" alt="image" src="https://github.com/user-attachments/assets/28a5276f-cab8-4f70-a4c5-b312bd8a7639" />

**Verify:** The message prints on `docker run`

---

### Task 2: Dockerfile Instructions
Create a new Dockerfile that uses **all** of these instructions:
- `FROM` — base image
- `RUN` — execute commands during build
- `COPY` — copy files from host to image
- `WORKDIR` — set working directory
- `EXPOSE` — document the port
- `CMD` — default command

<img width="491" height="414" alt="image" src="https://github.com/user-attachments/assets/5291b14e-3fab-43e4-ac4b-ad88b0ed5fc8" />

Build and run it. Understand what each line does.

<img width="1259" height="125" alt="image" src="https://github.com/user-attachments/assets/aef195e7-37cf-4554-ac67-553548a56427" />

---

### Task 3: CMD vs ENTRYPOINT
1. Create an image with `CMD ["echo", "hello"]` — run it, then run it with a custom command. What happens?
2. Create an image with `ENTRYPOINT ["echo"]` — run it, then run it with additional arguments. What happens?

<img width="835" height="600" alt="image" src="https://github.com/user-attachments/assets/f7d7f48d-94c5-4aeb-bb0a-412c22ebcb79" />

3. Write in your notes: When would you use CMD vs ENTRYPOINT?

   `ENTRYPOINT` when we want our container to behave like a specific executable, ensuring a particular command always runs.
   `CMD` to provide flexible default values or commands that users can easily override at runtime for utility or ad-hoc tasks.

* CMD without ENTRYPOINT: The container runs the CMD instruction, but any arguments provided in the docker run command replace the entire CMD instruction.
   - Example: CMD ["echo", "Hello World!"]. Running docker run myimage "Goodbye!" will output "Goodbye!" (overriding "Hello World!").
* ENTRYPOINT without CMD: The container runs the ENTRYPOINT command, and any docker run arguments are appended to it.
   - Example: ENTRYPOINT ["echo", "Hello"]. Running docker run myimage "World!" will output "Hello World!" (appending "World!" as an argument).
* ENTRYPOINT and CMD Together (Recommended Pattern): This is the most common and powerful use case, effectively creating an executable with default, overridable options.
   - Example: ENTRYPOINT ["echo", "Hello"], CMD ["World!"]. Running docker run myimage outputs "Hello World!". Running docker run myimage "KodeKloud!" outputs "Hello KodeKloud!" (overriding the CMD argument)

---

### Task 4: Build a Simple Web App Image
1. Create a small static HTML file (`index.html`) with any content
2. Write a Dockerfile that:
   - Uses `nginx:alpine` as base
   - Copies your `index.html` to the Nginx web directory

<img width="518" height="99" alt="image" src="https://github.com/user-attachments/assets/805bd7d2-e6a4-4075-b29a-52213b7f81d6" />

3. Build and tag it `my-website:v1`
4. Run it with port mapping and access it in your browser

<img width="1351" height="411" alt="image" src="https://github.com/user-attachments/assets/ec3b329f-b8cc-44ea-8d6d-f12530da1ea6" />
<img width="1272" height="595" alt="image" src="https://github.com/user-attachments/assets/38af8d34-6a97-414f-a4cd-e5dd1b1cce9c" />

---

### Task 5: .dockerignore
1. Create a `.dockerignore` file in one of your project folders
2. Add entries for: `node_modules`, `.git`, `*.md`, `.env`
3. Build the image — verify that ignored files are not included

<img width="311" height="199" alt="image" src="https://github.com/user-attachments/assets/273d0a4e-0321-46de-9ac5-b01124c873fb" />
<img width="619" height="633" alt="image" src="https://github.com/user-attachments/assets/6aac10cf-7304-4a3e-aec6-bd3d257cb6b6" />


---

### Task 6: Build Optimization
1. Build an image, then change one line and rebuild — notice how Docker uses **cache**
2. Reorder your Dockerfile so that frequently changing lines come **last**
3. Write in your notes: Why does layer order matter for build speed?

---
