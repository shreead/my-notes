# Proxmox Install
## VM
Easiest is to do it as a VM in Proxmox using ttek script:
https://tteck.github.io/Proxmox/#home-assistant-os-vm
```
bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/vm/haos-vm.sh)"
```

VM allows add-on store, HACS etc.

## System Settings
HA > Settings > System

## Add-ons
- Advanced SSH & Web Terminal
- File manager (to edit configuration.yaml)
- Mobile app

## Nginx Proxy Manager:
ha.304ad.net

Edit configuration.yaml and add these lines:
```
homeassistant:
  external_url: "https://ha.304ad.net"
  internal_url: "http://10.10.0.9:8123"

http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 10.10.0.4 # the IP of my NPM container
    - 10.10.0.0/16
```

## Mobile App
Devices and integration > Mobile App

## Zigbee
- Passthrough USB in Proxmox
- RESTART EVERYTHING!!
- Settings > System > Hardware to confirm
- Device should be autodiscovered in Settings > devices