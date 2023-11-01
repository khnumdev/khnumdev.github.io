---
layout: post
title:  "My current linux cheatsheet/setup"
date:   2023-11-01 19:47:20 +0200
categories: linux
---

Since I've joined [Worklytics](https://www.worklytics.co/) I'm using Linux as operanting system for my daily work there. I've used Linux first time
when I was at high-school and then when I was at university, but then I've switch to Microsoft stack including Windows and
rest of .NET things.

The purpose of this post is for me, as a note for the future with the links I've found useful for setup and using Ubuntu since then.
All credits belongs to the author of the related posts, I've just put there as a compilation.

## Dual boot with Bitlocker

First time I've installed Linux it was a like a pain. Since the release of Kubuntu (yeah I'm not as younger as I'd like to be) and Ubuntu the installation process, updates and package manager have improved a lot. Now just downloading the ISO of Ubuntu and put it in a USB pen is enough to setup the whole Linux system. The installation provides all the partition setup required for that. 

But for sure this is more complicated if you want to keep two SOs (Windows and Linux) over the same computer, having both with disk encryption features enabled. 

**NOTE**: This is only when two features are enabled. If Bitlocker/encryption are not enabled (and you SHOULD use it!) this post is not useful.

I've follow the detailed description of steps from [Mike Kasberg dual boot ubuntu and linux](https://www.mikekasberg.com/blog/2020/04/08/dual-boot-ubuntu-and-windows-with-encryption.html) which describe the following steps (leaving steps here in case of the post is removed/lost):
1. As a prerequisite, we are going to do a clean installation of both systems.
2. Make a USB pen with Ubuntu installation
3. Create the partitions from Ubuntu live installation:

```bash
sudo su
# sgdisk --zap-all /dev/sda
# sgdisk --new=1:0:+550M /dev/sda
# sgdisk --change-name=1:EFI /dev/sda
# sgdisk --typecode=1:ef00 /dev/sda
# mkfs.fat -F 32 /dev/sda1
```

4. Install Windows and use the partitioning tool, for creating a new partition
5. Enable Bitlocker
6. Check partition status:

```bash
ubuntu@ubuntu:~$ sudo sgdisk --print /dev/sda
Disk /dev/sda: 500118192 sectors, 238.5 GiB

Number  Start (sector)    End (sector)  Size       Code  Name
   1            2048         1128447   550.0 MiB   EF00  EFI
   2         1128448         1161215   16.0 MiB    0C01  Microsoft reserved ...
   3         1161216       167825076   79.5 GiB    0700  Basic data partition
   4       167825408       168900607   525.0 MiB   2700
```
7. Create partitions from Ubuntu live USB:

```bash
$ sudo su
# sgdisk --new=5:0:+1800M /dev/sda
# sgdisk --new=6:0:0 /dev/sda
# sgdisk --change-name=5:/boot --change-name=6:rootfs /dev/sda
# sgdisk --typecode=5:8300 --typecode=6:8300 /dev/sda
# mkfs.ext4 -L boot /dev/sda5
```

8. Setup LUKS:

```bash
# cryptsetup luksFormat --type=luks2 /dev/sda6
WARNING!
========
This will overwrite data on /dev/sda6 irrevocably.
   
Are you sure? (Type uppercase yes): YES
Enter passphrase for /dev/sda6: 
Verify passphrase: 
   
# cryptsetup open /dev/sda6 sda6_crypt
Enter passphrase for /dev/sda6: 
   
# ls /dev/mapper/
control sda6_crypt
```

9. Create new partitions for Ubuntu:

```
# pvcreate /dev/mapper/sda6_crypt
Physical volume "/dev/mapper/sda6_crypt" successfully created.
# vgcreate ubuntu-vg /dev/mapper/sda6_crypt
Volume group "ubuntu-vg" successfully created
# lvcreate -L 8G -n swap_1 ubuntu-vg
Logical volume "swap_1" created.
# lvcreate -l 100%FREE -n root ubuntu-vg
Logical volume "root" created.
```

10. Install Ubuntu, but **choosing* the partitions created in previous step instead of leaving the installer do it itself
11. [Setup the password to decrypt](https://www.mikekasberg.com/blog/2020/04/08/dual-boot-ubuntu-and-windows-with-encryption.html#phase-4-install-ubuntu) the disk


Now the computer is setup with dual boot, Bitlocker and LUKS. Note at this point, *any* change done by updates (for example, updating firmware from Ubuntu updates) may modify the boot partition causing that Bilocker *detects* that a change has been done in the disk, requesting you your key to allow you to unblock your computer on next reboot. This is why I've try to update the main stuff from Windows and *not* from Ubuntu.

## Git coloured current branch in terminal

This is from my friend [Kartones](https://github.com/Kartones), he shared some terminal settings that are interesting to have. This one is to have the existing git branch coloured as part of the path of the terminal line, so it is easy to know where you are.

Edit `.bashrc` file and add the following:

```
~/.bashrc

# enable git prompt
# source ~/.git-prompt.sh
# PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[01;34m\] \w\[\033[00m\]$(__git_ps1) \$ '
parse_git_branch() {
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
export PS1="\u@\h \w\[\033[32m\]\$(parse_git_branch)\[\033[00m\] $ "
```

And close terminals and reopen them again. 

## Speaker issues (not solved!)

Still not solved. Sound drivers still are a nightmare in Linux and in my case, if I connect a set of speakers from the 3.5 mm jack I heard an electrical noise as sound. It is not a computer issue because that is not happening in Windows.

This is the [post](https://askubuntu.com/questions/1230833/annoying-click-popping-sound-on-ubuntu-20-04) where some solutions are present; I've read that the one to 
enable the [auto-mute](https://askubuntu.com/a/1351408) from alsamixer works for some people, but not in my case.

On my computer, it seems like the headset input (but it is selected headphones when plugged in) is producing the internal input sound, colliding with the microphone. Weird, but if I disable it there are no noise (and no sound!). However with a bluetooth headset there is no problem.

There is another [post](https://askubuntu.com/questions/1241617/ubuntu-20-04-after-last-update-speakers-are-buzzing-unless-i-open-the-sound-s) with more details; playing with internal mic volume and gain improved the noise but still is there.

My current solution is to have the speakers in low volume, as they have a physical volume regulator and then have the system volume in high values.

## Low space in boot partition

After several updates, the boot partition (the lower one) will complain that there is no enough free space. That could be a problem because it avoids you to continue updating the system. Ubuntu offers a partition tool to free space if you know what you need to remove.

The other option is to run the autoremove command:

```bash
sudo apt autoremove
```

That works normally like a charm, but it wasn't my case. After some research I've discovered that I had a lot of old linux kernels in the partition that weren't removed and they cause to have the partition with lower space. The solution is easy: delete the old kernels but taking into account that you should delete the OLD ones, not the current ones in use. And as I've said before, I'm old enough to have suffered on my prime the best kernel panics and issues with Linux. I don't want that
on my current computer again.

I've found [this post](https://askubuntu.com/questions/1423156/ubuntu-22-upgrade-needs-extreme-amount-of-space-in-boot-partition/1449412#1449412) where it exposes a command to drop from the system all the kernels not used:

```bash
$ dpkg -l | egrep "linux-(signed|modules|image|headers)" | grep -v $(uname -r | cut -d - -f 1) | awk {'print $2'} | xargs sudo apt purge -y
```

And after that, the lower space issue was fixed.