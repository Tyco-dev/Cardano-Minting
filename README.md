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

1. Once you set up your VPS and retreive your login information, ssh into your VPS.
```
ssh root@ip-address
```
2. Create a "non-root" user.
```
sudo adduser mint
```
3. Add new user to "sudo" group.
```
sudo adduser mint sudo
```
4. Sign in to new user.
```
sudo su - mint
```

It's highly recommended to secure your VPS. You can enable firewall, add 2FA, add fail2ban and much more. This, however, is out of the scope of this guide.

Now that we have "signed in" to our new user, we can refer to the Guild Operators: [CNTools](https://cardano-community.github.io/guild-operators/) docs. You will need to follow their docs from the "Basics" section all the way to the end of the "Node & CLI" sub-section in "Build and Run". This is a very simple process, just copy and paste.

## Setp 2: How to mint on Cardano.
Now that we have our cardano-node setup and it is fully synced. We can start generating the required payment, stake, and policy files.

To save on stress and time, I highly recommend using Tmux. 

"tmux is an open-source terminal multiplexer for Unix-like operating systems. It allows multiple terminal sessions to be accessed simultaneously in a single window. It is useful for running more than one command-line program at the same time" https://en.wikipedia.org/wiki/Tmux
