# Create Ubuntu cloud-init template

## Steps:
1. Download the cloud image from ubuntu:
```sh
wget https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img
```

2. Resize
```sh
qemu-img resize noble-server-cloudimg-amd64.img 32G
```

3. Create VM
```sh
qm create 999 --name "ubuntu-2404-cloudinit-template" --ostype l26 --memory 1024 --agent 1 --bios seabios --machine q35 --cpu host --socket 1 --cores 1 --vga virtio --net0 virtio,bridge=vmbr0
```

4. Import disk
```sh
qm importdisk 999 noble-server-cloudimg-amd64.img local-zfs
```

5. Attach disk to VM:
```sh
qm set 999 --scsihw virtio-scsi-pci --virtio0 local-zfs:vm-999-disk-0,discard=on
# or qm set $VMID --scsihw virtio-scsi-pci --virtio0 shiny:$VMID/vm-$VMID-disk-0,discard=on
```

6. Set boot order:
```sh
qm set 999 --boot order=virtio0
```

7. Attach empty cloudinit drive
```sh
qm set 999 --scsi1 local-zfs:cloudinit
```

8. Configure cloudinit
```sh
wget https://github.com/my.keys
# qm set 999 --tags ubuntu-template,24.04,cloudinit
qm set 999 --ciuser <USERNAME>
qm set 999 --cipassword $(openssl passwd -6 <PASSWORD>)  # hash password with SHA512
qm set 999 --sshkeys my.keys
qm set 999 --ipconfig0 ip=dhcp
```

9. Convert to template
```
qm template 999
```


## Script:
```sh
#!/bin/bash
VMID=9999
STORAGE=shiny

wget https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img

qemu-img resize noble-server-cloudimg-amd64.img 32G

qm create $VMID --name "ubuntu-2404-cloudinit-template" --ostype l26 --memory 1024 --agent 1 --bios seabios --machine q35 --cpu host --socket 1 --cores 1 --vga virtio --net0 virtio,bridge=vmbr0

qm importdisk $VMID noble-server-cloudimg-amd64.img $STORAGE

qm set $VMID --scsihw virtio-scsi-pci --virtio0 $STORAGE:$VMID/vm-$VMID-disk-0.raw,discard=on
# or qm set $VMID --scsihw virtio-scsi-pci --virtio0 local-zfs:vm-$VMID-disk-0,discard=on

qm set $VMID --boot order=virtio0

qm set $VMID --scsi1 $STORAGE:cloudinit

wget https://github.com/my.keys

qm set $VMID --ciuser username

echo "Enter password (will be shown in plain text):"
read userpass
qm set $VMID --cipassword $(openssl passwd -6 $userpass)

qm set $VMID --sshkeys my.keys

qm set $VMID --ipconfig0 ip=dhcp

qm template $VMID

```

# References
- [https://github.com/UntouchedWagons/Ubuntu-CloudInit-Docs](https://github.com/UntouchedWagons/Ubuntu-CloudInit-Docs)