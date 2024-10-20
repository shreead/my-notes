# Basics
## Scrub
- Start scrub:  `zpool scrub tank`
- Stop scrub:  `zpool scrub -s tank`

## Check free space
- `zpool list (-v) (tank)`
- `zfs list -o space (tank/docker)`

## Replicate without root
- `sudo zfs allow -u shree send,snapshot,hold tank`

