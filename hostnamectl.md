# hostnamectl
## Usage:
```
$ hostname
server-prod01
```

```
$ hostnamectl
 Static hostname: server-prod01
       Icon name: computer-desktop
         Chassis: desktop 🖥️
      Machine ID: <machine_id>
         Boot ID: <boot_id>
Operating System: Ubuntu 24.04.1 LTS              
          Kernel: Linux 6.8.0-47-generic
    Architecture: x86-64
 Hardware Vendor: Dell Inc.
  Hardware Model: OptiPlex 7050
Firmware Version: 1.27.0
   Firmware Date: Mon 2023-09-18
    Firmware Age: 1y 1month 1w 6d
```
## Change hostname
```
# hostnamectl hostname server01
```
Also change the hosts file `/etc/hosts`
```
127.0.0.1 localhost
127.0.1.1 server01.my.domain server01
```
Reboot
