# Arch Security
## Password quality:
libpquality

`/etc/pam.d/passwd`:
```
#%PAM-1.0
password required pam_pwquality.so retry=2 minlen=10 difok=6 dcredit=-1 ucredit=-1 ocredit=-1 lcredit=-1 [badwords=myservice mydomain] enforce_for_root
password required pam_unix.so use_authtok sha512 shadow
```

## CPU Microcode:
List vulnerabilities:
```
grep -r . /sys/devices/system/cpu/vulnerabilities/ 
/sys/devices/system/cpu/vulnerabilities/spectre_v2:Mitigation: IBRS; IBPB: conditional; STIBP: conditional; RSB filling; PBRSB-eIBRS: Not affected; BHI: Not affected
/sys/devices/system/cpu/vulnerabilities/itlb_multihit:KVM: Mitigation: VMX disabled
```

GDS vulnerability:
```
sudoedit /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="mitigations=auto,nosmt"
# will also disable hyperthreading
GRUB_CMDLINE_LINUX_DEFAULT="mitigations=auto"

sudo grub-mkconfig -o /boot/grub/grub.cfg
```

## Login delay
```
sudoedit /etc/pam.d/system-login

auth optional pam_faildelay.so delay=4000000
```

## Lock root
```
sudo passwd --lock root
```

## References: 
[https://wiki.archlinux.org/title/Security](https://wiki.archlinux.org/title/Security)
