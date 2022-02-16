# Arch Linux Post Installation Setup and Config Scripts

<img src="https://imgur.com/a/AxKsk5m" />

This README contains the steps I do to install and configure a fully-functional Arch Linux installation containing a desktop environment, all the support packages (network, bluetooth, audio, printers, etc.), along with all my preferred applications and utilities. The shell scripts in this repo allow the entire process to be automated.

Setting up an Arch system from scratch is usually a time-intensive process. My goal in developing these scripts and my __[installation guide](https://github.com/TokyoDrift3r/Arch-Linux-Setup)__ was to be able to go from a blank hard drive to a fully functional Arch system with all my files, applications, and preferences set, as efficiently as possible.

Typically a complete install takes me between two and three hours. About and hour for the base install, and a couple hours for all the packages to download. In addition to the scripts in this repo I run a script that copies over my dotfiles, preferences, fonts, git repos, etc., so that when I log into a new system everything is exactly where I left off on my previous one. Almost everything. I run a Windows dev environment on VirtualBox which takes additional setup time, and I have to enable some cloud services so I can access my files.

Note: Some of the package choices and tweaks are specific to the laptop I'm currently running, a Dell XPS 13...which kicks ass and runs Linux flawlessly.

So...

### Don't just run these scripts. Examine them. Customize them. Create your own versions.

---

### System Description
I run Gnome as DE after going back and forth using i3-wm and awesome-wm but didn't feel like adding more configuration scripts to set up my environment. I am a big fan of pywal though for modding terminal colors to align with my wallpaper.

To boot I use `systemd` because it's minimalist, comes built-in, and since the Linux kernel has an EFI image, all we need is a way to execute it.

I also install the LTS Kernel along side the rolling one, and configure my bootloader to offer both as a choice during startup. This enables me to switch kernels in the event of a problem with the rolling one.

I don't run an application dock. The Whisker Menu (which I trigger via a shortcut) allows very fast lookup without needing a mouse. This is similar to Spotlight Search on Mac or the Start menu on Windows.

I set up my machines as development environments since that's mostly what I do. To that end I install lots of dev tools and customize various things to increase productivity. For exmple, I set up Apache server to run as me, with the WebServer directory located in my home folder.

I run my own utilites: __[WifiVPN](https://github.com/rickellis/WifiVPN)__ for network/VPN connectivity, __[AURIC](https://github.com/rickellis/AURIC)__ for AUR package management, and __[ConkyMatic](https://github.com/rickellis/ConkyMatic)__ to theme my Conky whenever I switch wallpaper.

---

The install steps are as follows:

### Install Arch Linux

Follow the steps in my __[Arch Linux Installation Gude](https://github.com/TokyoDrift3r/Arch-Linux-Setup)__. Then:

---

### Boot into new installation
And get online...

    $   sudo iwctl

---

### Install Reflector. 
Reflector allows the fastest Arch mirrors to be used.

    $   sudo pacman -Sy

    $   sudo pacman -S reflector rsync curl

Now generate mirrorlist. Note: If you are not in the U.S. change "United States" to your own country.

    $   reflector --verbose --country 'United States' -l 5 --sort rate --save /etc/pacman.d/mirrorlist

---

### Initialize .gitconfig file
So we can clone this repo...

    git config --global user.name "your-username"
    git config --global user.email "your-email@gmail.com"
    git config --global credential.helper cache
    git config --global credential.helper 'cache --timeout=31536000'

---

### Clone ArchMatic
Typically I just clone it into the home folder and delete it once I'm done.

    $   git clone https://github.com/rickellis/ArchMatic.git

---

### Run ArchMatic files
Run the following scripts:

    $   ./1-xorg.sh
    $   ./2-gnome.sh 
    $   ./3-network.sh 
    $   ./4-bluetooth.sh 
    $   ./5-audio.sh 
    $   ./6-printers.sh 
    $   ./7-software-pacman.sh
    $   ./8-software-aur.sh
    $   ./9-setup.sh

### Reboot

    $   reboot

### Initialize Gnome using gdm:
At the terminal, run:

    $   systemctl enable gdm.service


Congrats!

You should now have an Arch system running Gnome, with all the base packages that allow network connectivity, bluetooth, printers, etc., and a curated selection of applications.

### Additional Resources

I typically install the __[Arc Colora Themes](https://github.com/arcolinux/arcolinux-arc-themes)__. Just create a `.themes` folder in your home directory and place the Colora themes in there.
