# home-git-server-configuration
Create a home GitHub server for game development and any other purposes


## Requirements
- Git SCM (https://git-scm.com/downloads) or alternatively PowerShell
- Network attached storage (NAS)
- Network File System mount (NFS)
- Brain

## Install Latest Ubuntu server
```
sudo apt update
sudo apt upgrade
```

## Install Git on your server

```
sudo apt-get install git-core
```

## See if git-shell is already setup in /etc/shells
```
cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/bin/rbash
/bin/dash
/usr/bin/tmux
/usr/bin/screen
```

## If you don’t see git-shell listed, find out it’s location
```
which git-shell
/usr/bin/git-shel
```

## Add it to /etc/shells
```
sudo nano /etc/shells  # and add the path to git-shell from last command

press ctrl+o then enter
press ctrl+x to exit
```

## Create non sudoer account for security
```
sudo adduser --disabled-password git
```

## Switch to git user
```
sudo su git
```

## Navigate git directory create ./ssh and change permissions then create authorized_keys for keygens
```
cd /home/git
mkdir /home/git/.ssh/
chmod 700 /home/git/.ssh
touch /home/git/.ssh/authorized_keys
chmod 600 /home/git/.ssh/authorized_keys
```

## Access your PC or Laptop (not the server)
### Use "git bash here" or "powershell" for windows
```
shh-keygen -t rsa

Note:
this will appear...
"Generating public/private rsa key pair."
"Enter file in which to save the key (/c/Users/<youruser>/.ssh/id_rsa):"

Just press enter if you want to put it on default folder.

Enter passphrase (empty for no passphrase): "I would recommend that leave it blank and just press enter so you can push repository without password"

then after that the fingerprint will appear and the randomart image

remember the location if you didnt change it you will see it here:
/c/Users/<youruser>/.ssh/id_rsa
/c/Users/<youruser>/.ssh/id_rsa.pub

dont use id_rsa make sure it has file format .pub
```

### this code will show your keygen in terminal have a go and see it to yourself (dont forget to change <youruser>)
#### it will show ssh-rsa at suffix then random hash then your user and computer name on prefix
```
cat /c/Users/<youruser>/.ssh/id_rsa.pub
```
  
### Since your git (server) user doesnt have password you can put the keygen manually (authorized_keys) after running the code above

### try if you can connect to your server via ssh (this varries if you are outsite to your wifi/lan or network)
#### if you are outside of your network just google "whatsmyip" while accessing your server or localcomputer with your server 
#### connected to that network and it will show your public ip
#### then use ssh command to your laptop or pc
#### if you are in your home network your ip should look like this 192.169.0.xxx or 10.0.1.xxx depends on how you set your networking
```
ssh git@<server-ip-address>
  
type exit to exit ssh
```
  
## Creating repo (since we dont have a fancy GUI we need to do this manually)
```
Note: running this code on server and dont forget to change the project name <project-repo>
cd /home/git
mkdir -p <project-repo>.git (yes it is forder)
cd /home/git/<project-repo>.git

Note: Initiate empty repo
git init --bare

Note: this will appear
Initialized empty Git repository in /home/git/<project-repo>.git

cd ..
chown -R git.git <project-repo.git>/
  
Thats it for the server
```
  
## Creating repo on local machine (PC/Laptop)
1) Create new folder name it with your project name (make sure its empty)
2) Right click and Git bash here and run (make sure you add the dot at the end of the code to clone it inside your directory) it means clone it here.
```
  git clone git@<local-ip-address>:<project-repo>.git .
```
3) create README.md or .gitignore using nano or manually
```
  nano README.md
```
4) Paste all your files
5) run git add to track your changes
```
  git add .
```
6) commit your changes
```
  git commit -m "initiate README.md" -a
  
  #Example output
  [master (root-commit) 3099423] initiate README.md
  2 files changed, 2 insertions(+)
  create mode 100644 README.md

```
7) Adding git remote (When you are at home)
  
```
  git remote add origin_home ssh://git@<local-ip-address>/home/git/<project-repo>.git
```
  
7.1) Addiing git remote (When you are outside)
  you must know your internet provider public IP address and must port forward port 22 on router
```
  git remote add origin_outside ssh://git@<public-ip-address>/home/git/<project-repo>.git
```


## Cloning your project to another machine
### dont forget to add and create keygen to authorized_keys if the machine is new
```
  git clone git@<local-or-public-ip-address>:<project-repo>.git .
```
  
  
  
  
  
  
  
--------------DEPRICATED METHOD----------------------
## Submit your keygen to your Server (only use this method if your user has password)
### This code will show your id_rsa.pub and copy it silently then connect to ssh using git user then create authorized_keys file then paste your id_rsa.pub data
#### dont forget to changes values <youruser> <sudoer-user> <server-ip-address> also it will ask for password
```
cat /c/Users/<youruser>/.ssh/id_rsa.pub | ssh <sudoer-user>@<server-ip-address> "mkdir -p /home/git/.ssh && cat >> /home/git/.ssh/authorized_keys"
  
Note: you should be able to see it in your server by running
sudo nano /home/git/.ssh/authorized_keys

just press ctrl+x to exit
```
--------------END DEPRICATED METHOD----------------------
  
  
  
  
  
  
  
  
