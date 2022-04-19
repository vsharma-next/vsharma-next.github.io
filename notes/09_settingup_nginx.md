# Setting up ngnix on a fresh VPS

Situation: you have already setup a VPS for passwordless access, enabled the UFW firewall with only SSH ports open.

The first thing to do now is setup a web server.

## installing nginx

```
sudo apt update
sudo apt install nginx
```

## allow firewall to let nginx to access the internet

```
sudo ufw allow 'Nginx HTTP'
```

**THAT IS IT FOR THE BASIC SETUP**

NOTE: make sure that on your DNS provider, there are two A records pointing to your IP - example.com and \*.example.com

```
sudo pkill -f nginx & wait $!
sudo systemctl start nginx
```
