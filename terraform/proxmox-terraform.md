# Manage Proxmox with Terraform

Using Telmate provider - [https://registry.terraform.io/providers/Telmate/proxmox/latest/docs](https://registry.terraform.io/providers/Telmate/proxmox/latest/docs)

## Setup user/permissions in Proxmox:
Create a role for terraform user:
```
pveum role add TerraformProv -privs "Datastore.AllocateSpace Datastore.AllocateTemplate Datastore.Audit Pool.Allocate Sys.Audit Sys.Console Sys.Modify VM.Allocate VM.Audit VM.Clone VM.Config.CDROM VM.Config.Cloudinit VM.Config.CPU VM.Config.Disk VM.Config.HWType VM.Config.Memory VM.Config.Network VM.Config.Options VM.Migrate VM.Monitor VM.PowerMgmt SDN.Use"
```

Create terraform user:
```
pveum user add terraform-prov@pve --password <password>
pveum user add terraform-prov@pve
```

Assign terraform role to terraform user - not necessary if using token
```
pveum acl modify / --users terraform-prov@pve --roles TerraformProv
```

If additional permissions are needed:
```
pveum role modify TerraformProv -privs "Datastore.AllocateSpace Datastore.AllocateTemplate Datastore.Audit Pool.Allocate Sys.Audit Sys.Console Sys.Modify VM.Allocate VM.Audit VM.Clone VM.Config.CDROM VM.Config.Cloudinit VM.Config.CPU VM.Config.Disk VM.Config.HWType VM.Config.Memory VM.Config.Network VM.Config.Options VM.Migrate VM.Monitor VM.PowerMgmt SDN.Use"
```

Create a token for the user and store the token id/secret in `terraform.tfvars`
```
pveum user token add terraform-prov@pve terraform-token

proxmox_pve2_token_id = "terraform-prov@pve!terraform-token"
proxmox_pve2_token_secret = "xx-xx-xx-xx"
```

Apply terraform role to the token:
```
pveum acl modify / --tokens 'terraform-prov@pve!terraform-token' --roles TerraformProv
```

## main.tf
Define the provider:
```
terraform {
  required_providers {
    proxmox = {
      source = "telmate/proxmox"
      version = "3.0.1-rc4"
    }
  }
}
```

Provider proxmox:
```
provider "proxmox" {
  # connection
  pm_api_url = "https://10.10.10.20:8006/api2/json"
  pm_api_token_id = var.proxmox_pve2_token_id
  pm_api_token_secret = var.proxmox_pve2_token_secret
  pm_tls_insecure = true # if self-signed certificate

  # logging
  pm_log_enable = true
  pm_log_file   = "terraform-plugin-proxmox.log"
  pm_debug      = true
  pm_log_levels = {
    _default    = "debug"
    _capturelog = ""
  }
}

# these are defined in terraform.tfvars
variable "proxmox_pve2_token_secret" { }
variable "proxmox_pve2_token_id" { }
```

Resource `proxmox_vm_qemu`
```
resource "proxmox_vm_qemu" "test_vm_creation" {

  name        = "vm-terr01"
  target_node = "pve2"

#  clone = "ub-2404-cldinit-tmp"
  ipconfig0 = "gw=10.10.0.1,ip=10.10.200.59/16"

  cores    = 3
  sockets  = 1
  cpu      = "host"
  memory   = 2560

  network {
    model = "virtio"
    bridge = "vmbr0"
  }

  disk {
    slot         = "virtio0"
    type         = "disk"
    storage      = "local-zfs"
    size         = "4G"
    backup       = true
  }

}
```

### 1. 