# Resize VM Disk

## 1. Increase/resize disk from GUI of Proxmox

## 2. Extend physical drive partition

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

## 3. Extend Logical volume

```
# view logical volume
lvdisplay

# resize logical volume to 100%
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv

# confirm
lvdisplay
```



## 4. Resize Filesystem

```
# resize filesystem
resize2fs /dev/ubuntu-vg/ubuntu-lv

# confirm results
fdisk -l
```
