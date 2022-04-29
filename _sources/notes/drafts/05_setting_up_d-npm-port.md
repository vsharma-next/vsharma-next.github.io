# Installing docker, docker-compose, nginx-proxy-manager and portainer

Source: [awesome open source](https://www.youtube.com/watch?v=TdEKVPWbC58&ab_channel=AwesomeOpenSource)

Status: Reset - not sure will follow this method.

## Steps taken

1. ### update system

```
sudo apt update -y > ~/logs/docker-script-install.log
```

2. ### open http ports

```
sudo ufw allow http
```

_note_: https was already opened earlier

3. ### setting up nginx-proxy-manager (npm)

#### download the docker-compose.yml

```
mkdir -p docker/nginx-proxy-manager

cd docker/nginx-proxy-manager

curl https://gitlab.com/bmcgonag/docker_installs/-/raw/main/docker_compose.nginx_proxy_manager.yml -o docker-compose.yml
```

_note_: save docker-compose.yml somewhere

#### spin up nginx-proxy-manager

```
docker-compose up -d
```

_the -d launches the docker container in the background, thereby freeing up the terminal_

4. ### setup portainer

- create docker volume for portainer

```
sudo docker volume create portainer_data
```

- install portainer-ce

```
sudo docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```

WHOOPS made a mistake - installed Portainer-agent instead of Portainer-CE

[got help from] (https://linuxize.com/post/how-to-remove-docker-images-containers-volumes-and-networks/)

```
# got the container ID from docker ps and stopped it first
docker stop 3988ec92833d
```

```
# listing all docker containers
docker container ls -a
```

```
# removing containers not working currently
docker container prune
```

```
# removing networks / volumes / images and containers
docker system prune -a
```

_note: this also removed some older stuff that I had installed but forgotten about_

5. ### setting up npm and portainer on web gui directly

Make sure that the relevant ports are open through the firewall.

- npm: port 81
- portainer: port 9000

ofcourse - make sure that at this point, port 80 (http) and port 443 (https) are also open.
