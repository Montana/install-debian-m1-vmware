# Install Debian on VMWare Fusion with a Apple Silicon (M1 Pro/Max) (Aug 2022)

Grab the ISO of Debian, drag/drop into VMWare, once VMWare boots GRUB, pick the older kernel in this case `5.10.0`. You'll notice you'll get errors if you try to run anything as root:

<img width="1522" alt="Screen Shot 2022-08-03 at 2 39 40 PM" src="https://user-images.githubusercontent.com/20936398/182717322-764ddd2f-b7de-4b65-989a-8d304acfbf14.png">

There's been a few workarounds that have worked for others but not for me, so I'll list them here, in your GRUB console, you want to run: 

```bash
aspci=force
```
Now you've done that, press continue, select what kernel you want - hover over it and press `e` you should see a screen with some configuration on it: 

<img width="1685" alt="Screen Shot 2022-08-03 at 2 47 36 PM" src="https://user-images.githubusercontent.com/20936398/182718460-63dd8e7a-ce8e-4968-9c29-a053b48d61cb.png">

In the above configuration, add this line:

```bash
modprobe.blacklist=vmwgfx
```

Now that you've blacklisted `vmwgfx`, try to now boot into Debian, and hopefully it doesn't get stuck on boot, make sure all your sensitive configurations are set: 

<img width="1614" alt="Screen Shot 2022-08-03 at 2 45 59 PM" src="https://user-images.githubusercontent.com/20936398/182718270-63583bdb-a13a-4ffc-b7f2-1da29ced2f37.png">

So you got in, and now you're trying to run commands, you'll notice you'll get the error "This account isn't in under any sudoer list", let's take a look inside the `/etc/sudoers`: 

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

## GRUB

Below I'll show you my GRUB configuration, a lot of people have been having issues with getting a full screen, and high resolution, even when saying using something like `xrandr` to set these variables. The workaround lays in your GRUB config, this is what mine currently looks like: 

<img width="669" alt="Screen Shot 2022-08-03 at 2 41 49 PM" src="https://user-images.githubusercontent.com/20936398/182717744-3c99a242-b711-4c8e-a15d-5d5619ca1ef7.png">

You'll see `GRUB_GFXMODE=1920x1200` is set, once that is set, run `sudo reboot`, and you should be running in high res and full screen.

## Final housekeeping 

You may want to add this to your `/etc/fstab`: 

```bash
vmhgfs-fuse /mnt/hgfs fuse defaults,allow_other 0 0
```
Now your install on the older kernel, should be complete! 

<img width="1035" alt="Screen Shot 2022-08-03 at 2 52 00 PM" src="https://user-images.githubusercontent.com/20936398/182719049-826f2ae4-03d5-4e49-b361-d448fa25cc9d.png">

Author: Montana Mendy
