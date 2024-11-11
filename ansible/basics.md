# Ansible Basics
## Install
Ubuntu:
```
pipx install --include-deps ansible
pipx upgrade --include-injected ansible
```
Arch: [https://wiki.archlinux.org/title/Ansible](https://wiki.archlinux.org/title/Ansible)
```
sudo pacman -S ansible
```

## Configuration
Generate configuration file:
```
ansible-config init --disabled > ansible.cfg
```
Important lines:
```
[defaults]
inventory=./hosts

[privilege_escalation]
become_ask_pass=True
```

## Inventory
Define location in `ansible.cfg` as above. 

Sample `hosts`:
```
[webservers]
server01
10.10.1.5
web01.example.com

[vms]
server02 ansible_host=10.10.1.2 ansible_port=22 ansible_user=some-user
server03 ansible_user=another_user
```
List inventory:
```
ansible-inventory --list -i hosts
```

## Ad-hoc commands
Format:
```
ansible [pattern] -i [inventory-file] -m [module-name] -a "[module options]" --key-file [ssh-key-file] --ask-become-pass
```
Initial ping:
```
ansible all --key-file ~/.ssh/ansible-key -i inventory -m ping --ask-become-pass
```
Ping only:
```
ansible all -m ping
```
Gather facts:
```
ansible all -m gather_facts
ansible all -m gather_facts --limit server01 | grep ansible_distribution
```
Elevated ad-hoc commands:
```
ansible all -m apt -a update_cache=yes --become --ask-become-pass
```
Use cases for ad-hoc commands:
```
# Reboot
ansible vm02 -a "/sbin/reboot" -f 10 -u shr --become --ask-become-pass

# Copy file
ansible vm02 -m ansible.builtin.copy -a "src=/etc/hosts dest=/tmp/hosts mode=600 owner=shr group=shr"

# Install packages
ansible vms -m ansible.builtin.apt -a "name=vim state=present"

# Start a service
ansible webservers -m ansible.builtin.service -a "name=httpd state=started"
```


