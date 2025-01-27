# Basics
## Scrub
- Start scrub:  `zpool scrub tank`
- Stop scrub:  `zpool scrub -s tank`

## Check free space
- `zpool list (-v) (tank)`
- `zfs list -o space (tank/docker)`

## zfs allow
To replicate without root
```
sudo zfs allow -u shree send,snapshot,hold tank
```

## Snapshots
Snapshots are browsable. Inside the dataset directory, there's a browsable .zfs directory.
```
cd tank/data
cd .zfs
cd snapshot
cd zfs_auto_snap_..... 
```
and find the file, cp to another directory

## Checkpoint
Check for checkpoint:
```
zpool get checkpoint tank
zpool list tank
```
Create checkpoint:
```
zpool checkpoint tank
```
Do stuff, and if everything is ok, discard checkpoint:
```
zpool checkpoint -d tank
```
