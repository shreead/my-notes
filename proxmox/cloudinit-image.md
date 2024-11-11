1. Download the cloud image from ubuntu:
`wget https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img`

2. Resize
`qemu-img resize noble-server-cloudimg-amd64.img 32G`

3. Create VM
`qm create 999 --name "ubuntu-2404-cloudinit-template" --ostype l26 --memory 1024 --agent 1 --bios seabios --machine q35 --cpu host --socket 1 --cores 1 --vga serial0 --serial0 socket --net0 virtio,bridge=vmbr0`

4. Import disk
`qm importdisk 999 noble-server-cloudimg-amd64.img local-zfs`

5. Attach disk to VM:
`qm set 999 --scsihw virtio-scsi-pci --virtio0 local-zfs:vm-999-disk-0,discard=on`

6. Set boot order:
`qm set 999 --boot order=virtio0`

7. Attach empty cloudinit drive
`qm set 999 --scsi1 local-zfs:cloudinit`

8. Configure cloudinit
```
wget https://github.com/my.keys
# qm set 999 --tags ubuntu-template,24.04,cloudinit
qm set 999 --ciuser <USERNAME>
qm set 999 --cipassword $(openssl passwd -6 <PASSWORD>)
qm set 999 --sshkeys my.keys
qm set 999 --ipconfig0 ip=dhcp
```
9. Convert to template
`qm template 8001`
