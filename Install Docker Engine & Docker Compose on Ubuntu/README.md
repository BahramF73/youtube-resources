# Install Docker Engine & Docker Compose on Ubuntu

This repository contains the commands used in my YouTube tutorial for installing **Docker Engine**, **Docker Compose**, and **Docker Buildx** on **Ubuntu** using Docker's official repository.

The guide also covers how to:

- Install Docker from the official repository
- Configure Docker to run without `sudo`
- Verify the installation
- Run your first Docker container

---

## Check Ubuntu Version

```bash
cat /etc/os-release
```

---

## Check System Architecture

```bash
dpkg --print-architecture
```

---

## Remove Conflicting Packages

```bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)
```

---

## Update Package Index

```bash
sudo apt update
```

---

## Install Required Packages

```bash
sudo apt install -y ca-certificates curl
```

---

## Create the Keyring Directory

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

---

## Download Docker's GPG Key

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

---

## Make the Key Readable

```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

---

## Add Docker Repository

```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF
```

---

## Verify the Repository Configuration

```bash
cat /etc/apt/sources.list.d/docker.sources
```

---

## Update Package Index Again

```bash
sudo apt update
```

---

## Install Docker Engine

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## Verify Docker

```bash
docker --version
```

---

## Verify Docker Compose

```bash
docker compose version
```

---

## Verify Docker Buildx

```bash
docker buildx version
```

---

## Verify Docker Service

```bash
sudo systemctl status docker
```

Start Docker if necessary:

```bash
sudo systemctl start docker
```

Enable Docker at boot:

```bash
sudo systemctl enable docker
```

Or do both at once:

```bash
sudo systemctl enable --now docker
```

Check whether Docker is running:

```bash
systemctl is-active docker
```

Check whether Docker starts automatically at boot:

```bash
systemctl is-enabled docker
```

---

## Test Docker

```bash
sudo docker run hello-world
```

---

## View Images

```bash
sudo docker images
```

---

## View Containers

Running containers:

```bash
sudo docker ps
```

All containers:

```bash
sudo docker ps -a
```

---

## Delete an Image

Remove the image:

```bash
sudo docker image rm hello-world
```

An image cannot normally be removed while it is still being used by an existing container, even if that container has already stopped.

You can either force the image removal:

```bash
sudo docker image rm -f hello-world
```

Or remove the container first, then remove the image:

```bash
sudo docker rm CONTAINER_ID
sudo docker image rm hello-world
```

---

## Run Docker Without sudo

### Verify the docker group

```bash
getent group docker
```

### Add your user

```bash
sudo usermod -aG docker "$USER"
```

Log out and log back in, or run:

```bash
newgrp docker
```

Verify your groups:

```bash
groups
```

Test Docker again:

```bash
docker run --rm hello-world
```

---

## Display Docker Information

```bash
docker info
```

---

## Run an Nginx Container

```bash
docker run -d --name nginx-test -p 8080:80 nginx
```

Open:

```text
http://localhost:8080
```

View running containers:

```bash
docker ps
```

View logs:

```bash
docker logs nginx-test
```

Stop:

```bash
docker stop nginx-test
```

Remove:

```bash
docker rm nginx-test
```

Or force remove:

```bash
docker rm -f nginx-test
```

---

## Notes

- Uses Docker's **official repository**.
- Installs Docker Engine, Docker Compose, and Docker Buildx.
- Docker Compose is available through the modern `docker compose` command.
- Adding a user to the `docker` group allows Docker to be used without `sudo`.
- Members of the `docker` group effectively have root-equivalent privileges.

⭐ If this repository helped you, please consider giving it a Star.

## Video

Watch the [full tutorial](https://youtu.be/) on my [YouTube channel](https://www.youtube.com/@BahRamTech).

## License

This project is licensed under the [MIT License](../LICENSE).