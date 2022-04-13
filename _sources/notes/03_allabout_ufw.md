# Ubuntu's firewall (UFW)

## What is UFW

## Links

Details from

1. [digitalocean](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-18-04)
2. [random google result](https://fedingo.com/how-to-check-open-ports-in-ufw/)

## Why did I use it

1. when a new vps was created, all ports were closed EXCEPT OpenSSH
2. later, when testing some docker stuff - for example during installing bookstackapp - all ports were closed and so, I had to open https as

```
sudo ufw allow https
```

OR

```
sudo ufw allow 443
```

3. To check which ports are open

```
sudo ufw status
```

4. allow firewall (ufw) to keep ssh ports open

```
ufw allow OpenSSH
```

5. main-step: enable firewall

```
ufw enable
```

6. Something goes wrong

```
sudo ufw reset
```
