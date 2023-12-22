---
date: 2023-06-10
authors: [nadeem]
title: Beginners guide on securing your linux server
description: >
  Essential steps and best practices to ensure your server is secure and up to date
categories:
  - Indie Hacking
  - Ideas
---

Will be looking into the essentials of securing a Linux server as part of my daily write-up of creating a Newsletter Service for Engineers.

I wanted to get set up the Infrastructure first before having a working building pipeline instead of rushing my way through in the end trying to get the server up and running quickly.

<!-- more -->

I'm using Digitial Ocean's droplet running Ubuntu 22.04(LTS) here but any other Linux VPS can also be used from any provider. Just make sure you have all the privileges and you may have to do some tweaks in the commands depending on your system

## Connect to the instance

You will be most likely connecting as the root user for the first time. If you have specified your password when creating a VPS then use it to SSH or add SSH key pair (recommend)

```bash
ssh root@192.168.0.1
```

## System Updates

Let's get all packages updated and avoid any vulnerabilities and pending patches.

```bash
//Ubuntu
apt update && apt upgrade

// CentOS/RHEL Stream and Fedora
dnf upgrade

//Alpine
apk update && apk upgrade

//Arch Linux
pacman -Syu

//CentOS 7
yum update

//OpenSuse
zypper update
```

## Adding a limited user account

The default `root` user has unlimited privileges on the system and we should not be using it for day-to-day operations. Using a limited user account also prevents us from accidentally breaking our system.

```bash
adduser johndoe

// CentOs / Fedora
useradd example_user && passwd example_user
```

Some of the tasks would still require administrative privileges which we will perform with the help of `sudo` granting us elevated privileges temporarily

```bash
adduser john_doe sudo

// CentOS / Fedora
usermod -aG wheel example_user
```

Now we can exit from the instance and re-login from the limited user account

```bash
> exit
> ssh johndoe@192.168.0.1
```

> A nice part of having sudo is that it also logs all the commands performed by users.

## Hardening SSH Access

The essentials of hardening your SSH server are

- Using ssh-key pair for authentication
- Restricted `ssh` config on server
- Restricted firewall

### Using SSH Key Pairs for authentication

SSH key pairs are secure keys cryptographically generated that help authenticate and identify a unique user. They are harder to replicate and you don't have to remember the credentials unlike your passwords

**Creating a key pair**

We need to generate our key pair (public and private). Most Linux systems already come with the handle utility `ssh-keygen` to create a key pair.

We will generate a 4096-bit RSA key pair. You can also generate using a different algorithm for encrypting. While creating a key-pair you will be asked to enter a passphrase, you can also skip this step but it's recommended to use a passphrase. A passphrase ensures the private key can't be decrypted without it and hence nobody else can use your private key if in the worse it gets leaked.

> If you already have a generated a SSH Key pair, this command will overwrite it

```bash
ssh-keygen -b 4096
```

**Upload the public key**

We have to add our public in `~/.ssh/authorized_keys` on our server, otherwise, it won't accept any request from any unknown user.

```bash
ssh-copy-id example_user@192.0.2.1

// Using scp to copy the file
> mkdir -p ~/.ssh && sudo chmod -R 700 ~/.ssh/
> scp ~/.ssh/id_rsa.pub example_user@203.0.113.10:~/.ssh/authorized_keys
```

Finally, you would want to restrict access to your SSH keys if you have multiple users on the server

```bash
sudo chmod -R 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
```

Now when you exit and log back into the server it will automatically log in or ask for your passphrase if you haven't opted to save it in the keychain

### Restricted SSH config

Open the ssh daemon config in an editor on your server

```bash
sudo nano /etc/ssh/sshd_config
```

**Disallow root logins over SSH**

Since we have created a limited user account we don't need root access over SSH anymore.

> Usually `sudo` provides us access to almost all operations. But incase if we ever need to log-in as a `root` most VPS provider have a recovery console on the browser which basically is like physically connecting to the instance. If you don't have a recoverly console option you can also skip this step

```bash
# Authentication:
...
PermitRootLogin no
```

**Disable SSH password authentication**

We don't need password authentication anymore and disabling password authentication ensures nobody can attempt to log in or bruteforce into our SSH

> SSH key pair identiy are limited to every system. If you use multiple devices to log-in it's better to add those SSH key pairs in the authorised keys before performing this step or you can also skip this step

```bash
# Change to no to disable tunnelled clear text passwords
PasswordAuthentication no
```

**Disable additional protocol**

By default, the ssh daemon listens to both `IPv4` and `IPv6`. If you only use one which is usually `IPv4` we can update the config to disable accepting connections from `IPv6` address.

> It only affects the SSH dameon otherwise services will still be accessible over both protocols

```bash
# Port 22
AddressFamily inet
```

**Changing the SSH port**

Additionally, you can also update the ssh daemon to listen to a different port instead of `22` by default. I tend to leave it because the server will be secure enough till the end and usually you can use port scanning utilities to identify open ports anyway

```bash
# Port 22
AddressFamily inet
```

**Restart the SSH service**

```bash
// Systemmd (Ubuntu, Debian, CentOS, Fedora)
sudo systemctl restart sshd

// SystemV
sudo service sshd restart
```

## Configure a Firewall

I prefer to restrict the use of a Firewall provided by the VPS provider which usually has a UI to update the Firewall config on the fly from the dashboard. It allows me to restrict SSH or certain other services ; `Portainer` or `Postgres Database` from my IP address. Since I also own and host VPN to connect to the instance the IPs don't change but in-case if I ever have to add additional IP, it's easier to log in to the dashboard and update the firewall settings.

## Additional things you can do

#### Add a custom hostname

The compute instance usually has a specific purpose or domain that represents it and it would be better when we log in and see that in the terminal instead of localhost

```bash
hostnamectl set-hostname engg-updates
```

So now when you log you will see the following in the terminal

```bash
~ root@engg-updates:
```

#### Add a custom timezone

Most of us would prefer the server running in our timezone which will also help in debugging something easily

> `timedatectl` may not be available in all distributions. So look for alternatives for updating the time-zones

```bash
timedatectl list-timezones | grep 'America'

timedatectl set-timezone 'America/New_York'
```

Lastly, these are just the most basic steps to secure any Linux server. Additional security layers depend on the intended use and you may need to use a monitoring system, access control, fine-tune sudo, restrict exposed services and more
