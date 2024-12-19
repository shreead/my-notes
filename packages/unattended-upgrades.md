## 1. Install unattended-upgrades
```
sudo apt install unattended-upgrades
```

## 2. Create these two files
`/etc/apt/apt.conf.d/20auto-upgrades`
```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```
`/etc/apt/apt.conf.d/50unattended-upgrades`
```
Unattended-Upgrade::Allowed-Origins {
        "${distro_id}:${distro_codename}";
        "${distro_id}:${distro_codename}-security";
        "${distro_id}:${distro_codename}-updates";
        //"${distro_id}ESMApps:${distro_codename}-apps-security";
        //"${distro_id}ESM:${distro_codename}-infra-security";
};

Unattended-Upgrade::Mail "{{ MY_EMAIL }}";
Unattended-Upgrade::MailOnlyOnError "true";
Unattended-Upgrade::Sender "unattended-upgrades@{{ inventory_hostname }}";

Unattended-Upgrade::Automatic-Reboot "ture";
Unattended-Upgrade::Automatic-Reboot-WithUsers "false";
Unattended-Upgrade::Automatic-Reboot-Time "04:00";


```

## 3. Restart unattended-upgrades service
```
sudo systemctl restart unattended-upgrades
```

### Ref:
https://github.com/mvo5/unattended-upgrades/blob/master/README.md
