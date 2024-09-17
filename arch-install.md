# Arch Install
## Sources
- [https://wiki.archlinux.org/title/Installation_guide](https://wiki.archlinux.org/title/Installation_guide)
- [https://gist.github.com/furdarius/b9e907227f01ba342040246638a718f3](https://gist.github.com/furdarius/b9e907227f01ba342040246638a718f3)
- [https://phoenixnap.com/kb/arch-linux-install](https://phoenixnap.com/kb/arch-linux-install)

## Installation steps
### Boot into installation media
- [download](https://archlinux.org/download/) ISO, burn to USB, boot to installation screen command prompt (aka archiso)
- in BIOS, choose UEFI, but disable secure boot for now 
- may do zfs in the future like [this](https://github.com/eoli3n/archiso-zfs)

### Confirm UEFI 64 bit
```
cat /sys/firmware/efi/fw_platform_size
# should say 64
```

### Connect to wifi
```
iwctl
> device list
> station wlan0 scan
> station wlan0 get-networks
> station wlan0 connect <SSID_NAME>
> station wlan0 show
> exit
ip a
```

### Confirm date/time is accurate
`timedatectl`

### Format disks and mount partitions
```
fdisk -l

mkfs.fat -F 32 /dev/sda1
mkfs.ext4 /dev/sda2
mkswap /dev/sda3
# /dev/sda4 is zfs

mount /dev/sda2 /mnt
mount --mkdir /dev/sda1 /mnt/boot
swapon /dev/sda3
```

### Install packages
```
pacstrap -K /mnt
base base-devel
linux linux-firmware
intel-ucode
sudo nano man git
grub efibootmgr
networkmanager
```

### Generate fstab
```
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab
```

### chroot to /mnt
```
arch-chroot /mnt

# adjust time
ln -sf /usr/share/zoneinfo/America/Detroit /etc/localtime
hwclock --systohc

# adjust locale
nano /etc/locale.gen
# uncomment en_US.UTF-8 UTF-8

locale-gen

nano /etc/locale.conf
LANG=en_US.UTF-8

# adjust hostname
nano /etc/hostname
acerold
```

### Set root password
`passwd`

### Install bootloader (GRUB)
```
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

# sometimes backup path may be necessary
grub-install --target=x86_64-efi --efi-directory=/boot --removable

grub-mkconfig -o /boot/grub/grub.cfg
```

### Exit and reboot
```
exit
reboot
```

## After boot

### Add new user
```
useradd -mG wheel shree
passwd shree

nano /etc/sudoers
visudo
%wheel ALL=(ALL:ALL) ALL

sudo whoami
# should say root
```

### Connect to wifi
```
sudo systemctl start NetworkManager
sudo systemctl enable NetworkManager
nmcli device wifi list
nmcli device wifi connect <SSID_NAME> password <WIFI_PASSWORD>
```

### Update all packages
```
sudo pacman -Syu
```

### Install XFCE
```
sudo pacman -S xfce4 xfce4-goodies lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings
sudo systemctl enable lightdm.service
reboot 
```

### Congratulations!!
