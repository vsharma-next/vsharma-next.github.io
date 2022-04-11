# How to setup NGINX + docker on VPS

## First step: setting up docker

1. installing docker from apt

```
sudo apt update
sudo apt install docker.io docker-compose
```

2. setting up docker to work with user without needing sudo all the time

```
sudo usermod -aG docker johndoe
```

3. logout and login again. The following command should work

```
docker ps
```

and you should get

```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## Second step: setting up volumes for docker

1. `mkdir containers && cd containers`
2. `mkdir -p bookstack database proxy/data proxy/letsencrypt`
3. get docker-compose.yml from [here](https://gist.github.com/ssddanbrown/7f2a045b612dbfcc029abe6d27c7dc58)
4. open the port for nginx-proxy-manager `sudo ufw allow 22 ` ( note that the number 22 may be different for each case - check the docker-compose.yml for details - look for "Admin Web Port")

## Third step: starting up docker

1. from docker's best practices, create the local network for within the docker container by

```
docker network create web
```

2. pull down the docker images

```
docker-compose pull
```

3. start first run of docker directly on terminal

```
docker-compose up
```
