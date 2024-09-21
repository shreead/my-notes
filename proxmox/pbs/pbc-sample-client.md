Sample backup script
```
#!/bin/bash

export PBS_PASSWORD='longterm password created for user'
export PBS_USER_STRING='USER@pam!Proxmox-Backup-Client'
export PBS_SERVER='192.168.X.X'
export PBS_DATASTORE='THE-PBS-DATASTORE'
export PBS_REPOSITORY="${PBS_USER_STRING}@${PBS_SERVER}:${PBS_DATASTORE}"
export PBS_HOSTNAME="$(hostname -s)"
echo "Run pbs backup for $PBS_HOSTNAME ..."
proxmox-backup-client backup ${PBS_HOSTNAME}.pxar:/ --include-dev /etc/pve --skip-lost-and-found \
	&& proxmox-backup-client prune host/${PBS_HOSTNAME} --keep-weekly 4 \
	&& curl healthchecks script
```