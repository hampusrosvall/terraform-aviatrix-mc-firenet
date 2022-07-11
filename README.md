# terraform-aviatrix-mc-firenet

### Description
Aviatrix Terraform module for firenet deployment in multiple clouds, to be used in conjunction with [mc-transit module](https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet).

### Compatibility
Module version | Terraform version | Controller version | Terraform provider version | [mc-transit module](https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-transit) version
:--- | :--- | :--- | :--- | :---
v1.1.2 | >=1.1.0 | ~> 6.7.1186 | ~> 2.22.0 | ~> v2.1.5

Check [release notes](https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/master/RELEASE_NOTES.md) for more details.
Check [Compatibility list](https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/master/COMPATIBILITY.md) for older versions.

### Usage Example
```
module "mc_transit" {
  source  = "terraform-aviatrix-modules/mc-transit/aviatrix"
  version = "v2.1.5"

  cloud                  = "AWS"
  cidr                   = "10.1.0.0/23"
  region                 = "eu-central-1"
  account                = "AWS"
  enable_transit_firenet = true
}

module "firenet_1" {
  source  = "terraform-aviatrix-modules/mc-firenet/aviatrix"
  version = "v1.1.2"

  transit_module = module.mc_transit
  firewall_image = "Palo Alto Networks VM-Series Next-Generation Firewall Bundle 1"
}
```

### Variables
The following variables are required:

key | value
:--- | :---
[firewall_image](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#firewall_image) | The firewall image to be used to deploy the NGFW's. Use "aviatrix" to deploy Aviatrix FQDN egress filtering GW's (AWS/Azure/GCP).
transit_module | Refer to the mc-transit module that built the transit. This module plugs directly into it's output to build firenet on top of it.

The following variables are optional:

<img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-spoke/blob/master/img/aws.png?raw=true" title="AWS"> = AWS, <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-spoke/blob/master/img/azure.png?raw=true" title="Azure"> = Azure, <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-spoke/blob/master/img/gcp.png?raw=true" title="GCP"> = GCP, <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-spoke/blob/master/img/oci.png?raw=true" title="OCI"> = OCI, <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-spoke/blob/master/img/alibaba.png?raw=true" title="Alibaba"> = Alibaba

Key | Supported_CSP's |  Default value | Description
:-- | --: | :-- | :--
associated | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | true | Associate firewalls with transit gateway.
attached | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | true | Attach firewall instances.
[bootstrap_bucket_name_1](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#bootstrap_bucket_name) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> | | Name of bootstrap bucket to pull firewall config from. (If bootstrap_bucket_name_2 is not set, this will used for all NGFW instances)
[bootstrap_bucket_name_2](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#bootstrap_bucket_name) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> | | Name of bootstrap bucket to pull firewall config from. (Only used if 2 or more FW instances are deployed, e.g. when ha_gw is true. Applies to "even" fw instances (2,4,6 etc))
[bootstrap_storage_name_1](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#bootstrap_storage_name) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> | null | Storagename to get bootstrap files from (PANW only). (If bootstrap_storage_name_2 is not set, this will used for all NGFW instances)
[bootstrap_storage_name_2](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#bootstrap_storage_name) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> | null | Storagename to get bootstrap files from (PANW only) (Only used when HA FW instance is deployed)
custom_fw_names | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | [] | If set, the NGFW instances will be deployed with the names provided in this list. First half of the list for instances in az1, second half for az2.
[east_west_inspection_excluded_cidrs](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firenet#east_west_inspection_excluded_cidrs) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | | Network List Excluded From East-West Inspection.
egress_cidr | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> | | CIDR For Egress VPC for GCP Firenet. Only required when deploying in GCP and enable_transit_firenet is true.
[egress_enabled](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firenet#egress_enabled) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | false | Enable/disable internet egress via NGFW.
[egress_static_cidrs](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firenet#egress_static_cidrs) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | [] | List of egress static CIDRs. Egress is required to be enabled. Example: ["1.171.15.184/32", "1.171.15.185/32"].
[fail_close_enabled](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firenet#fail_close_enabled) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | false | Set to true to enable fail close
[file_share_folder_1](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#file_share_folder) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> | null | Name of the folder containing the bootstrap files (PANW only) (If file_share_folder_2 is not set, this will used for all NGFW instances)
[file_share_folder_2](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#file_share_folder) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> | null | Name of the folder containing the bootstrap files (PANW only) (Only used when HA FW instance is deployed)
[firewall_image_id](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#firewall_image_id) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> | | Firewall image ID. Applicable to AWS and Azure only. For AWS, please use AMI ID. For Azure, the format is “Publisher:Offer:Plan:Version”.
[firewall_image_version](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#firewall_image_version) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | | When not provided, latest available will be used.
fw_amount | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> | | The amount of NGFW instances to deploy. These will be deployed accross multiple AZ's. Amount must be even and only applies when transit is HA.
[iam_role_1](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#iam_role) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> | | IAM Role used to access bootstrap bucket. (If iam_role_2 is not set, this will used for all NGFW instances)
[iam_role_2](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#iam_role) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> | | IAM Role used to access bootstrap bucket. (Only used if 2 or more FW instances are deployed, e.g. when ha_gw is true. Applies to "even" fw instances (2,4,6 etc))
[inspection_enabled](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firenet#inspection_enabled) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | true | Enable/disable east/west + north/south inspection via NGFW.
[instance_size](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#firewall_size) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <br> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <br> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <br> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | c5.xlarge <br> Standard_D3_v2 <br> n1-standard-4 <br> VM.Standard2.4 | Size of the NGFW instances
[keep_alive_via_lan_interface_enabled](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firenet#keep_alive_via_lan_interface_enabled) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | False | Enable Keep Alive via Firewall LAN Interface.
mgmt_cidr | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> | | CIDR For Management VPC for GCP Firenet. Only required when deploying in GCP and enable_transit_firenet is true and deploying Palo Alto NGFW.
[password](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#password) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> | Aviatrix#1234 | Default initial password for firewall instances
[storage_access_key_1](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#storage_access_key) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> | null | Storage_access_key to access bootstrap storage (PANW only) (If storage_access_key_2 is not set, this will used for all NGFW instances)
[storage_access_key_2](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#storage_access_key) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> | null | Storage_access_key to access bootstrap storage (PANW only) (Only used when HA FW instance is deployed)
[tags](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#tags) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> | | Map of tags to assign to the firewall or FQDN egress gw's.
use_gwlb | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> | | Deploy firenet using the AWS GWLB.
[user_data_1](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#user_data) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | | Userdata to bootstrap FortiGate or Checkpoint Firewall.
[user_data_2](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#user_data) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/aws.png?raw=true" title="AWS"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/gcp.png?raw=true" title="GCP"> <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/oci.png?raw=true" title="OCI"> | | Userdata to bootstrap FortiGate or Checkpoint Firewall. If not set, user_data_1 will be used.
[username](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance#username) | <img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-mc-firenet/blob/main/img/azure.png?raw=true" title="Azure"> | fwadmin | Applicable to Azure or AzureGov deployment only. "admin" as a username is not accepted. (For Checkpoint it is always admin)

### Outputs
This module will return the following objects:

key | description
:--- | :---
[aviatrix_firenet](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firenet) | The created Aviatrix firenet object with all of it's attributes.
[aviatrix_firewall_instance](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance) | A list of the created firewall instances and their attributes.

### Common Errors

When using a firewall_image string that does not exist, a data lookup will fail and throw the error below. Make sure you are using a valid firewall_image. These can differ between clouds. Check the Aviatrix controller UI to see available firewall images.
```
│ Error: Invalid index
│ 
│   on variables.tf line 172:
│   (source code not available)
│ 
│ The given key does not identify an element in this collection value: the collection has no elements.
```
