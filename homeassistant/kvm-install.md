# KVM Install

source: https://community.home-assistant.io/t/install-home-assistant-os-with-kvm-on-ubuntu-headless-cli-only/254941

## 1 - Preparation

First, check that your CPU can run VMs.

```
sudo grep -c -E '(vmx|svm)' /proc/cpuinfo
```

As long as it doesn’t return 0, then you’re good.  
Next, check that it can use KVM acceleration. 

```
sudo kvm-ok
```

### 1.1 - Install KVM and dependencies

KVM might be installed already, the same with OVMF (needed to start a VM with UEFI) but just in case.

```
apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils ovmf virt-manager
```

### 1.2 - Add user to libvirt and kvm groups

```
adduser shr libvirt
adduser shr kvm
```

where shr is the user that will deploy VMs

### 1.3 - Check that install was successful

```
systemctl status libvirtd
```

It should say that it is running. If not, then

```
systemctl enable --now libvirtd
```

### 1.4 - Create directories and download image

Find the current qcow2 image from the Home Assistant install page, and replace the link after the wget command.

Link to find image: https://www.home-assistant.io/installation/alternative

```
mkdir -vp /var/lib/libvirt/images/hassos-vm && cd /var/lib/libvirt/images/hassos-vm
wget https://github.com/home-assistant/operating-system/releases/download/13.1/haos_ova-13.1.qcow2.xz
```

### 1.5 - Set up a storage pool

```
virsh pool-create-as --name hassos --type dir --target /var/lib/libvirt/images/hassos-vm
```

## 2 - Set up network

Create a new network bridge (that we’ll call br0 here).

Edit the Netplan config:

nano /etc/netplan/00-installer-config.yaml

Add lines for the bridge, so the file looks like this afterwards:

```
network:
  ethernets:
    eno1:
      dhcp4: true
  version: 2

  bridges:
    br0:
      dhcp4: yes
      interfaces:
             - eno1
      parameters:
        stp: true
```

Check for any errors with:

```
netplan generate
```

If none, then apply changes, and check:

```
netplan apply
brctl show
```

The last command should output a list of bridges, and the newly created br0 should be among them.

After starting the VM, this command will also show vnet0 under interfaces.

### 2.1 - Turn off firewall for KVM interface

Edit the docker.service

```
sudo systemctl edit docker.service
```

and insert:
```
[Service]
ExecStartPost=/usr/sbin/iptables -I DOCKER-USER -i br0 -o br0 -j ACCEPT
```

Reload the docker daemon and restart docker to take effect.

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 3 - Install HassOS VM

Unzip the xz file:

```
sudo xz -d -v haos_ova-13.1.qcow2.xz
```

Create VM:
```
cd /var/lib/libvirt/images/hassos-vm

virt-install --import --name hass \
--memory 4096 --vcpus 4 --cpu host \
--disk haos_ova-13.1.qcow2,format=qcow2,bus=virtio \
--network bridge=br0,model=virtio \
--osinfo detect=on,require=off \
--graphics none \
--noautoconsole \
--boot uefi
```

Enable autostart:
```
virsh autostart hassos
```

Provide static IP in the router (device name homeassistant).
Or inside HA > Settings > System > Network.

Open [homeassistant](http://10.10.0.9:8123)

Restore from backup if available. 

NOTE: If you’re stuck at UEFI shell, @MrksHfmn suggests that you try disable secure booting from BIOS. See this post 394.

## 4 - Attach USB device
Source: https://community.home-assistant.io/t/install-home-assistant-os-with-kvm-on-ubuntu-headless-cli-only/254941/21

Guide taken mostly from [here](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/virtualization_administration_guide/sect-Managing_guest_virtual_machines_with_virsh-Attaching_and_updating_a_device_with_virsh#proc-Attaching_and_updating_a_device_with_virsh-Hotplugging_USB_devices_for_use_by_the_guest_virtual_machine).

Find the `idVendor` and `idProduct` of the USB device you want to attach.

```
lsusb -v
```

An example from my system, an Aeotec Z-wave:

```
  idVendor           0x0658 Sigma Designs, Inc.
  idProduct          0x0200 Aeotec Z-Stick Gen5 (ZW090) - UZB
```

With the above, create an xml file:

```
sudo nano /var/lib/libvirt/images/hassos-vm/aeoteczwave.xml
```

And insert

```bash
<hostdev mode='subsystem' type='usb' managed='yes'>
  <source>
    <vendor id='0x0658'/>
    <product id='0x0200'/>
  </source>
</hostdev>
```

[NOTE] - the “managed=‘yes’” makes it behave as if “nodedev-detach” and “nodedev-reattach” had been called at correct times, so you shouldn’t need to do anything else.

Then you can attach USB devices using virsh

```
virsh attach-device hassos --file /var/lib/libvirt/images/hassos-vm/aeotechzwave.xml --persistent
```

[Note] use “virsh detach-device [DOMAIN] [FILE]” to detach, where it will reattach again upon next boot. Add “–persistent” to this, to make the detach persistent.

You might have issues where after a reboot of Ubuntu, it doesn’t show up. In that case, I have found that detatching and attaching again solves this, assuming the file is /var/lib/libvirt/images/hassos-vm/aeotechzwave.xml, then do:

virsh detach-device hassos --file /var/lib/libvirt/images/hassos-vm/aeotechzwave.xml --persistent
virsh attach-device hassos --file /var/lib/libvirt/images/hassos-vm/aeotechzwave.xml --persistent

## 5 - After install

```
virsh help
virsh list
virsh list --all  # including stopped

virsh start hassos
virsh shutdown hassos
virsh destroy hassos   # forcibly
virsh destroy hassos --graceful

virsh dumpxml hassos | grep "mac address" | awk -F\' '{ print $2}'
```