# Docker Complete Command Notes — Ubuntu Linux

> These notes cover the commonly required Docker commands from installation to daily usage, networking, volumes, Dockerfiles, Docker Compose, troubleshooting, and cleanup.

---

# 1. Check Linux System

## Check Linux distribution and version

```bash
cat /etc/os-release
```

## Check Linux kernel version

```bash
uname -r
```

## Check complete system/kernel information

```bash
uname -a
```

## Check system architecture

```bash
uname -m
```

---

# 2. Update Ubuntu Before Installing Docker

## Update the local package index

```bash
sudo apt update
```

## Upgrade installed packages

```bash
sudo apt upgrade -y
```

---

# 3. Install Docker on Ubuntu

## Install Docker using the Ubuntu package repository

```bash
sudo apt install docker.io -y
```

## Check installed Docker version

```bash
docker --version
```

## Start Docker service

```bash
sudo systemctl start docker
```

## Enable Docker to start automatically when the system boots

```bash
sudo systemctl enable docker
```

## Check Docker service status

```bash
sudo systemctl status docker
```

## Restart Docker service

```bash
sudo systemctl restart docker
```

## Stop Docker service

```bash
sudo systemctl stop docker
```

## Check detailed Docker installation and environment information

```bash
sudo docker info
```

---

# 4. Run Docker Without sudo

## Add the current user to the docker group

```bash
sudo usermod -aG docker $USER
```

## Apply the new docker group in the current terminal session

```bash
newgrp docker
```

## Verify that Docker works without sudo

```bash
docker ps
```

> After adding the user to the `docker` group, you can normally use Docker commands without `sudo`.

---

# 5. Verify Docker Installation

## Run Docker's test container

```bash
docker run hello-world
```

## Display currently running containers

```bash
docker ps
```

## Display all containers including stopped containers

```bash
docker ps -a
```

## Display Docker images available locally

```bash
docker images
```

---

# 6. Docker Help Commands

## Display Docker's main help menu

```bash
docker --help
```

## Display help for a particular Docker command

```bash
docker run --help
```

## Display Docker version information

```bash
docker version
```

---

# 7. Working with Docker Images

## Display all locally available Docker images

```bash
docker images
```

## Alternative command to display Docker images

```bash
docker image ls
```

## Pull an image from a container registry such as Docker Hub

```bash
docker pull nginx
```

## Pull a specific version/tag of an image

```bash
docker pull nginx:1.27
```

## Download an Ubuntu image

```bash
docker pull ubuntu
```

## Inspect detailed information about an image

```bash
docker image inspect nginx
```

## Display the layers/history used to build an image

```bash
docker history nginx
```

## Remove an image

```bash
docker rmi nginx
```

## Alternative command to remove an image

```bash
docker image rm nginx
```

## Force-remove an image when appropriate

```bash
docker rmi -f nginx
```

## Remove unused dangling images

```bash
docker image prune
```

## Remove all images that are not being used by containers

```bash
docker image prune -a
```

---

# 8. Search for Images

## Search Docker Hub for an image

```bash
docker search nginx
```

## Search Docker Hub for Ubuntu-related images

```bash
docker search ubuntu
```

---

# 9. Run Containers

## Create and start a container from an image

```bash
docker run nginx
```

## Run a container in detached/background mode

```bash
docker run -d nginx
```

## Run a container and assign a custom name

```bash
docker run -d --name nginx-container nginx
```

## Run a container and publish a container port on the host

```bash
docker run -d --name nginx-container -p 80:80 nginx
```

> Port format:

```text
-p HOST_PORT:CONTAINER_PORT
```

## Map host port 8080 to container port 80

```bash
docker run -d --name nginx-container -p 8080:80 nginx
```

## Run an interactive Ubuntu container

```bash
docker run -it ubuntu bash
```

## Automatically remove a container after it exits

```bash
docker run --rm ubuntu echo "Hello Docker"
```

## Run a container with an environment variable

```bash
docker run -d -e APP_ENV=production nginx
```

## Run a container with multiple environment variables

```bash
docker run -d -e APP_ENV=production -e APP_PORT=8080 myapp
```

## Run a container using environment variables from a file

```bash
docker run --env-file .env myapp
```

## Run a container with a restart policy

```bash
docker run -d --restart unless-stopped --name nginx-container nginx
```

---

# 10. Container Listing and Status

## Display running containers

```bash
docker ps
```

## Display all containers

```bash
docker ps -a
```

## Display only container IDs

```bash
docker ps -q
```

## Display IDs of all containers

```bash
docker ps -aq
```

## Display the latest created container

```bash
docker ps -l
```

## Display container disk usage

```bash
docker ps --size
```

---

# 11. Start, Stop and Restart Containers

## Stop a running container

```bash
docker stop nginx-container
```

## Start a stopped container

```bash
docker start nginx-container
```

## Restart a container

```bash
docker restart nginx-container
```

## Pause processes inside a running container

```bash
docker pause nginx-container
```

## Resume a paused container

```bash
docker unpause nginx-container
```

## Immediately terminate a running container

```bash
docker kill nginx-container
```

---

# 12. Enter and Work Inside a Running Container

## Open a Bash shell inside a running container

```bash
docker exec -it nginx-container bash
```

## Open an sh shell when Bash is unavailable

```bash
docker exec -it nginx-container sh
```

## Execute a command inside a running container

```bash
docker exec nginx-container ls
```

## Check environment variables inside a container

```bash
docker exec nginx-container env
```

## Exit an interactive container shell

```bash
exit
```

---

# 13. Container Logs

## Display container logs

```bash
docker logs nginx-container
```

## Continuously follow container logs

```bash
docker logs -f nginx-container
```

## Display the latest 100 log lines

```bash
docker logs --tail 100 nginx-container
```

## Display logs with timestamps

```bash
docker logs -t nginx-container
```

---

# 14. Inspect Containers

## Display detailed container configuration and runtime information

```bash
docker inspect nginx-container
```

## Display the container's IP address

```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' nginx-container
```

## Display processes running inside a container

```bash
docker top nginx-container
```

## Display changes made to the container filesystem

```bash
docker diff nginx-container
```

## Display port mappings for a container

```bash
docker port nginx-container
```

---

# 15. Container Resource Monitoring

## Display live CPU, memory and network usage for running containers

```bash
docker stats
```

## Display resource usage for a particular container

```bash
docker stats nginx-container
```

## Display one resource-usage snapshot instead of continuously updating

```bash
docker stats --no-stream
```

---

# 16. Copy Files Between Host and Container

## Copy a file from the host into a container

```bash
docker cp file.txt nginx-container:/tmp/file.txt
```

## Copy a file from a container to the host

```bash
docker cp nginx-container:/tmp/file.txt ./file.txt
```

## Copy a directory from the host into a container

```bash
docker cp myfolder nginx-container:/tmp/
```

---

# 17. Rename Containers

## Rename an existing container

```bash
docker rename old-name new-name
```

---

# 18. Remove Containers

## Remove a stopped container

```bash
docker rm nginx-container
```

## Force-remove a running container

```bash
docker rm -f nginx-container
```

## Remove all stopped containers

```bash
docker container prune
```

## Remove all containers

```bash
docker rm -f $(docker ps -aq)
```

---

# 19. Docker Volumes

## Display Docker volumes

```bash
docker volume ls
```

## Create a named Docker volume

```bash
docker volume create app-data
```

## Inspect a Docker volume

```bash
docker volume inspect app-data
```

## Run a container using a named volume

```bash
docker run -d --name nginx-container -v app-data:/usr/share/nginx/html nginx
```

## Mount a host directory into a container using a bind mount

```bash
docker run -d --name nginx-container -v $(pwd):/usr/share/nginx/html nginx
```

## Remove a Docker volume

```bash
docker volume rm app-data
```

## Remove unused Docker volumes

```bash
docker volume prune
```

---

# 20. Docker Networks

## Display Docker networks

```bash
docker network ls
```

## Inspect a Docker network

```bash
docker network inspect bridge
```

## Create a custom bridge network

```bash
docker network create my-network
```

## Run a container on a custom network

```bash
docker run -d --name container1 --network my-network nginx
```

## Run another container on the same custom network

```bash
docker run -d --name container2 --network my-network nginx
```

## Connect an existing container to a network

```bash
docker network connect my-network nginx-container
```

## Disconnect a container from a network

```bash
docker network disconnect my-network nginx-container
```

## Remove a custom Docker network

```bash
docker network rm my-network
```

## Remove unused Docker networks

```bash
docker network prune
```

---

# 21. Test Communication Between Containers

## Create a custom Docker network

```bash
docker network create app-network
```

## Start the first container on the network

```bash
docker run -d --name web --network app-network nginx
```

## Start another container on the same network

```bash
docker run -it --rm --network app-network busybox
```

## Test container-name-based communication from inside the BusyBox container

```bash
ping web
```

---

# 22. Dockerfile — Build Your Own Image

## Create a Dockerfile

```bash
nano Dockerfile
```

## Example Dockerfile for a Python Flask application

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python3", "app.py"]
```

## Build a Docker image from the Dockerfile in the current directory

```bash
docker build -t flask-app .
```

## Build an image with a specific tag/version

```bash
docker build -t flask-app:v1 .
```

## Display the newly built image

```bash
docker images
```

## Run the application container

```bash
docker run -d --name flask-app-container -p 5000:5000 flask-app
```

## Check whether the application container is running

```bash
docker ps
```

## View application logs

```bash
docker logs flask-app-container
```

---

# 23. Docker Image Tags

## Add another tag to an existing image

```bash
docker tag flask-app flask-app:v1
```

## Display image tags

```bash
docker images
```

## Remove a particular image tag

```bash
docker rmi flask-app:v1
```

---

# 24. Save and Load Docker Images

## Export a Docker image to a tar archive

```bash
docker save -o flask-app.tar flask-app
```

## Load a Docker image from a tar archive

```bash
docker load -i flask-app.tar
```

---

# 25. Export and Import Container Filesystems

## Export a container filesystem to a tar archive

```bash
docker export -o container-backup.tar nginx-container
```

## Import a filesystem archive as a Docker image

```bash
docker import container-backup.tar nginx-backup
```

> `docker save/load` is mainly used for Docker images and preserves image metadata/layers.

> `docker export/import` works with a container filesystem and does not preserve the original image history in the same way.

---

# 26. Docker Registry / Docker Hub

## Log in to a Docker registry such as Docker Hub

```bash
docker login
```

## Tag a local image using the repository name required for pushing

```bash
docker tag flask-app username/flask-app:v1
```

## Push an image to the registry

```bash
docker push username/flask-app:v1
```

## Pull the image later

```bash
docker pull username/flask-app:v1
```

## Log out from the registry

```bash
docker logout
```

---

# 27. Docker Compose

## Check Docker Compose version

```bash
docker compose version
```

## Start services defined in compose.yaml or docker-compose.yml

```bash
docker compose up
```

## Start Compose services in detached/background mode

```bash
docker compose up -d
```

## Build images and start the services

```bash
docker compose up -d --build
```

## Display Compose containers/services

```bash
docker compose ps
```

## Display logs from Compose services

```bash
docker compose logs
```

## Follow Compose logs continuously

```bash
docker compose logs -f
```

## Display logs for one Compose service

```bash
docker compose logs service-name
```

## Execute a command inside a running Compose service container

```bash
docker compose exec service-name bash
```

## Use sh when Bash is unavailable

```bash
docker compose exec service-name sh
```

## Stop Compose services without removing the containers

```bash
docker compose stop
```

## Start previously stopped Compose services

```bash
docker compose start
```

## Restart Compose services

```bash
docker compose restart
```

## Stop and remove Compose containers and networks

```bash
docker compose down
```

## Stop Compose services and remove associated named/anonymous volumes declared by the project

```bash
docker compose down -v
```

## Pull images required by the Compose configuration

```bash
docker compose pull
```

## Build or rebuild Compose service images

```bash
docker compose build
```

---

# 28. Example Docker Compose File

## Create a Compose configuration file

```bash
nano compose.yaml
```

## Example Compose configuration with an application and MongoDB

```yaml
services:
  node-app:
    build: .
    ports:
      - "3000:3000"
    environment:
      MONGO_URL: mongodb://mongodb:27017/assignmentdb
    depends_on:
      - mongodb

  mongodb:
    image: mongo
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

## Start the complete application

```bash
docker compose up -d
```

## Verify the running services

```bash
docker compose ps
```

## Check application logs

```bash
docker compose logs node-app
```

## Check MongoDB logs

```bash
docker compose logs mongodb
```

## Verify DNS/service-name resolution from the application container

```bash
docker compose exec node-app getent hosts mongodb
```

## Check an environment variable inside the application container

```bash
docker compose exec node-app printenv MONGO_URL
```

## Stop and remove the Compose application

```bash
docker compose down
```

---

# 29. Docker Disk Usage

## Display Docker disk usage

```bash
docker system df
```

## Display detailed Docker disk usage

```bash
docker system df -v
```

---

# 30. Docker Cleanup Commands

## Remove stopped containers

```bash
docker container prune
```

## Remove unused images

```bash
docker image prune
```

## Remove unused networks

```bash
docker network prune
```

## Remove unused volumes

```bash
docker volume prune
```

## Remove unused containers, networks, build cache and dangling images

```bash
docker system prune
```

## Remove unused resources including all unused images

```bash
docker system prune -a
```

## Remove unused resources including unused volumes

```bash
docker system prune -a --volumes
```

> Be careful with prune commands because they permanently remove unused Docker resources.

---

# 31. Useful Troubleshooting Commands

## Check whether Docker service is running

```bash
sudo systemctl status docker
```

## Restart Docker when the daemon has a problem

```bash
sudo systemctl restart docker
```

## Display recent Docker service logs on a systemd-based Linux system

```bash
sudo journalctl -u docker
```

## Display recent Docker service logs

```bash
sudo journalctl -u docker --since "10 minutes ago"
```

## Display container logs

```bash
docker logs container-name
```

## Inspect container configuration

```bash
docker inspect container-name
```

## Check container processes

```bash
docker top container-name
```

## Check container resource usage

```bash
docker stats container-name
```

## Check container port mappings

```bash
docker port container-name
```

## Check Docker networks

```bash
docker network ls
```

## Check Docker volumes

```bash
docker volume ls
```

## Check Docker disk usage

```bash
docker system df
```

---

# 32. Common Docker Command Flow

## Pull an image

```bash
docker pull nginx
```

## Verify the image

```bash
docker images
```

## Create and start a container

```bash
docker run -d --name nginx-container -p 80:80 nginx
```

## Verify the running container

```bash
docker ps
```

## Check logs

```bash
docker logs nginx-container
```

## Inspect the container

```bash
docker inspect nginx-container
```

## Monitor container resources

```bash
docker stats nginx-container
```

## Enter the container when required

```bash
docker exec -it nginx-container bash
```

## Stop the container

```bash
docker stop nginx-container
```

## Start it again

```bash
docker start nginx-container
```

## Remove the container after stopping it

```bash
docker rm nginx-container
```

## Remove the image when it is no longer required

```bash
docker rmi nginx
```

---

# 33. Complete Application Development Flow

## Create the project directory

```bash
mkdir my-app
```

## Move into the project directory

```bash
cd my-app
```

## Create a Dockerfile

```bash
nano Dockerfile
```

## Build the application image

```bash
docker build -t my-app:v1 .
```

## Verify the image

```bash
docker images
```

## Run the application container

```bash
docker run -d --name my-app-container -p 8080:8080 my-app:v1
```

## Verify the running container

```bash
docker ps
```

## Check application logs

```bash
docker logs my-app-container
```

## Follow application logs continuously

```bash
docker logs -f my-app-container
```

## Enter the running container when troubleshooting is required

```bash
docker exec -it my-app-container sh
```

## Inspect the container

```bash
docker inspect my-app-container
```

## Stop the application

```bash
docker stop my-app-container
```

## Remove the container

```bash
docker rm my-app-container
```

## Remove the application image when no longer required

```bash
docker rmi my-app:v1
```

---

# 34. Important Difference — Docker Commands vs Linux Commands

Most Docker commands remain the same regardless of the Linux distribution.

For example, these commands normally remain the same:

```bash
docker pull nginx
docker images
docker ps
docker ps -a
docker run nginx
docker stop container-name
docker start container-name
docker restart container-name
docker rm container-name
docker rmi image-name
docker logs container-name
docker inspect container-name
docker exec -it container-name bash
docker network ls
docker volume ls
docker build -t my-app .
docker compose up -d
docker compose down
docker system df
docker system prune
```

The main commands that can change between Linux distributions are the commands used to install Docker and manage operating-system packages/services.

---

# 35. If Linux Distribution Changes in the Future

## Ubuntu / Debian

Package manager:

```bash
apt
```

Update packages:

```bash
sudo apt update
```

Install a package:

```bash
sudo apt install package-name
```

---

## Fedora

Package manager:

```bash
dnf
```

Update packages:

```bash
sudo dnf update
```

Install a package:

```bash
sudo dnf install package-name
```

---

## RHEL / Rocky Linux / AlmaLinux

Package manager:

```bash
dnf
```

Install a package:

```bash
sudo dnf install package-name
```

> Older RHEL-family systems may use `yum`.

---

## Amazon Linux

Amazon Linux commonly uses:

```bash
dnf
```

> Older Amazon Linux versions may use `yum`.

---

# 36. What Should Be Replaced When Linux Distribution Changes?

If notes contain:

```bash
sudo apt update
```

`apt` is Ubuntu/Debian-specific, so replace it with the package manager used by the new distribution.

If notes contain:

```bash
sudo apt install package-name
```

replace the installation command according to the new Linux distribution.

Commands beginning with Docker itself normally remain unchanged:

```bash
docker ...
```

For example:

```bash
docker ps
docker images
docker pull nginx
docker run nginx
docker stop container-name
docker rm container-name
docker network ls
docker volume ls
docker compose up -d
```

These are Docker CLI commands, not Ubuntu package-management commands.

---

# 37. Important Port Rule When Docker Runs on a Cloud VM

Publishing a Docker port makes the container service available through a host port.

Example:

```bash
docker run -d -p 5000:5000 flask-app
```

This means:

```text
Host Port 5000 -> Container Port 5000
```

If the Docker host is an AWS EC2 instance, external access also requires the corresponding Security Group inbound rule.

Example flow:

```text
Internet
   |
   | TCP 5000 allowed by Security Group
   v
EC2 Host :5000
   |
   | Docker Port Mapping
   v
Container :5000
```

Docker port publishing and cloud firewall/Security Group rules solve different parts of external connectivity.

---

# 38. Docker Command Quick Reference

Check Docker:

```bash
docker --version
```

Check Docker service:

```bash
sudo systemctl status docker
```

Pull image:

```bash
docker pull image-name
```

List images:

```bash
docker images
```

Run container:

```bash
docker run image-name
```

Run in background:

```bash
docker run -d image-name
```

Run with name:

```bash
docker run -d --name container-name image-name
```

Publish port:

```bash
docker run -d -p host-port:container-port image-name
```

List running containers:

```bash
docker ps
```

List all containers:

```bash
docker ps -a
```

View logs:

```bash
docker logs container-name
```

Enter container:

```bash
docker exec -it container-name bash
```

Stop container:

```bash
docker stop container-name
```

Start container:

```bash
docker start container-name
```

Restart container:

```bash
docker restart container-name
```

Remove container:

```bash
docker rm container-name
```

Remove image:

```bash
docker rmi image-name
```

List networks:

```bash
docker network ls
```

List volumes:

```bash
docker volume ls
```

Build image:

```bash
docker build -t image-name .
```

Start Compose application:

```bash
docker compose up -d
```

Stop and remove Compose application:

```bash
docker compose down
```

Check Docker disk usage:

```bash
docker system df
```

Clean unused Docker resources:

```bash
docker system prune
```