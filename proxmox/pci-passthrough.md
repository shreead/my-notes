PCI Passthrough

nano /etc/default/grub
update this line to:
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=off i915.enable_gvt=0"

update-grub



nano /etc/modules

vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd

