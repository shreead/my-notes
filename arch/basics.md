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

## yay
- arch user repository helper
- search aur, compile a package from source with makepkg and then install it via pacman

Install yay:
```
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

First Use:
-   Use `yay -Y --gendb` to generate a development package database for `*-git` packages that were installed without yay. This command should only be run once.    
-   `yay -Syu --devel` will then check for development package updates
-   Use `yay -Y --devel --save` to make development package updates permanently enabled (`yay` and `yay -Syu` will then always check dev packages)

Examples:
```
yay = yay -Syu
```
Go here for full list: [https://github.com/Jguer/yay](https://github.com/Jguer/yay)

