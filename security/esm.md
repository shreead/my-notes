# Ubuntu Pro ESM
**Install essentials**
```
sudo apt install ubuntu-advantage-tools
```

Check status:
```
pro status
```
Check source repository of installed packages
```
pro security-status
pro security-status --esm-apps   # list of packages covered by esm-apps
```

Enable pro:
```
sudo pro attach
```
and then follow instructions on https://ubuntu.com/pro/attach using the provided code, or 
**Login to the website to get the token and then do**
```
sudo pro attach [TOKEN]
```
Update/upgrade apps
```
sudo apt update && sudo apt upgrade -y
```
Also enable on unattended-upgrades file `/etc/apt/apt.conf.d/50unattended-upgrades`
```
Unattended-Upgrade::Allowed-Origins {
        "${distro_id}ESMApps:${distro_codename}-apps-security";
        "${distro_id}ESM:${distro_codename}-infra-security";
};

```