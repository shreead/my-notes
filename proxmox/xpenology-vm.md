# Setup Xpenology VM on Proxmox
Source: [https://blog.nootch.net/post/poor-mans-synology-nas-on-proxmox/](https://blog.nootch.net/post/poor-mans-synology-nas-on-proxmox/)

## Proxmox setup
1. wget arc-xx.iso.zip from https://github.com/AuxXxilium/arc/releases
2. unzip
3. Create VM with:
   - virtIO everything
   - qemu-agent
   - no disk, no CDROM
   - 2 core, 4GB RAM
4. `qm disk import <VMID> arc.img <storage>`
5. Via GUI import drive as SATA
6. Add another disk as SATA

## Boot VM
1. Boot into Arc config
2. Install with Arc patch
3. Build Arc loader and reboot

## Install and configure DSM
1.  Boot into Arc DSM mode
2.  Complete the rest from http://IP:5000
