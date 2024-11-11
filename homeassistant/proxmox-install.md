# Home Assistant Proxmox Install
## Tteck Install Script
Easiest is to do it as a VM in Proxmox using tteck script:
https://tteck.github.io/Proxmox/#home-assistant-os-vm
```
bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/vm/haos-vm.sh)"
```

VM allows add-ons

## System Settings
HA > Settings > System

## Add-ons
- Advanced SSH & Web Terminal
- File manager (to edit configuration.yaml)
- Mobile app

## Nginx Proxy Manager:
[Setup reverse proxy](reverse-proxy-npm.md)

## Zigbee
- Passthrough USB in Proxmox
- RESTART EVERYTHING!!
- Settings > System > Hardware to confirm
- Device should be autodiscovered in Settings > devices