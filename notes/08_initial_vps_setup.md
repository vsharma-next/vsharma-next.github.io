# First steps upon getting a VPS

Situation: you have just bought a virtual private server (VPS).

- You chose ubuntu xx.yy as the OS
- you have been given a dedicated IPv4 ip address (and perhaps even a IPv6 ip address) 109.205.xxx.yy
- you have the root account named 'root'
- and you have the password for it.

## Logging into the VPS

from ==local== computer, you can use good old SSH to log into the VPS.

```
ssh root@109.205.xxx.yy
```

## Create a new user

It is advisable to create a new user and **not use root**

```
adduser johndoe
```

However, it is still good to have _admin_ privileges.

```
usermod -aG sudo johndoe
```

## Setting up a basic firewall

On Ubuntu, a firewall software **UFW** comes pre-installed. UFW has a set of pre-determined applications that the firewall allows.

```
ufw app list
```

Typically, only **OpenSSH** is allowed. OpenSSH is the software that allows for SSH connections.

Unable UFW

```
ufw enable
```

Exit the VPS and re-login using the new user.

## setup password-less access

On ==local== computer, generate ssh keys (modern key types are ed25519)

```
ssh-keygen -t ed25519
```

now transfer the publiclly generated key (id_ed25519.pub) to the VPS.

```
ssh-copy-id -i ~/.ssh/id_ed25519.pub johndoe@109.205.xxx.yy
```

## add the VPS to the ~/.ssh/config

This allows one to just type `ssh vpsname ` in the terminal and directly get into the VPS.

```
 Host vpsname
 HostName 109.205.xxx.yy
 User boss
 IdentityFile ~/.ssh/id_ed25519
```

## Finally - update

Log back into VPS and

```
sudo apt update
```

## some more links

- Basic setup from [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04)
- About UFW and firewalls from [DO-Firewalls](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-18-04)
