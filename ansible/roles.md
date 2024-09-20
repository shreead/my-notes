# Ansible Roles
## Folder structure
```
playbooks
└─ roles
    ├─ apt-upgrader
    |   ├─ defaults
    |   ├─ files - files to be transferred
    |   ├─ handlers
    |   ├─ meta
    |   └─ tasks - main list of tasks
    └─ moted-cleaner
        ├─ defaults
        ├─ files
        ├─ handlers
        ├─ meta
        └─tasks
```

## Automate folder creation
```
cd roles
ansible-galaxy init apt-upgrader
```