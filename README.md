# Cardano minting guide. (Version 1.31.0)

# Step 1: Set up Linux environment
#### Hardware requirements:
- Min 2 vCPU
- Min 12gb of RAM (recommend 16gb)
- 50gb+ of disk space
#### VPS providers:
- [Contabo](https://contabo.com/en/) (What I prefer to use)
- [AWS](https://aws.amazon.com/)
- [Digital Ocean](https://www.digitalocean.com/)

For this guide, I will be using Ubuntu 20.04

#### Create "non-root" sudo user:
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

# Step 2: Setup Cardano-Node using CNTools.

For this step, its best to go through the Guild Operators: [CNTools](https://cardano-community.github.io/guild-operators/) docs on your own to gain a full understanding. But I will quick list out the commands you need to run. 

Make sure you are in your "home" directory:
```
cd
```

Download the pre-requisites scripts:
```
mkdir "$HOME/tmp";cd "$HOME/tmp"
curl -sS -o prereqs.sh https://raw.githubusercontent.com/cardano-community/guild-operators/master/scripts/cnode-helper-scripts/prereqs.sh
chmod 755 prereqs.sh
```

Run prereqs.sh:
```
./prereqs.sh
```
Then:
```
. "${HOME}/.bashrc"
```
This will generate the CNTools file structure for Cardano-Node.

Next we need to clone the cardano-node github repo:
```
cd ~/git
git clone https://github.com/input-output-hk/cardano-node
cd cardano-node
```
Then fetch the lastest version (1.31.0) and build:
```
git fetch --tags --all
git pull
git checkout $(curl -s https://api.github.com/repos/input-output-hk/cardano-node/releases/latest | jq -r .tag_name)
$CNODE_HOME/scripts/cabal-build-all.sh
```
Building the node will take anywhere from 30 minutes to 2 hours depending on your VPS.

After build is finished, verify that the cardano-cli and node is working and running on 1.31.0:
```
cardano-cli version
cardano-node version
```

# Step 3: Organizing Cardano-node files.
Now that we have our cardano-node setup and it is fully synced. We can start generating the required payment, stake, and policy files. You do not have to organize the files the same way I do. I just prefer things to be tiddy.

CD into your CNODE_HOME directory:
```
cd $CNODE_HOME
```
Create a new directory called "workdir". This is where we will put all of our keys, policy files, and metadata.
```
mkdir workdir
cd workdir
```
Since workdir is the level where we will be running Cardano CLI commands, we need to tell the node where the socket is. CNTools places the socket in:
```
/opt/cardano/cnode/sockets
#or
$CNODE_HOME/sockets
```
Now, we don't want to have constantly keep export the socket everytime we want to run CLI commands...so there is an easy fix for this.

### Installing and Setting up Tmux

To save on stress and time, I highly recommend using [Tmux](https://linuxize.com/post/getting-started-with-tmux/). 

"tmux is an open-source terminal multiplexer for Unix-like operating systems. It allows multiple terminal sessions to be accessed simultaneously in a single window. It is useful for running more than one command-line program at the same time" https://en.wikipedia.org/wiki/Tmux

1. Install Tmux on VPS:
```
sudo apt install tmux
```
2. Create a named Tmux session:
```
tmux new -s minting
```

Now that we have a Tmux session, we need to export the cardano node "socket" so we are able to run cardano CLI commands and also never have to export the socket again since the tmux "minting" session will always be live. 

To exit out of a tmux session and go back to the normal session, simply press:

"Ctrl+b d"

You can list out your current tmux sessions using the command:
```
tmux ls
```

Then to open an existing tmux session, like the one we just created...we run:
```
tmux a -t minting
```

and now, we are back in our minting shell.

### Exporting Node Socket

As I stated above, we need to tell Cardano-node where the socket is so we can run the CLI commands.

Make sure you are in the workdir we created earlier.
```
cd /opt/cardano/cnode/workdir
# or
cd cd $CNODE_HOME/workdir
```
Then we run:
```
export CARDANO_NODE_SOCKET_PATH=/opt/cardano/cnode/sockets/node0.socket
```
