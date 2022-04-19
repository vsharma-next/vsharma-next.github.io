# How to use Docker

Source: https://docker-curriculum.com/

## Introduction

- Docker is a tool that allows developers, sys-admins etc. to easily deploy their applications in a **sandbox (called containers)** to run on the host operating system i.e. Linux.
- The key benefit of Docker is that it allows users to package an application with all of its dependencies into a standardized unit for software development.

## Installation

### Docker

Source: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04

### Docker Compose

Source: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04

### Exactly my history

```
## DELETING ALL EXISTING DOCKER STUFF
sudo apt-get purge docker-ce docker-ce-cli docker-ce-rootless-extras docker-scan-plugin docker-engine
sudo apt-get autoremove docker-ce docker-ce-cli docker-ce-rootless-extras docker-scan-plugin docker-engine
### removing old config files
sudo rm -rf /var/lib/docker /etc/docker
sudo rm /etc/apparmor.d/docker
sudo groupdel docker
sudo rm -rf /var/run/docker.sock
sudo rm -rf /usr/local/bin/docker-compose
sudo rm -rf /etc/docker
sudo rm -rf ~/.docker
## install from docker
sudo apt install docker-ce
### important: docker must be added to get sudo permissions.
sudo usermod -aG docker ${USER}
su - ${USER}
## Installing docker compose ( check the v2.4.1 from github)
mkdir -p ~/.docker/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.4.1/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
```

##

## some tricks and tips

1. https://linuxhint.com/stop_all_docker_containers/
2. https://linuxize.com/post/how-to-remove-docker-images-containers-volumes-and-networks/

### to reset system

- stop all containers from running

```
docker container stop $(docker container list -qa)
```

- remove everything from the system

```
docker system prune -a
```

FJuL2fBSNUmNfuiSCVOGvteXTk1P8Pt0
