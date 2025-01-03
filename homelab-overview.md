# Homelab Overview
## Internet
ISP provided locked modem/AP
Second router behind it - Mikrotik Hex S

## Servers
### Proxmox
LXC (Plex) and VMs
Specs:
- Motherboard: ASRock B660M PRO RS
- Processor: Intel Core i5-12400 2.5GHz 6C/12T
- Memory: 64GB DDR4-3200
- Storage: 10 HDD bays, 4 SSD bays.
- ZFS 6x12TB, raidz1, 3 wide, 2 vdevs, 43TiB usable 

### TrueNAS
Pull Backup from other servers
Specs:
- Motherboard: Supermicro X11SSL-F
- Processor: Intel Xeon i3-1220 v6 
- Memory: 64GB

### Dell Wyse 7050
Services:
- Pihole + Unbound

### Dell Optiplex MFF
Services:
- Bookstack
- Frigate
- Home Assistant
- Traefik
- Unifi Controller


