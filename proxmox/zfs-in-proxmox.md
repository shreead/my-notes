# A very short guide into how Proxmox uses ZFS
Source: [https://www.reddit.com/r/Proxmox/comments/jppohv/a_very_short_guide_into_how_proxmox_uses_zfs/](https://www.reddit.com/r/Proxmox/comments/jppohv/a_very_short_guide_into_how_proxmox_uses_zfs/)
  

## Level 1: Pool ( zpool command )

  

A zpool is a storage space composition of one or more disks in stripe(raid0), mirror(raid1) or raidz arrangement(s). Optionally you can also add (ssd) disks dedicated for special purposes like cache,metadata, deduplication tables or write-logs. Use the zpool command to manage the pool i.e. crate a new pool/add/remove devices.

  

ZFS stores the pool's configuration in a header in all its parttaking drives - so when you would plug them all out and reconnect them i.e. on different SATA ports it would automatically find them and recognize their role in the pool. _NOTE: In some cases, i.e. when the drives were plugged onto another system, you need to manually help ZFS via the "zfs import" command._

  

Each pool has a name. Here: "rpool".

  

Example: Here is a _**zpool list -v**_ of a pool named "rpool" containing 2 striped disks with a total size 79G

  

NAME SIZE ALLOC FREE ... HEALTH ...

rpool 79G 29.6G 49.4G ONLINE

sda4 49.5G 8.38G 41.1G ONLINE

sdb 29.5G 21.3G 8.24G ONLINE

  

## Level 2: ZFS dataset tree (zfs command)

  

In that pool, ZFS can store multiple datasets on which each you can use zfs's cool features like snapshot/rollback/clone(=small, linked copy-on-write clone)/send a delta to another host. Also you can individually _**zfs set**_ compression, encryption, disk space quota, deduplication etc. on them.

  

**Behind a dataset is** either a **filesystem** (which is instantly available = no need to run _mkfs_ or _resizefs_ and it is also mounted automatically) **or a** **volume** (block device/disk with a fixed size).

  

These datasets are stored hiararchically in a tree, let's just use the word ZFS dataset tree for it (contrary to Linux-tree which are the normal files that you see when you type _ls_). Here is that ZFS dataset tree (command: **zfs list**) in a fresh pve installation with ZFS partitioning chosen during the installer plus some sample vms/ct's added:

  

[

  

![r/Proxmox - zfs list](https://preview.redd.it/9f9eb5a02tx51.png?width=1290&format=png&auto=webp&s=ce71facc160647521fd11e1c1533b4236c6e321f)

  

](https://preview.redd.it/9f9eb5a02tx51.png?width=1290&format=png&auto=webp&s=ce71facc160647521fd11e1c1533b4236c6e321f "Image from r/Proxmox - zfs list")

  

zfs list

  

_As we see, we have some datasets that act only as an organizational (ZFS-)"folder". Anyway they are mounted into the (Linux-) file tree but no one ever stores a single (Linux-)file or (Linux-)folder in there._


## Level 3: Using the datasets's filesystems and volumes in pve ( pvecm command or gui)

  

The pve datacenter has 2 storages defined for diffrent purposes (click on Datacenter->Storage):

  

1. **local-zfs** (type: zfspool*) **for block-devices** which points to **rpool/data** in the ZFS dataset tree (see above). Here it can store its vm-drives and use all the cool zfs features (like mentioned above) + also use trim/discard to mark blocks in the middle as free. _*NOTE: "zfs-dataset" would be the more accurate term here._

2. **local** (type: dir) **for normal files** -> **/var/lib/vz.** Just a linux directory where it can store normal files for backups, ISOs and templates. (under the hood, everything under / is backed by ZFS and strored in the dataset rpool/ROOT/pve-1 as you see in the above tree)

  

### More info

  

A good docu is the zpool and zfs man pages.

  

[PVE admin guide: ZFS on Linux](https://pve.proxmox.com/pve-docs/pve-admin-guide.html#storage_zfspool)

  

[PVE admin guide: Local ZFS Pool Backend](https://pve.proxmox.com/pve-docs/pve-admin-guide.html#storage_zfspool)