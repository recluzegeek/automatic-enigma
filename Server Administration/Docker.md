# Docker Installation and Setup Guide

## Installing Docker and Docker Compose

- On debian based,

  ```bash
  sudo apt update && sudo upgrade -y
  sudo apt install docker docker-compose  
  ```

- On arch based,

  ```bash
  sudo pacman -Syyu
  sudo pacman -S docker docker-compose
  ```

## Post Installation Steps

1. **Create Docker Group**:
   - Run `sudo groupadd docker` to create the Docker group.

2. **Add User to Docker Group**:
   - Run `sudo usermod -aG docker $USER` to add your user to the Docker group.
   - Log out and log back in for the changes to take effect.
   - **If you're testing in virtual machine, reboot the machine for the changes to take effect.**

3. **Activate Group Changes**:
   - If needed, run `newgrp docker` to activate the changes to groups.

4. **Verify Installation**:
   - Run `docker run hello-world` to verify that you can run Docker commands without sudo.

## Docker Containers & Images

- **Containers and Images**:
  - Containers are instances of images.
  - Images are like operating systems, while containers are like running instances of those operating systems.

- **Port Mapping**:
  - Use the `-p` flag to specify port mappings when running containers.

- **Environment Variables**:
  - Use the `-e` flag to pass key-value pairs as environment variables when running containers.

  ```docker
  docker run -it -p 8080:8080 -p 6379:6379 -e name='Test Application' test/application
  ```

- You can also map multiple ports and also environment variables

## Docker Compose

- **Basic Commands**:
  - Use `docker compose up` to start services defined in `docker-compose.yml`.
  - Use `docker compose down` to stop and remove containers, networks, and volumes.
  - Use `docker compose -d` for detached mode, running services in the background.

## Common Docker Commands

- **Testing Docker Installation**:
  - Run `docker run hello-world` to test the Docker installation.

- **Running Containers**:
  - Run `docker run -it ubuntu` to start an interactive Ubuntu container.

- **Listing Images and Containers**:
  - Use `docker image ls` or `docker images` to list locally available Docker images.
  - Use `docker ps` or `docker container ls` to list running Docker containers.

- **Logging and Interacting with Containers**:
  - Run `docker log container` to view logs of a specific container.
  - Use `docker exec container ls` to execute commands within a container.

- **Starting and Stopping Containers**:
  - Run `docker start container` to start a stopped container.
  - Run `docker stop container` to stop a running container.
  - Use `docker exec -it ubuntu` to attach the host terminal to a Docker container permanently.

- **Removing Containers and Images**:
  - Run `docker rm container` to remove a container.
  - Run `docker image rm container -f` to remove a Docker image.
    - `-f` flag is used to apply the removal operation forcefully.

- **Managing Volumes and Services**:
  - Run `docker volume create portainer_data` to create a volume.
  - Use `docker run` commands to start services, like Portainer, with specified volumes and ports.

## Cleaning Up Docker Environment

```bash
sudo pacman -R docker-compose docker
sudo rm -rf /var/lib/docker /etc/docker
sudo rm /etc/apparmor.d/docker
sudo groupdel docker
sudo rm -rf /var/run/docker.sock
reboot

sudo rm -rf /var/lib/docker /etc/docker

reboot
```

My tiny container world, wiped clean :expressionless: Time to start from scratch…
