# Docker

Docker is an open-source platform that allows you to automate the deployment, scaling, and management of applications using containerization. Containerization is a lightweight alternative to full machine virtualization, where applications are packaged along with their dependencies and run within isolated environments called containers.

Docker provides a way to package an application and its dependencies into a standardized unit called a Docker image. A Docker image contains everything needed to run the application, including the code, runtime, libraries, and system tools. These images are built based on Dockerfiles, which are text files that specify the instructions to create the image.

Benefits of using Docker include:

1. **Isolation**: Containers provide isolation between applications and their dependencies, allowing you to run multiple applications on the same host without conflicts.
2. **Portability**: Docker containers can run on any system that supports Docker, providing consistency and eliminating compatibility issues.
3. **Scalability**: Docker simplifies the process of scaling applications by allowing you to easily create multiple instances of containers and distribute the load.
4. **Version control**: Docker images can be versioned, making it easy to roll back to a previous version or deploy multiple versions simultaneously.
5. **Efficiency**: Containers are lightweight and share the host system's kernel, which reduces resource overhead and improves performance.

## Key Docker terms

### Docker Container

A container is a running instance of a Docker image. It can be thought of as a lightweight, isolated environment that encapsulates an application and its dependencies. Containers are isolated from one another and from the host system, but they share the same underlying kernel. Each container has its own filesystem, processes, network interfaces, and resource allocations.

### Docker Image

An image is a lightweight, standalone, and executable software package that includes everything needed to run an application, including the code, runtime, libraries, and system tools. It is built from a set of instructions defined in a Dockerfile. Docker images are portable and can be easily shared and deployed across different environments. They form the basis for creating Docker containers.

### Docker Volume

A volume is a Docker feature that allows you to persist and share data between containers and between containers and the host system. Volumes provide a way to store and manage data independently of the container's lifecycle. They can be used for storing configuration files, databases, log files, or any other data that needs to be preserved beyond the lifespan of a container.

### Docker Network

A Docker network is a virtual network that allows containers to communicate with each other and with other networks, such as the host system or external networks. Docker provides various types of networks, including bridge networks, overlay networks, and host networks. Bridge networks, which are the default, create an isolated network for containers on the same host, while overlay networks enable communication between containers across multiple hosts in a swarm.

### Docker Registry

A Docker registry is a centralized repository for storing and distributing Docker images. It can be public, such as Docker Hub, or private, hosted within an organization's infrastructure. Docker images can be pushed to and pulled from a registry, allowing for easy sharing and distribution of containerized applications.

### Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications. It allows you to define your application's services, networks, and volumes in a YAML file called a `docker-compose.yml`. With Docker Compose, you can manage complex applications that consist of multiple interconnected containers. It simplifies the process of setting up and configuring the environment, making it easier to replicate and share the application stack.

## Docker Lifecycle Commands

1. **docker run**: This command is used to create and start a new container based on a specified Docker image. It is one of the most fundamental commands in Docker.
2. **docker stop**: This command is used to stop a running container. It sends a SIGTERM signal to the main process running in the container, allowing it to shut down gracefully.
3. **docker start**: Used to start a stopped container.
4. **docker restart**: This command restarts a container. It first stops the container and then starts it again.
5. **docker pause**: Pauses a running container, suspending all processes within it.
6. **docker unpause**: Resumes a paused container, allowing its processes to continue running.
7. **docker kill**: This command sends a SIGKILL signal to forcibly terminate a running container.
8. **docker rm**: Deletes one or more stopped containers. If you want to delete a running container, you can add the `-f` or `--force` flag.
9. **docker rmi**: Removes one or more Docker images from your local machine.
10. **docker ps**: Lists all running containers. Adding the `-a` flag will also show stopped containers.
11. **docker logs**: Displays the logs generated by a running container. You can specify the container name or ID.
12. **docker exec**: Allows you to execute a command within a running container. It's useful for debugging or performing administrative tasks inside a container.
13. **docker inspect**: Provides detailed information about a Docker container or image, including its configuration, network settings, and more.
14. **docker pull**: Fetches a Docker image from a registry (such as Docker Hub) to your local machine.
15. **docker push**: Pushes a Docker image from your local machine to a registry.

## Most common flags while running docker image

1. **-d, --detach**: Run the container in the background and print the container ID.
2. **-p, --publish**: Publish a container's port(s) to the host. Format: `hostPort:containerPort`.
3. **--name**: Assign a name to the container.
4. **-e, --env**: Set environment variables.
5. **-v, --volume**: Bind mount a volume. Format: `hostPath:containerPath`.
6. **--rm**: Automatically remove the container when it exits.
7. **-i, --interactive**: Keep STDIN open even if not attached.
8. **-t, --tty**: Allocate a pseudo-TTY.
9. **--restart**: Restart policy to apply when a container exits. Options: `no`, `on-failure`, `always`, `unless-stopped`.
10. **--network**: Connect the container to a network. Options: `bridge`, `host`, `none`, or user-defined networks.

## Docker-compose lifecycle commands

Docker Compose is a tool used for defining and running multi-container Docker applications. Here are some of the most common Docker Compose lifecycle commands:

1. **docker-compose up**: This command creates and starts all the services defined in the `docker-compose.yml` file. If services are already running, it recreates them.

2. **docker-compose down**: Stops and removes all containers, networks, and volumes created by `docker-compose up`. It effectively shuts down the entire application stack.

3. **docker-compose build**: Builds or rebuilds the images defined in the `docker-compose.yml` file. It's useful when you make changes to your Dockerfiles or other build-related files.

4. **docker-compose start**: Starts existing containers for services defined in the `docker-compose.yml` file.

5. **docker-compose stop**: Stops running containers for services defined in the `docker-compose.yml` file, without removing them.

6. **docker-compose restart**: Restarts containers for services defined in the `docker-compose.yml` file.

7. **docker-compose pause**: Pauses running containers for services defined in the `docker-compose.yml` file.

8. **docker-compose unpause**: Resumes paused containers for services defined in the `docker-compose.yml` file.

9. **docker-compose exec**: Executes a command inside a running container. Usage: `docker-compose exec <service_name> <command>`.

10. **docker-compose logs**: Displays log output from services. Usage: `docker-compose logs <service_name>`.

11. **docker-compose ps**: Lists containers for services defined in the `docker-compose.yml` file, along with their status.

12. **docker-compose rm**: Removes stopped containers for services defined in the `docker-compose.yml` file.

## Most common flags while running docker-compose 

When running `docker-compose up`, you can use various flags to customize its behavior. Here are some commonly used flags:

1. **-d, --detach**: Run containers in the background and print their names.

2. **--no-color**: Disable colorized output.

3. **--no-deps**: Don't start linked services.

4. **--build**: Build images before starting containers.

5. **--force-recreate**: Recreate containers even if their configuration and image haven't changed.

6. **--always-recreate-deps**: Recreate dependent containers.

7. **--no-recreate**: If containers already exist, don't recreate them. Ignores any changes to services in `docker-compose.yml`.

8. **--no-build**: Don't build an image, even if it's missing.

9. **--no-start**: Don't start the services after creating them.

10. **--abort-on-container-exit**: Stops all containers if any container is stopped.

11. **--exit-code-from SERVICE**: Returns the exit code of the specified service container.

12. **-t TIMEOUT, --timeout TIMEOUT**: Use this to override the default timeout setting for the command's execution.

13. **--remove-orphans**: Remove containers for services not defined in the `docker-compose.yml` file.

14. **--scale SERVICE=NUM**: Scale a service to a specified number of instances.

## Volume Management Commands

1. **docker volume create**: Create a new volume.
2. **docker volume ls**: List all volumes.
3. **docker volume inspect**: Display detailed information about a volume.
4. **docker volume rm**: Remove one or more volumes.

## Network Management Commands

1. **docker network create**: Create a new network.
2. **docker network ls**: List all networks.
3. **docker network inspect**: Display detailed information about a network.
4. **docker network rm**: Remove one or more networks.

## Best practices for building Docker systems

Certainly! Building a robust Docker system involves adhering to best practices to ensure efficiency, security, scalability, and maintainability. Here are some best practices for building Docker systems:

1. **Use Official Base Images**: Utilize official Docker images whenever possible as they are regularly updated, maintained, and have undergone security checks.

2. **Minimize Image Size**: Keep Docker images as small as possible by removing unnecessary dependencies, using multi-stage builds, and minimizing layers.

3. **Leverage Dockerfile Caching**: Arrange Dockerfile instructions to maximize caching benefits, placing frequently changing instructions at the end.

4. **Avoid Running Containers as Root**: Whenever feasible, run containers with non-root users to minimize security risks.

5. **Use .dockerignore**: Utilize .dockerignore files to exclude unnecessary files and directories from being copied into the Docker image, reducing its size and build time.

6. **Implement Health Checks**: Include health checks in Dockerfiles to monitor container health and automate restarts when necessary.

7. **Separate Development and Production Environments**: Maintain separate Dockerfiles and configurations for development and production environments to avoid potential security and performance issues.

8. **Use Docker Compose for Multi-Container Applications**: Employ Docker Compose to manage multi-container applications, defining services, networks, and volumes in a single YAML file for easier orchestration.

9. **Monitor Resource Usage**: Monitor Docker resource usage (CPU, memory, disk space) to optimize performance and prevent resource exhaustion.

10. **Manage Secrets Securely**: Avoid hardcoding sensitive information such as passwords and API keys into Docker images; instead, use Docker secrets or environment variables.

11. **Implement Logging and Monitoring**: Configure logging and monitoring solutions to capture container logs and monitor performance metrics for better visibility and troubleshooting.

12. **Automate Image Builds**: Implement Continuous Integration (CI) pipelines to automate Docker image builds, tests, and deployments for streamlined development workflows.

13. **Regularly Update Images**: Stay updated with security patches and new releases by regularly updating Docker images and dependencies.

14. **Implement Container Orchestration**: Consider using container orchestration platforms like Kubernetes or Docker Swarm for managing containerized applications at scale, ensuring high availability, scalability, and resilience.

15. **Document Dockerfiles and Docker Compose Files**: Provide clear and concise documentation for Dockerfiles and Docker Compose files to facilitate collaboration and future maintenance.

By following these best practices, you can build Docker systems that are secure, efficient, scalable, and easy to maintain, enabling smooth deployment and operation of containerized applications.

## Example 1(Jupyter notebook).

```shell
# Use a base image with Python and Jupyter Notebook pre-installed
FROM jupyter/base-notebook:latest

# Set metadata for an image
LABEL maintainer="Your Name <your.email@example.com>"
LABEL description="Dockerfile for running Jupyter Notebook server"

# Set environment variables
ENV NB_USER=nitesh
ENV NB_UID=1000
ENV NB_GID=100

# Create a directory for notebooks
WORKDIR /home/$NB_USER/work

# Expose port for accessing Jupyter Notebook
EXPOSE 8888

# Copy the requirements.txt file from local to container
COPY requirements.txt /tmp/

# Install packages listed in requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt

# Start the Jupyter Notebook server
CMD ["jupyter", "notebook", "--ip='*'", "--port=8888", "--no-browser", "--allow-root"]
```

```shell
# Build docker image
$ docker build -t my_jupyter_notebook .

# Run docker image
$ docker run -p 8888:8888 -v /path/to/local/directory:/home/nitesh/work my_jupyter_notebook
```

## Example 2. (Django application)

```shell
# Use a base image with Python pre-installed
FROM python:3.9

# Set metadata for an image
LABEL maintainer="Your Name <your.email@example.com>"
LABEL description="Dockerfile for running Django application"

# Set environment variables
ENV PYTHONUNBUFFERED 1
ENV DJANGO_HOME /app

# Create a directory for the Django application
RUN mkdir $DJANGO_HOME

# Set the working directory
WORKDIR $DJANGO_HOME

# Copy the requirements.txt file from local to container
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the Django application code to the container
COPY . .

# Expose port for accessing Django application
EXPOSE 8000

# Run Django development server
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

```shell
# Build docker image
$ docker build -t my_django_app .

# Run docker image
$ docker run -p 8000:8000 my_django_app
```

## Example 3. (MySQL)

```shell
# Use a base image with MySQL pre-installed
FROM mysql:latest

# Set metadata for an image
LABEL maintainer="Your Name <your.email@example.com>"
LABEL description="Dockerfile for running MySQL database server"

# Set environment variables for MySQL root password and database
ENV MYSQL_ROOT_PASSWORD=root_password
ENV MYSQL_DATABASE=my_database

# Expose port for accessing MySQL server
EXPOSE 3306

# Create a volume for MySQL data
VOLUME /var/lib/mysql
```

```shell
# Build docker image
$ docker build -t my_mysql_server .

# Run docker image
$ docker run -p 3306:3306 --name my_mysql_container -e MYSQL_ROOT_PASSWORD=root_password -e MYSQL_DATABASE=my_database -v mysql_data:/var/lib/mysql -d my_mysql_server

```



## Example 4. (Docker Compose)

```yaml
version: '3'

services:
  # MySQL service
  application-mysql:
    image: mysql:latest
    container_name: application_mysql
    restart: always
    environment:
      MYSQL_DATABASE: ${DOCKER_MYSQL_DATABASE}
      MYSQL_ROOT_PASSWORD: ${DOCKER_MYSQL_ROOT_PASSWORD}
      MYSQL_USER: ${DOCKER_MYSQL_USER}
      MYSQL_PASSWORD: ${DOCKER_MYSQL_PASSWORD}
    ports:
      - '3306:3306'
    volumes:
      - mysql_data:/var/lib/mysql

  # Redis service
  application-redis:
    image: redis:latest
    container_name: application_redis
    restart: always
    volumes:
      - redis_data:/data
    ports:
      - '6379:6379'

  # Django service
  application-web:
    image: django:3.8
    container_name: application_web
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - ../.:/app
    ports:
      - '8000:8000'
    depends_on:
      - application-mysql
      - application-redis

volumes:
  mysql_data:
  redis_data:
```

## Example 5. (Docker Compose)

```yaml
version: '3.8'

services:
  django_app:
    build:
      context: ./django_app
    ports:
      - "${DJANGO_PORT:-8000}:8000"  # Map Django port from .env file or default to 8000
    depends_on:
      - redis
      - mysql

  redis:
    image: redis:latest
    ports:
      - "6379:6379"  # Expose Redis port

  mysql:
    image: mysql:latest
    ports:
      - "3306:3306"  # Expose MySQL port
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:-root_password}  # Set MySQL root password from .env file or default to root_password
      MYSQL_DATABASE: ${MYSQL_DATABASE:-my_database}  # Set MySQL database name from .env file or default to my_database

  grafana:
    image: grafana/grafana:latest
    ports:
      - "${GRAFANA_PORT:-3000}:3000"  # Map Grafana port from .env file or default to 3000
    depends_on:
      - prometheus
    environment:
      GF_SECURITY_ADMIN_PASSWORD: ${GRAFANA_ADMIN_PASSWORD:-admin_password}  # Set Grafana admin password from .env file or default to admin_password

  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"  # Expose Prometheus port
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml:ro  # Mount Prometheus configuration file
    depends_on:
      - django_app
```


**Explanation:**

- **django_app**: This service builds the Django application using a Dockerfile located in the `./django_app` directory. It exposes port 8000 and depends on the Redis and MySQL services.
- **redis**: This service uses the official Redis image and exposes port 6379 for communication with the Django application.
- **mysql**: This service uses the official MySQL image, sets the root password and database name, and exposes port 3306.
- **grafana**: This service uses the official Grafana image, exposes port 3000, and depends on the Prometheus service. It sets the admin password for Grafana.
- **prometheus**: This service uses the official Prometheus image, exposes port 9090, and mounts a configuration file from the `./prometheus` directory. It depends on the Django application service.

## Docker Advance Topics

Advanced topics in Docker go beyond the basics of containerization and explore more complex and specialized use cases. Here are some advanced topics in Docker:

1. **Container Orchestration**: Learn how to manage and scale containerized applications across multiple hosts using container orchestration tools like Kubernetes, Docker Swarm, or Amazon ECS.

2. **Microservices Architecture**: Explore techniques for designing, building, and deploying microservices-based applications using Docker containers to achieve scalability, maintainability, and flexibility.

3. **Container Networking**: Understand advanced networking concepts in Docker, such as overlay networks, network plugins, service discovery, and load balancing, to optimize communication between containers and services.

4. **Container Security**: Dive deeper into container security best practices, including image scanning, vulnerability management, runtime protection, network segmentation, and access control to protect containerized applications and data.

5. **Docker Networking and Storage Drivers**: Learn about different networking and storage drivers available in Docker, such as bridge, host, overlay networks, and storage plugins, and how to choose the right driver for your use case.

6. **Docker Images and Registries**: Explore advanced techniques for optimizing Docker images, using multi-stage builds, layer caching, image squashing, and pushing/pulling images to/from private or public registries like Docker Hub, AWS ECR, or Google Container Registry.

7. **Docker Volumes and Bind Mounts**: Master advanced volume management techniques in Docker, including using volume plugins, remote mounts, and shared volumes across Swarm or Kubernetes clusters.

8. **Docker Networking Plugins**: Extend Docker's networking capabilities by integrating with third-party networking plugins and solutions, such as Calico, Weave, Flannel, or Cilium, to enable advanced features like network policies, encryption, and observability.

9. **Docker Compose and Stacks**: Harness the power of Docker Compose to define and manage multi-container applications with complex dependencies, environments, and configurations using Docker Compose files and Docker Stacks.

10. **Performance Optimization**: Optimize Docker container performance by fine-tuning resource allocation, container isolation, kernel parameters, and container startup/shutdown times to achieve optimal performance and efficiency.

11. **Continuous Integration and Deployment (CI/CD)**: Implement CI/CD pipelines for building, testing, and deploying Dockerized applications using tools like Jenkins, GitLab CI/CD, CircleCI, or Travis CI to automate the software delivery process and ensure rapid and reliable releases.

12. **Monitoring and Logging**: Set up monitoring and logging solutions for Docker environments to gain insights into container health, resource usage, application performance, and security events using tools like Prometheus, Grafana, ELK stack (Elasticsearch, Logstash, Kibana), or Splunk.

13. **Immutable Infrastructure**: Embrace the concept of immutable infrastructure by treating containers as disposable, immutable units that are replaced rather than updated, to ensure consistency, predictability, and reliability in distributed systems.

These advanced topics in Docker provide a deeper understanding of containerization technology and enable developers and DevOps engineers to build, deploy, and manage containerized applications effectively in production environments.
