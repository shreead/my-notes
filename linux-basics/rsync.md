# rsync
Example:
```
rsync -axXv --progress <source> <destination>
```

Options:  
```
-P --progress  # shows progress during transfer

--info=progress2  # stats for whole transfer

--dry-run

| pv -leps NUMBER-OF-FILES --> for progress bar
```

## rsync daemon
Run rsync as daemon to accept file transfers

#### On target machine:
Configuration stored in `/etc/rsyncd.conf`
```
[my-rsync-backup]
path = /path/to/backup/target
hosts allow = 10.0.0.10
hosts deny = *
list = true
uid = root
gid = root
read only = false
```

Enable and start the service:
```
sudo systemctl start rsync
sudo systemctl enable rsync
```

#### On source machine
Exclude list: `/etc/rsync_exclude.lst`
```
foldertoexclude
filetoexclude.txt
```

To start:
```
rsync -avz --delete --exclude-from=/etc/rsync_exclude.lst /host/location/to/upload/ 10.0.0.2::backup-name
```
  

## References:

- https://www.server-world.info/en/note?os=Debian_11&p=rsync
- https://rsync.samba.org/resources.html