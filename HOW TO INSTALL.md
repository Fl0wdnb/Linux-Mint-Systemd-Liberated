# Linux Mint Debian Edition (liberated from SystemD)

### Summary
Using a simple algorithm, you can add the LMDE repos to a Devuan base installation, install the necessary keyring and files for LMDE and boot into Linux Mint (either cinnamon, mate or xfce)
#### Justification
LMDE uses SystemD, which is inherently NOT an init system, but a program suffering from software control creep (like Microsoft). Many Like Linux Mint, but hate SystemD. This guide will explain how to liberate Linux Mint for the anti-SysD users.
#### Steps to create
1. Install Devuan Chimaera as a base installation. You may want to include tools for networking. If you cannot, prepare a Linux Live image and chroot just like you would installing Gentoo.
2. Obtain network access if you have not already done so (For non-chroot method, skip the mounting and move on to step 4). If you cannot, mount the filesystem, proc, sys, dev and run:
<br>
sudo su
<br>
mount /dev/sdX /mnt
<br>
mount --types proc /proc /mnt/proc
<br>
mount --rbind /sys /mnt/sys
<br>
mount --make-slave /mnt/sys
<br>
mount --rbind /dev /mnt/dev
<br>
mount --make-rslave /mnt/dev
<br>
mount --bind /run /mnt/run
<br>
mount --make-slave /mnt/run
<br><br>
    3. Temporarily change the Root Filesystem using the chroot command:
<br><br>
chroot /mnt or chroot /dev/sdX
<br><br>
    4. Update APT under root privileges. Install vim, sudo and mc (optional package to find things).
<br>
su (do not type if chrooted, because you should already be in root. It's a waste of energy but it still works)
<br>
apt update
<br>
apt install -y vim mc sudo
<br><br>
    5. Navigate to /etc/apt under mc or using cd
<br>
mc
<br>
OR
<br>
cd /etc/apt
<br><br>
    6. Use either vim or prefered text editor to edit the sources.list file
<br>
vim sources.list
<br>
nano -w sources.list
<br>
emacs sources.list
<br>
echo [add text here] >> sources.list
etc...
<br><br>
    7. Add the repo for LMDE
<br>
deb http://packages.linuxmint.com/ elsie main upstream import backport
<br><br>
    8. Update APT again using the allow insecure repos command. Install the Linux Mint Keyring for GPG using the bypass unauthenticated flag
<br>
apt update --allow-insecure-repositories
<br>
apt install --allow-unauthenticated linuxmint-keyring
<br><br>
    9. Find the dependencies for mint-meta-core (listed to save you the effort). Install all dependencies except for mintupdate (systemd dependent program)
<br>
sudo apt install -y linuxmint-keyring mint-info mint-artwork debian-system-adjustments mint-translations mintbackup, mintinstall mintsystem mint-mirrors mintreport mintsources mintwelcome command-not-found inxi grub2-theme-mint
<br><br>
    10. Install one of 3 GTK Desktops for Linux Mint (more choices than LMDE)
<br>
apt install mate-desktop mate-tweak tilda plank
<br>
apt install xfce4
<br>
apt install cinnamon
<br><br>
    11. Exit chroot if using that method and reboot
<br>
exit
<br>
sudo reboot
