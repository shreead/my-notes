# PCI Passthrough

1. Edit grub
`nano /etc/default/grub`

update this line to:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=off i915.enable_gvt=0"
```

2. Update grub
`update-grub`

3. Add modules
`nano /etc/modules`
```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

4. Reboot

5. Proxmox GUI:
- Datacenter > Resource Mappings > PCI > Add - select and give a name
- Add the above named PCI in the VM

