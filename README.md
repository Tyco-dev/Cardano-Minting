# Cardano minting guide. (Version 1.31.0)

## Step 1: Set up Linux environment
#### Hardware requirements:
- Min 2 vCPU
- Min 12gb of RAM (recommend 16gb)
- 50gb+ of disk space
#### VPS providers:
- [Contabo](https://contabo.com/en/) (What I prefer to use)
- [AWS](https://aws.amazon.com/)
- [Digital Ocean](https://www.digitalocean.com/)

For this guide, I will be using Ubuntu 20.04

Once you set up your VPS and retreive your login information, ssh into your VPS.
```
ssh root@ip-address
```
Create a "non-root" user.
```
sudo adduser mint
```
Add new user to "sudo" group.
```
sudo adduser mint sudo
```
Sign in to new user.
```
sudo su - mint
```
Now that we have "signed in" to our new user, we need to run some updates on our server,
```
sudo apt-get update -y
sudo apt-get upgrade -y

```
