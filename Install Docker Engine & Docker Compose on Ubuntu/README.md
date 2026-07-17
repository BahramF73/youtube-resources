# Install Docker Engine & Docker Compose on Ubuntu

This repository contains the commands used in my YouTube tutorial for installing **Docker Engine**, **Docker Compose**, and **Docker Buildx** on **Ubuntu** using Docker's official repository.

The guide also covers how to:

- Install Docker from the official repository
- Configure Docker to run without `sudo`
- Verify the installation
- Run your first Docker container

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

## Update Package Index Again

```bash
sudo apt update
```

---

## Install Docker Engine

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## Verify Installation

```bash
docker --version
docker compose version
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

## Run an Nginx Container

```bash
docker run -d --name nginx-test -p 8080:80 nginx
```

Open:

```
http://localhost:8080
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
