# Docker on ZFS
Source: [https://docs.docker.com/engine/storage/drivers/zfs-driver/](https://docs.docker.com/engine/storage/drivers/zfs-driver/)

## Summary: 
- Mount `/var/lib/docker/` on a ZFS-formatted filesystem.
- Change storage driver to ZFS in `/etc/docker/daemon.json`.

## Steps:
1.  Stop docker and empty folder:
```sh
sudo systemctl stop docker.service docker.socket
sudo cp -au /var/lib/docker /var/lib/docker.bk
sudo rm -rf /var/lib/docker/*
```

2.  Create a zpool or dataset and mount:
```sh
sudo zpool create -f zpool-docker -m /var/lib/docker sda sdb
sudo zfs create zpool/docker -m /var/lib/docker
```

3.  Edit `/etc/docker/daemon.json` and add:
```json
{
  "storage-driver": "zfs"
}
```

4.  Confirm:
```sh
sudo docker info
  Storage Driver: zfs
     Zpool: zpool-docker
     Zpool Health: ONLINE
```