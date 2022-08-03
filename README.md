# install-debian-m1-vmware

Grab the ISO of Debian, drag/drop into VMWare, once VMWare boots GRUB, pick the older kernel in this case `5.10.0`. You'll notice you'll get errors if you try to run anything as root.

The error you'll get is "This account isn't in under any sudoer list", let's take a look inside the `/etc/sudoers`: 

```bash
vim /etc/sudoers
```

This is how mine currently looks like:

<img width="945" alt="Screen Shot 2022-08-03 at 2 31 43 PM" src="https://user-images.githubusercontent.com/20936398/182716218-619b6894-058c-43a0-a811-dd3786abc860.png">

The `/etc/sudoers` file is what is used to determine if a user has permission to run commands that require elevated privileges. So if you run `vim /etc/sudoers` and still get kicked back an error, you'll need to run `su -l` then `vim /etc/sudoers`.

## Backports

You'll need to setup backports in Debian, take a look in `etc/apt/sources`. This is how my current `sources.list` looks like: 

```bash
deb http://deb.debian.org/debian buster main contrib non-free
deb-src http://deb.debian.org/debian buster main contrib non-free

deb http://deb.debian.org/debian buster-updates main contrib non-free
deb-src http://deb.debian.org/debian buster-updates main contrib non-free

deb http://deb.debian.org/debian buster-backports main contrib non-free
deb-src http://deb.debian.org/debian buster-backports main contrib non-free

deb http://security.debian.org/debian-security/ buster/updates main contrib non-free
deb-src http://security.debian.org/debian-security/ buster/updates main contrib non-free
```

Now you've updated your sources, let's update them: 

```bash
sudo apt update
sudo apt-get update
```

