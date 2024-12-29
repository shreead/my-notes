# Misc
- sha256sum:
  - `sha256sum filename.iso`
  - `echo "CHECKSUM filename.iso" | sha256sum -c`

- dd
  - `dd if=filename.iso of=/dev/sdb bs=4M status=progress`
 
# Delete a filetype
```
find . -name "*.bak" -type f -delete
```



