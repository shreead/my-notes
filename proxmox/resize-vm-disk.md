# Resize VM Disk
## For disks using LVM:

1. Increase/resize disk from GUI of Proxmox

2. Extend physical drive partition
```
# check free space
sudo fdisk -l

# extend physical drive partition
growpart /dev/sda 3

# check physical volume size
pvdisplay

# instruct LVM that disk size has changed
sudo pvresize /dev/sda3

# check
pvdisplay
```

3. Extend Logical volume
```
# view logical volume
lvdisplay

# resize logical volume to 100%
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv

# confirm
lvdisplay
```

4. Resize Filesystem
```
# resize filesystem
resize2fs /dev/ubuntu-vg/ubuntu-lv

# confirm results
fdisk -l
```

## For disks not using LVM:
1. Increase/resize disk from GUI of Proxmox

2. Confirm change in guest (vda if using VirtIO):
```
sudo dmesg | grep vda
[ 3982.979046] vda: detected capacity change from 34359738368 to 171798691840
```

3. Print the current partition table
```
sudo fdisk -l /dev/vda
Device       Start       End   Sectors  Size Type
/dev/vda1  2099200 251658206 249559007  119G Linux filesystem
/dev/vda14    2048     10239      8192    4M BIOS boot
/dev/vda15   10240    227327    217088  106M EFI System
/dev/vda16  227328   2097152   1869825  913M Linux extended boot

```

4. Resize the partition to occupy the remaining space: 
```
sudo parted /dev/vda
(parted) print
Fix/Ignore? F 

Number  Start   End     Size    File system  Name  Flags
14      1049kB  5243kB  4194kB                     bios_grub
15      5243kB  116MB   111MB   fat32              boot, esp
16      116MB   1074MB  957MB   ext4               bls_boot
 1      1075MB  129GB   128GB   ext4

(parted) resizepart 1 100%
(parted) quit
```

5. resize2fs
```
sudo resize2fs /dev/vda1
```
