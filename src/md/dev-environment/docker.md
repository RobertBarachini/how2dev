# Introduction

Docker is one of the most useful tools for development and deployment of apps. It allows you to create isolated (more on that some other time) containers and run them consistently on your machine as well as on other remote environments. It is also a vital part of CI/CD pipelines and infrastructure as code which make your life easier (especially in large projects).

# Setup

## Installation

This guide assumes you have a working version of a Linux distribution installed on your machine. If you don't, check out the [VM Setup](vm-setup.md) guide to get started.

We'll be using the apt repository method to install Docker and its components.

```sh
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Install the latest version of Docker Engine and containerd:

```sh
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

We can now run Docker commands. You can test it by running:

```sh
sudo docker run hello-world
```

## Post-installation

To run Docker commands without `sudo`, add your user to the `docker` group:

```sh
# Create docker group ; NOTE: docker group may already exist
sudo groupadd docker
# Add user to docker group
sudo usermod -aG docker $USER
# Log out and log back in to apply the changes or run the following command
newgrp docker
# Test the hello-world container without sudo
docker run hello-world
```

Now we will make sure docker daemon starts on boot:

```sh
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

### Portainer

To make it easier to manage containers and environments via a web browser, we can use Portainer.

```sh
# Create a volume for Portainer
docker volume create portainer_data
# Run Portainer
docker run -d -p 8887:8000 -p 8888:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest

# NOTE: if you're planning on using edge agents with this server you will need to use -p 8000:8000
```

You can now access Portainer by going to `https://localhost:8888` in your browser. Visit `https://localhost:8888/#!/init/admin` for initial config. Create a user and create a strong password and store it in a password manager. Make sure to bookmark the page for easy access.

# Basic usage

TODO

# Useful commmands

TODO

```

```
