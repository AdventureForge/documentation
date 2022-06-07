---
sidebar_position: 8
---

# Procedure to deploy and configure a basic Ubuntu VM with Docker and Github cli set-up

## Create a droplet

On DigitalOcean, create a new droplet with basic settings

- minimal ram/cpu/ssd for small apps
- use root user with strong password (this will be deactivated later)
- Set an understable name to your VM
- click on "create"

## Connect as root user

Connect to the VM as root with ssh

```bash
$ ssh root@<ip_adress>
```

Apply system updates

```bash
$ apt-get update
$ apt-get dist-upgrade -y
```

Reboot the system

```
$ reboot
```

## Create a new user

Create a new user

```bash
$ adduser <username>
```

Add user to sudo group

```bash
$ usermod -aG sudo <username>
```

You will be prompt to enter a password, choose a strong password

Test login with the new user

```
$ su - <username>
```

Test sudo command with the new user

```
$ sudo apt-get update
```

## Check connection with your new user

Before doing the following steps, try connecting the server with your new user via ssh

```bash
$ ssh <username>@<ip_address>
```

## Generate SSH Key

### On your client

Create an ssh key pair

```bash
$ cd ~/.ssh
$ ssh-keygen -t rsa -b 4096 -C <your-email-address@domain.tld>
```

Change key name if needed.

Upload the public key on your server and connect pointing to your private key

```bash
$ scp -P <ssh_port> ~/.ssh/<key_name>.pub <username>@<ip_address>:~/.ssh/authorized_keys
$ ssh -i ~/.ssh/<key_name> <username>@<ip_address> -p <ssh_port>
$ ssh-add ./<key_name>
```

### On your server

#### Edit SSH config

```bash
$ sudo nano /etc/ssh/sshd_config
```

change this lines to the value bellow

```
Port <ssh_port>
...
PermitRootLogin no
...
AllowUsers <username>
...
PasswordAuthentication no
```

then restart ssh

```bash
$ sudo systemctl restart sshd
```

check the connection with your user

```bash
$ ssh <username>@<ip_address> -p <ssh_port>
```

logout from root and reconnect with you user

## Install Docker

(Based on oficial Docker documentationb)[]

Clean previously installed packages

```bash
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

### Installation using repository

Install packages

```bash
$ sudo apt-get update
$ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

Add Docker's official GPG key

```bash
 $ sudo mkdir -p /etc/apt/keyrings
 $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

Set-up stable repository

```bash
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Install Docker engine

```bash
 $ sudo apt-get update
 $ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

add Docker to sudo group

```
$ sudo usermod -aG docker $USER
```

## Install Fail2ban

```bash
$ sudo apt-get update && sudo apt-get upgrade
$ sudo apt-get install fail2ban -y
$ cd /etc/fail2ban
$ sudo cp fail2ban.conf fail2ban.local
$ sudo nano fail2ban.local
```

edit local file with the settings bellow

```
[sshd]
enabled = true
port = 717
filter = sshd
logpath = /var/log/auth.log
maxretry = 5
bantime = -1
```

restart fail2ban service

```bash
$ sudo systemctl restart fail2ban
```

check fail2ban status

```bash
$ sudo fail2ban-client status
```

check sshd jail

```bash
$ sudo fail2ban-client status sshd
```

## Install GitHub CLI

Install

```bash
$ curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
$ sudo apt update
$ sudo apt install gh
```

Login

```bash
$ gh auth login
```

Follow the procedure
