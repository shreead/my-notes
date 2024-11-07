# zpool commands
## Trim
Improve write performance of SSDs esp if they're almost full

Check status:
```
zpool status -t tank
```
Start trimming:
```
zpool trim tank
```
Set auto trim:
```
zpool set autotrim=on tank
```
