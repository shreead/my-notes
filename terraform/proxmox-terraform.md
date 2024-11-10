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


