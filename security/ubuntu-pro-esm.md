# Ubuntu Pro ESM
## 1. Install essentials
```
sudo apt install ubuntu-advantage-tools
```

## 2. Check status
```
sudo pro status
```
Check source repository of installed packages
```
sudo pro security-status
sudo pro security-status --esm-apps   # list of packages covered by esm-apps
```

## 3. Enable Pro
```
sudo pro attach
```
and then follow instructions on https://ubuntu.com/pro/attach using the provided code, or 
**Login to the website to get the token and then do**
```
sudo pro attach [TOKEN]
```
`esm-apps`, `esm-infra`, and `livepatch` will be automatically enabled. 

## 4. Update/upgrade
```
sudo apt update && sudo apt upgrade -y
```

## 5. Allow unattended-upgrades
Edit `/etc/apt/apt.conf.d/50unattended-upgrades` to add:
```
Unattended-Upgrade::Allowed-Origins {
        "${distro_id}ESMApps:${distro_codename}-apps-security";
        "${distro_id}ESM:${distro_codename}-infra-security";
};

```
Restart unattended-upgrades
```
sudo systemctl restart unattended-upgrades
```