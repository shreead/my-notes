# Plex LXC
## Enable Hardware Transcode

https://www.reddit.com/r/Proxmox/comments/136r72x/intel_igpu_lxc_passthrough_for_plex_hardware/

Edit `/etc/pve/lxc/101.conf` which should contain:
```
arch: amd64
cores: 2
hostname: plex
memory: 4096
mp0: /rusty/media,mp=/media
net0: name=eth0,bridge=vmbr0,firewall=1,gw=10.10.0.1,hwaddr=12:34:56:78:90:AB,ip=10.10.xx.xx/16,type=veth
onboot: 1
ostype: ubuntu
rootfs: shiny:101/vm-101-disk-1.raw,size=32G
swap: 1024
tags: 24.04;ubuntu
```

And add these lines:
```
lxc.cgroup2.devices.allow: a
lxc.cap.drop:
lxc.cgroup2.devices.allow: c 226:0 rwm
lxc.cgroup2.devices.allow: c 226:128 rwm
lxc.cgroup2.devices.allow: c 29:0 rwm
lxc.mount.entry: /dev/fb0 dev/fb0 none bind,optional,create=file
lxc.mount.entry: /dev/dri dev/dri none bind,optional,create=dir
lxc.mount.entry: /dev/dri/renderD128 dev/dri/renderD128 none bind,optional,create=file
```
