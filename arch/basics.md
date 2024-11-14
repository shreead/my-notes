# Arch basics
## pacman
Options:
```
-S     sync/install  
-Syu   refresh package database and upgrade system  
-Runs  remove packages and dependencies except those that others depend on

-Q     list of installed packages
-Qe    list of explicitly installed packages (or grep from bash history)
-Qeg   show in groups
```

Install packages:
```
sudo pacman -S git bash
sudo pacman -S bash>=3.0
```

Uninstall packages:
```
sudo pacman -Runs neofetch
```

My manually installed packages:
```
bash-completion
firefox
fastfetch htop s-tui
imagemagick
linux-lts linux-lts-headers base-devel git intel-ucode
openssh
ark unzip
xfce4 xfce4-goodies lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings
archzfs-linux zfs-dkms zfs-utils zfs-linux
```