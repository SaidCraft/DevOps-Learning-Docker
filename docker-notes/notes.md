# Docker Notes
This repository is a personal learning resource to document and reinforce my understanding of Docker. It covers all the key concepts, benefits, syntax, and commands related to Docker, along with hands-on examples demonstrating how Docker fits into modern DevOps workflows.

### What is Docker?

- Docker is a tool that allows you to **build, package,** and **run applications** as **containers**. It simplifies app deployment by ensuring your app runs the same in any environment.

###  What is a Container?

- A container is a lightweight, portable unit that packages your application, dependencies, and environment settings all in one.


###  Key benefits of Containers:
- **Isolation -** Each container runs independently from others and the host, preventing conflicts.
- **Consistency -** Avoids the "it works on my machine" issue by bundling all depedencies.
- **Portability -** Containers run anywhere Docker is supported (e.g., laptops).
- **Efficiency -** Share the host OS kernel, making it more lightweight than VMs. Uses fewer resources and starts almost instantly.

## Key Components of Docker


| **Component**       | **Definition**                                                                 | **Command Examples**              |
|---------------------|--------------------------------------------------------------------------------|-----------------------------------|
| Dockerfile          | A text file containing step-by-step instructions to build a Docker image       | *(You write this manually)*       |
| Docker Image        | A read-only, packaged snapshot of your application built from a Dockerfile     | `docker build -t myapp .`         |
| Docker Container    | A running instance of a Docker image                                           | `docker run myapp`                |
| Start Container     | Start a stopped container                                                      | `docker start <container_id>`     |
| Stop Container      | Stop a running container                                                       | `docker stop <container_id>`      |
| Remove Container    | Delete a container from the system                                             | `docker rm <container_id>`        |
| Remove Image        | Delete a Docker image                                                          | `docker rmi myapp`                |

### Understanding the `-p` Flag
The `-p` flag in Docker is used to **expose a container's internal post to the host machine,** allowing applcications running inside the container to be accessed from outside the container - often referred to as the ""outside world."

**What is the "outside world"?**

In simple terms, the "outside world" refers to **anything outside the container -** like:

- your laptop's web browser (e.g., visiting `http://localhost`)

without `-p`, the app stays inside the container and can't be reached from outside.

### syntax

``` docker
docker run -p <host_port>:<container_port> <image>
docker run -p 8080:80 nginx
```
This maps port 80 inside the container to port 8080 on your local machine. You can then access the Nginx web server by visiting http://localhost:8080.
## Dockerfile breakdown

A dockerfile has five key instructions:

1. `FROM` - Defines the base image to use for the docker image.
2. `WORKDIR` - Sets the working directory inside the container for commands.
3. `RUN` - Executes commands (e.g., install packages, dependencies, etc.)
4. `COPY` - Copies files from your host machine into the container.
5. `CMD` - Specifies the command to run when the container starts.

## Docker Networking

By default, containers are **isolated**. To enable communication:

### Type Docker Networks:

| **Network Type**    | **Description**                                                                |
|---------------------|--------------------------------------------------------------------------------|
| `bridge`            | Default network where containers talk on the same network      | 
| `host`              | container shares the host's network (no isolation)     | 
| `none`              | No network access (fully isolated)                                          | 


### Example Use Case

If you build a web app and want to connect it to a database container (e.g., Flask + MySQL), you must:
- Create a **custom Docker network**
- Run both containers with `--network my-custom`
- Ensure each container is referenced in your code (e.g., `host='mydb'`)


## Docker Compose

As the project grow, managing multiple containers manually become complex.

**Docker Compose** is a tool that help you define and run **multi-container applications** using a YAML file.

### Benefits of Docker Compose:
- **Manages all containers with one command** - with a single `docker compose up` or `docker compose down` command, you can start or stop all defined services, eliminating the need to manage each container manually.
- **Improves development speed and onboarding** - New developers can spin up the entire application stack in second without complex setup instructions, reducing ramp-up time.
- **keeps environment consistent** - Ensure the same container versions, configuration, and dependencies are used across development, testing and production environments. 

### Basic Compose Workflow:
1. Create `docker-compose.yml` listing all services
2. Use `docker compose up` to start
3. Use `docker compose down` to stop

### Team collaboration::
- Saves setup time for new team members
- Ensure environment consistency across machines

### Docker Compose Template
```yaml
version: '3.8' # Defines the version of Docker Compose syntax

services:
  web: # Defines the 'web' service
    build: . # Build from Dockerfile in current directory
    ports:
      - "5000:5000" # Map host port 5000 to container port 5000
    depends_on:
      - db # Ensure 'db' service starts before 'web'

  db:
    image: mysql:5.7 # Use official MySQL 5.7 image
    environment:
      MYSQL_ROOT_PASSWORD: example # Set root password
```

## Docker Registry
A **Docker Registry** is a library/repository where you store and retrieve Docker images.

### Types:
- **Public Registry** - e.g., Docker Hub
- **Private Registry** - e.g., AWS ECR, Azure Container Registry (Secured and private)

### Why it matters:
- Central place for teams to access images
- Simplifies deployment pipelines
- Prevent "it works on my machine" issues by using versioned images

### Docker Registry Commands:
``` yaml
# Log in to Docker Hub or custom registry
docker login

# Tag your image for the registry
# Format: docker tag <local-image> <registry>/<username>/<repo>:<tag>
docker tag myapp myusername/myapp:latest

# Push image to Docker Hub
# Format: docker push <registry>/<username>/<repo>:<tag>
docker push myusername/myapp:latest

# Pull image from Docker Hub
# Format: docker pull <registry>/<username>/<repo>:<tag>
docker pull myusername/myapp:latest 
```
This repository is part of my long-term goal to master containerization and DevOps workflows using Docker.