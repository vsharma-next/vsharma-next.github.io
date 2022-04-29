# Setting up fresh VPS with nginx reverse proxy and 2 demo apps

**Input**:

1. Root access to a VPS with ubuntu 20.04
2. Domain name with dns settings (A records) pointing to the IP address of the VPS

**Steps**:

## PRE-APP INSTALL

### 0. setup bash history settings properly

- login into to root@ip_address
- edit /etc/bash.bashrc to add the following lines

```
# Print the timestamp of each command
HISTTIMEFORMAT='%F %T '

# Save 100000 lines of history in memory
HISTSIZE=100000
# Save 2,000,000 lines of history to disk (will have to grep ~/.bash_history for full listing)
HISTFILESIZE=2000000
# Do not store a duplicate of the last entered command
HISTCONTROL=ignoredups
# Ignore more
HISTIGNORE='ls:ll:ll -htr:clear:history'

# Configure BASH to append (rather than overwrite the history):
shopt -s histappend

# Attempt to save all lines of a multiple-line command in the same entry
shopt -s cmdhist

# save multi-line commands to the history with embedded newlines
shopt -s lithist

# After each command, append to the history file and reread it
export PROMPT_COMMAND="${PROMPT_COMMAND:+$PROMPT_COMMAND$"\n"}history -a; history -c; history -r"
```

Source: Source: https://www.thomaslaurenson.com/blog/2018-07-02/better-bash-history/

### 1. setup new user (username: boss)

login to VPS as root. Then ...

```
# Add new user
adduser boss

# provide admin priviliges to boss
usermod -aG sudo boss
```

Stay on in the same session as root for the next step

### 2. Setup firewall (UFW)

```
# allow ufw to let ssh access
ufw allow OpenSSH

# enable ufw
ufw enable
```

**NOTE**: it is very important to allow OpenSSH while in the first login terminal and BEFORE enabling firewall. Otherwise, if openssh is not enabled, ufw is enabled and then you log-out - you may not be able to login again !

Finally - exit the session and re-login as boss

Source: https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04

### 3. Enable passwordless access to VPS.

On local machine

```
# If ssh keys are not preset (xxx.pub files in ~/.ssh), generate ssh keys first using
ssh-keygen -t ed25519

# Copy ssh key to VPS
ssh-copy-id -i ~/.ssh/id_ed25519.pub boss@ipaddress

# setup a simple command to login to VPS
edit ~/.ssh/config to add

Host myVPS
 HostName ipaddress
 User boss
 IdentityFile ~/.ssh/id_ed25519

With the above addition, to login to VPS, you simply have to type ssh myVPS in the terminal

```

source: https://user.cscs.ch/access/auth/#generating-ssh-keys

### 4. installing nginx for proxy on VPS

```
sudo apt update
sudo apt install nginx

# adjust ufw to allow for nginx
sudo ufw allow 'Nginx Full'

this command sets up both HTTP and HTTPS access to ngnix
```

### 5. setup a static page as test.

```
# creating a folder in home as web
mkdir -p ~/web/main
cd ~/web/main
echo "Hello World @ Home Page" >> index.html
```

```
# stop nginx for the time being
sudo systemctl stop nginx
# go to nginx folder
cd /etc/nginx
# add a file for website homepage
sudo vim ./site-available/solarsherpa.xyz
# in the file solarsherpa.xyz add
server{

       server_name solarsherpa.xyz www.solarsherpa.xyz;

       root /home/boss/web/main/;
       index index.html index.htm;

       location / {
                try_files $uri $uri/ =400;
       }

}
# create a soft link of this file in the folder site-enabled
cd ./sites-enabled # note that you are /etc/nginx
sudo ln -sf ../sites-available/solarsherpa.xyz .
cd ..
# modify file ./ngnix.conf
uncomment line: server_names_hash_bucket_size 64

# check if everything is ok
sudo nginx -t

# start nginx again
sudo systemctl start nginx
```

At this point, if you go to solarsherpa.xyz on your browser, you should see Hello World @ Home Page !

### 0. add SSL certificates using Certbot

```
# install certbot and it's extension for nginx

sudo apt install certbot python3-certbot-nginx
```

```
# obtain SSL certificates for our existing static homepage
sudo certbot --nginx -d solarsherpa.xyz -d www.solarsherpa.xyz

# 1. you will be asked for your email address
# 2. you will be asked to accept terms and conditions (YES)
# 3. you will be asked to share your email address with (No)
# 4. if everything goes properly, you will be asked whether you want to redirect all http traffic to your page to https (yes -> select option 2)

```

That is it ! You are done - the homepage solarsherpa.xyz and www.solarsherpa.xyz are secure !

## Setting up docker and docker compose on VPS

```
# allow APT to use HTTPS for access -
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# Add GPG key for official docker repos
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# add Docker repo to APT sources
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

# check if APT sources are correcty
apt-cache policy docker-ce

# install docker-ce (docker community edition)
sudo apt install docker-ce

# make sure that user (in our case: boss) has admin rights for docker (otherwise, you can only use docker as sudo docker xyz - can get very annoying after a while)
sudo usermod -aG docker ${USER}
su - ${USER}

# add docker-compose
mkdir -p ~/.docker/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.4.1/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose

```

**NOTE**: in the final command above - update the version (v2.4.1) according to latest release on the github page of docker/compose

## Install App #1: Bookstack

```
# first create a file in sites-available from template_dockerapp.xyz
cp template_dockerapp.xyz wiki.solarsherpa.xyz
vim wiki.solarsherpa.xyz
edit the two locations to edit : server_name and the port number
```

choose a port number : we choose 8341.

```
# link wiki.solarsherpa.xyz in sites-available to sites-enables
sudo ln -sf ../sites-available/wiki.solarsherpa.xyz .

# restart nginx
sudo systemctl restart nginx

# enable https on wiki.solarsherpa.xyz
sudo certbot --nginx -d wiki.solarsherpa.xyz
```

```
# go to home
cd ~
# create containers
mkdir containers
cd containers
# make dir for bookstack
mkdir bookstack
cd bookstack
# make local folders for persistent data for volumes within the app
mkdir app_data
mkdir app_db
vim docker-compose.yml

```

- In the docker-compose.yml, add :

```yaml
version: '2'
services:
  bookstack:
    image: lscr.io/linuxserver/bookstack
    container_name: bookstack
    environment:
      - PUID=1000
      - PGID=1000
      - APP_URL=https://wiki.solarsherpa.xyz
      - DB_HOST=bookstack_db
      - DB_USER=bookstack_hero
      - DB_PASS=<db_pass_1> # read note below
      - DB_DATABASE=bookstackapp
    volumes:
      - ./app_data:/config
    ports:
      - 127.0.0.1:8341:80
    restart: unless-stopped
    depends_on:
      - bookstack_db
  bookstack_db:
    image: lscr.io/linuxserver/mariadb
    container_name: bookstack_db
    environment:
      - PUID=1000
      - PGID=1000
      - MYSQL_ROOT_PASSWORD=<db_pass_2> # read note below
      - TZ=Europe/Paris
      - MYSQL_DATABASE=bookstackapp
      - MYSQL_USER=bookstack_hero
      - MYSQL_PASSWORD=<db_pass_1>
    volumes:
      - ./app_db:/config
    restart: unless-stopped
```

**NOTE** : create the long password strings for db_pass_1 and db_pass_2 using :

```
head /dev/urandom | tr -dc A-Za-z0-9 | head -c 32 ; echo ''

```

## Install App #2: Openproject

- As always, we first begin by setting up the subdomain adrress with nginx

```
cd /etc/nginx/sites-available
sudo cp template_dockerapp.xyz pm.solarsherpa.xyz
vim pm.solarsherpa.xyz
```

- in the file pm.solarsherpa.xyz, add

```
server{

       server_name pm.solarsherpa.xyz;

       location / {
          proxy_pass http://127.0.0.1:8342;
          proxy_set_header   X-Forwarded-For $remote_addr;
          proxy_set_header   Host $http_host;
       }
}
```

**NOTE**: note the port number . Also, the proxy_set_header commands come from openproject's documentation.

- Add link to the pm.solarsherpa.xyz file to sites-enabled folder as before.
- get ssl certificate:

```
sudo certbot --nginx -d pm.solarsherpa.xyz
```

**NGINX is setup !**

- make folder for openproject in the containers folder
  cd ~/containers
  mkdir openproject
  cd openproject
  mkdir assets
  mkdir pgdata

- generate secret key

```
head /dev/urandom | tr -dc A-Za-z0-9 | head -c 32 ; echo ''
```

- run command:

```
docker run -d -p 127.0.0.1:8342:80 --name openproject -e SECRET_KEY_BASE=output_of_secret_key_above -e SERVER_HOSTNAME=pm.solarsherpa.xyz -v /home/boss/containers/openproject/pgdata:/var/openproject/pgdata -v /home/boss/containers/openproject/assets:/var/openproject/assets openproject/community:12
```
