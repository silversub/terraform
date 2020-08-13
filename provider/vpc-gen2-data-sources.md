---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-13"

keywords: terraform provider plugin, terraform vpc gen 2, terraform vpc, gen 2 compute terraform, terraform vpc subnet

subcollection: terraform

---

{:beta: .beta}
{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}


# VPC infrastructure data sources (Gen 2 compute) 
{: #vpc-gen2-data-sources}

Before you start working with your data source, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}



## `ibm_is_image`
{: #vpc-image}

Retrieve information about a virtual server image for Gen 2 compute. 
{: shortdesc}

### Sample Terraform code
{: #vpc-image-sample}

The following example retrieves information about the `centos-7.x-amd64` image. 
{: shortdesc}

```

data "ibm_is_image" "ds_image" {
    name = "centos-7.x-amd64"
}

```

### Input parameters
{: #vpc-image-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required/ optional|Description|
|----|-----------|--------|----------------------|
|`name`|String|Required|The name of the image.|
|`visibility`|String|Optional|The visibility of the image. Accepted values are `public` or `private`.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #vpc-image-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`id`|String|The unique identifier for this image.|
|`crn`|String|The CRN for this image.|
|`os`|String|The name of the operating system.|
|`status`|String|The status of this image.|
|`architecture`|String|The architecture for this image.|
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_is_images`
{: #vpc-images}

Retrieve information about Gen 2 virtual server images. 
{: shortdesc}

### Sample Terraform code
{: #vpc-images-sample}

```

data "ibm_is_images" "ds_images" {
}

```

### Input parameters
{ #vpc-images-input}

No input parameters are required for this data source.
{: shortdesc}

### Output parameters
{ #vpc-images-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`images`|List of objects|List of all supported images.  |
|`images.name`|String|The name for this image.  |
|`images.id`|String|The unique identifier for this image.  |
|`images.crn`|String|The CRN for this image.  |
|`images.os`|String|The name of the operating system.  |
|`images.status`|String|The status of this image.  |
|`images.architecture`|String|The architecture for this image.  |
|`images.visibility`|String|The visibility of the image public or private.|
{: caption="Table 1. Available output parameters" caption-side="top"}


## `ibm_is_instance`
{: #instance}

Retrieve the details for a Gen 2 {{site.data.keyword.vsi_is_short}} instance.
{: shortdesc}

### Sample Terraform code
{: #instance-sample}

```
resource "ibm_is_vpc" "testacc_vpc" {
  name = "testvpc"
}

resource "ibm_is_subnet" "testacc_subnet" {
  name            = "testsubnet"
  vpc             = ibm_is_vpc.testacc_vpc.id
  zone            = "us-south-1"
  ipv4_cidr_block = "10.240.0.0/24"
}

resource "ibm_is_ssh_key" "testacc_sshkey" {
  name       = "testssh"
  public_key = file("~/.ssh/id_rsa.pub")
}

resource "ibm_is_instance" "testacc_instance" {
  name    = "testinstance"
  image   = "a7a0626c-f97e-4180-afbe-0331ec62f32a"
  profile = "bc1-2x8"

  primary_network_interface {
    subnet = ibm_is_subnet.testacc_subnet.id
  }

  network_interfaces {
    name   = "eth1"
    subnet = ibm_is_subnet.testacc_subnet.id
  }

  vpc  = ibm_is_vpc.testacc_vpc.id
  zone = "us-south-1"
  keys = [ibm_is_ssh_key.testacc_sshkey.id]
}

data "ibm_is_instance" "ds_instance" {
  name        = "${ibm_is_instance.testacc_instance.name}"
  private_key = file("~/.ssh/id_rsa")
  passphrase  = ""
}
```
{: codeblock}

### Input parameters
{: #instance-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
|`name`|String|Required|The name of the {{site.data.keyword.vsi_is_short}} instance that you want to retrieve. |
|`private_key`|String|Optional|The private key of an SSH key that you want to add to your {{site.data.keyword.vsi_is_short}} instance during creation in PEM format. SSH keys are used by virtual servers to identify a user or device through public-key cryptography. For more information about how to create SSH keys and upload them to {{site.data.keyword.cloud_notm}}, see [Locating or generating your SSH key](/docs/vpc?topic=vpc-ssh-keys#locating-ssh-keys). The SSH key is used to decrypt the default administrator password in Windows if Windows is installed as the operating system on your {{site.data.keyword.vsi_is_short}} instance.|
|`passphrase`|String|Optional|The passphrase that you used when you created your SSH key. If you did not enter a passphrase when you created the SSH key, do not provide this input parameter.|

### Output parameters
{: #instance-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id`|String|The ID that was assigned to the {{site.data.keyword.vsi_is_short}} instance.|
|`memory`|Integer|The amount of memory that was allocated to the instance.|
|`status`|String|The status of the instance.|
|`image`|String|The ID of the virtual server image that is used in the instance.|
|`zone`|String|The zone where the instance was created.|
|`vpc`|String|The ID of the VPC that the instance belongs to.|
|`resource_group`|String|The name of the resource group where the instance was created.|
|`vcpu`|Object|A list of virtual CPUs that were allocated to the instance.|
|`vcpu.architecture`|String|The architecture of the virtual CPU. |
|`vcpu.count`|Integer|The number of virtual CPUs that are allocated to the instance.|
|`gpu`|Object|A list of graphics processing units that are allocated to the instance.|
|`gpu.cores`|Integer|The number of cores that are assigned to the GPU.|
|`gpu.count`|Integer|The number of GPUs that are allocated to the instance.|
|`gpu.manufacture`|String|The manufacturer of the GPU.|
|`gpu.memory`|Integer|The amount of memory that was allocated to the GPU.|
|`gpu.model`|String|The model of the GPU.| 
|`primary_network_interface`|Object|A list of primary network interfaces that were created for the instance.| 
|`primary_network_interface.id`|String|The ID of the primary network interface.|
|`primary_network_interface.name`|String|The name of the primary network interface.|
|`primary_network_interface.subnet`|String|The ID of the subnet that is used in the primary network interface.|
|`primary_network_interface.security_groups`|List|A list of security groups that were created for the interface.|
|`primary_network_interface.primary_ipv4_address`|String|The IPv4 address range that the subnet uses.|
|`network_interfaces`|Object|A list of additional network interfaces that the instance uses.|
|`network_interfaces.id`|String|The ID of the additional network interface.|
|`network_interfaces.name`|String|The name of the additional network interface.|
|`network_interfaces.subnet`|String|The ID of the subnet that is used in the additional network interface.|
|`network_interfaces.security_groups`|List|A list of security groups that were created for the interface.|
|`network_interfaces.primary_ipv4_address`|String|The IPv4 address range that the subnet uses.|
|`boot_volume`|Object|A list of boot volumes that were created for the instance.|
|`boot_volume.id`|String|The ID of the boot volume attachment.|
|`boot_volume.name`|String|The name of the boot volume.|
|`boot_volume.device`|String|The name of the device that is associated with the boot volume.|
|`boot_volume.volume_id`|String|The ID of the volume that is associated with the boot volume attachment.|
|`boot_volume.volume_crn`|String|The CRN of the volume that is associated with the boot volume attachment.|
|`volume_attachments`|Object|A list of volume attachments that were created for the instance.| 
|`volume_attachments.id`|String|The ID of the volume attachment.|
|`volume_attachments.name`|String|The name of the volume attachment.|
|`volume_attachments.volume_id`|String|The ID of the volume that is associated with the volume attachment.|
|`volume_attachments.volume_name`|String|The name of the volume that is associated with the volume attachment.|
|`volume_attachments.crn`|String|The CRN of the volume that is associated with the volume attachment.|
|`resource_controller_url`|String|The URL of the {{site.data.keyword.cloud_notm}} dashboard that you can use to see details for your instance.| 
|`password`|String|The password that you can use to access your instance.|
|`keys`|Object|A list of SSH keys that were added to the instance during creation.|
|`keys.id`|String|The ID of the SSH key.|
|`keys.name`|String|The name of the SSH key that you entered when you uploaded the key to {{site.data.keyword.cloud_notm}}.|

## `ibm_is_instances`
{: #instances}

Retrieve the details for all Gen 2 {{site.data.keyword.vsi_is_short}} instances in your account.
{: shortdesc}

### Sample Terraform code
{: #instances-sample}

```
data "ibm_is_instances" "ds_instances" {
}
```
{: codeblock}

### Input parameters
{: #instances-input}

This data source does not support any input parameters.
{: shortdesc}

### Output parameters
{: #instances-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`instances`|Object|A list of {{site.data.keyword.vsi_is_short}} instances that exist in your account.|
|`instances.id`|String|The ID that was assigned to the {{site.data.keyword.vsi_is_short}} instance.|
|`instances.memory`|Integer|The amount of memory that was allocated to the instance.|
|`instances.status`|String|The status of the instance.|
|`instances.image`|String|The ID of the virtual server image that is used in the instance.|
|`instances.zone`|String|The zone where the instance was created.|
|`instances.vpc`|String|The ID of the VPC that the instance belongs to.|
|`instances.resource_group`|String|The name of the resource group where the instance was created.|
|`instances.vcpu`|Object|A list of virtual CPUs that were allocated to the instance.|
|`instances.vcpu.architecture`|String|The architecture of the virtual CPU. |
|`instances.vcpu.count`|Integer|The number of virtual CPUs that are allocated to the instance.|
|`instances.primary_network_interface`|Object|A list of primary network interfaces that were created for the instance.| 
|`instances.primary_network_interface.id`|String|The ID of the primary network interface.|
|`instances.primary_network_interface.name`|String|The name of the primary network interface.|
|`instances.primary_network_interface.subnet`|String|The ID of the subnet that is used in the primary network interface.|
|`instances.primary_network_interface.security_groups`|List|A list of security groups that were created for the interface.|
|`instances.primary_network_interface.primary_ipv4_address`|String|The IPv4 address range that the subnet uses.|
|`instances.network_interfaces`|Object|A list of additional network interfaces that the instance uses.|
|`instances.network_interfaces.id`|String|The ID of the additional network interface.|
|`instances.network_interfaces.name`|String|The name of the additional network interface.|
|`instances.network_interfaces.subnet`|String|The ID of the subnet that is used in the additional network interface.|
|`instances.network_interfaces.security_groups`|List|A list of security groups that were created for the interface.|
|`instances.network_interfaces.primary_ipv4_address`|String|The IPv4 address range that the subnet uses.|
|`instances.boot_volume`|Object|A list of boot volumes that were created for the instance.|
|`instances.boot_volume.id`|String|The ID of the boot volume attachment.|
|`instances.boot_volume.name`|String|The name of the boot volume.|
|`instances.boot_volume.device`|String|The name of the device that is associated with the boot volume.|
|`instances.boot_volume.volume_id`|String|The ID of the volume that is associated with the boot volume attachment.|
|`instances.boot_volume.volume_crn`|String|The CRN of the volume that is associated with the boot volume attachment.|
|`instances.volume_attachments`|Object|A list of volume attachments that were created for the instance.| 
|`instances.volume_attachments.id`|String|The ID of the volume attachment.|
|`instances.volume_attachments.name`|String|The name of the volume attachment.|
|`instances.volume_attachments.volume_id`|String|The ID of the volume that is associated with the volume attachment.|
|`instances.volume_attachments.volume_name`|String|The name of the volume that is associated with the volume attachment.|
|`instances.volume_attachments.crn`|String|The CRN of the volume that is associated with the volume attachment.|


## `ibm_is_instance_profile`
{: #vpc-instance-profile}

Retrieve information about a virtual server instance profile. 
{: shortdesc}

### Sample Terraform code
{: #vpc-instance-profile-sample}

The following example retrieves information about the `bx2-2x8` instance profile. 
{: shortdesc}

```

data "ibm_is_instance_profile" "profile" {
	name = "bx2-2x8"
}

```

### Input parameters
{: #vpc-instance-profile-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required/ optional|Description|
|----|-----------|--------|----------------------|
|`name`|String|Required|The name for this virtual server instance profile.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #vpc-instance-profile-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`family`|String|The family of the virtual server instance profile.|
{: caption="Table 1. Available output parameters" caption-side="top"}


## `ibm_is_instance_profiles`
{: #vpc-instance-profiles}

Retrieve information about supported virtual server instance profiles. 
{: shortdesc}

### Sample Terraform code
{: #vpc-instance-profiles-sample}

```

data "ibm_is_instance_profiles" "ds_instance_profiles" {
}

```


### Input parameters
{: #vpc-instance-profiles-input}

No input parameters are required for this data source.
{: shortdesc}

### Output parameters
{: #vpc-instance-profiles-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`profiles`|List of objects|List of all server instance profiles in the region.  |
|`profiles.name`|String|The name for this virtual server instance profile.  |
|`profiles.family`|String|The family of the virtual server instance profile.|
{: caption="Table 1. Available output parameters" caption-side="top"}


## `ibm_is_region`
{: #vpc-region}

Retrieve information about a VPC Gen 2 Compute region. 
{: shortdesc}

### Sample Terraform code
{: #vpc-region-sample}

```

data "ibm_is_region" "ds_region" {
    name = "us-south"
}

```

### Input parameters
{: #vpc-region-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required/ optional|Description|
|----|-----------|--------|----------------------|
|`name`|String|Required|The name of the region.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #vpc-region-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`status`|String|The status of the region.|
|`endpoint`|String|The endpoint of the region.|
{: caption="Table 1. Available output parameters" caption-side="top"}






## `ibm_is_security_group`
{: #sec-group-datasource}

Import the details of a security group as a read-only data source. You can reference the output parameters for each data source by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 
{: shortdesc}


### Sample Terraform code
{: #sec-group-sample}

The following example allows to create a different types of protocol rules `ALL`, `ICMP`, `UDP`, `TCP` and read the security group.
{: shortdesc}

```
resource "ibm_is_vpc" "testacc_vpc" {
  name = "test"
}

resource "ibm_is_security_group" "testacc_security_group" {
  name = "test"
  vpc  = ibm_is_vpc.testacc_vpc.id
}

resource "ibm_is_security_group_rule" "testacc_security_group_rule_all" {
  group     = ibm_is_security_group.testacc_security_group.id
  direction = "inbound"
  remote    = "127.0.0.1"
}

resource "ibm_is_security_group_rule" "testacc_security_group_rule_icmp" {
  group     = ibm_is_security_group.testacc_security_group.id
  direction = "inbound"
  remote    = "127.0.0.1"
  icmp {
    code = 20
    type = 30
  }
}

resource "ibm_is_security_group_rule" "testacc_security_group_rule_udp" {
  group     = ibm_is_security_group.testacc_security_group.id
  direction = "inbound"
  remote    = "127.0.0.1"
  udp {
    port_min = 805
    port_max = 807
  }
}

resource "ibm_is_security_group_rule" "testacc_security_group_rule_tcp" {
  group     = ibm_is_security_group.testacc_security_group.id
  direction = "egress"
  remote    = "127.0.0.1"
  tcp {
    port_min = 8080
    port_max = 8080
  }
}

data "ibm_is_security_group" "sg1_rule" {
  name = ibm_is_security_group.testacc_security_group.name
}
```

### Input parameters
{: #sec-group-ds-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------| 
|`name`|String|Required|The name of the security group.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #sec-group-ds-output}

Review the output parameters that are exported. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the security group.|
|`rules`|List of objects|A nested block describing the rules of the attributes. |
|`rules.rule_id`| String|ID of the rule.  |
|`rules.direction`|String|Direction of traffic to enforce, either inbound or outbound. |
|`rules.ip_version`|String|IP version: IPv4 or IPv6.  |
|`rules.protocol`|String|The type of the protocol `all`, `icmp`, `tcp`, `udp`.   |
|`rules.type`|String|The ICMP traffic type to allow.  |
|`rules.code`|String|The ICMP traffic code to allow.  |
|`rules.port_max`|Integer|The inclusive upper bound of TCP/UDP port range.  |
|`rules.port_min`|Integer|The inclusive lower bound of TCP/UDP port range. |
|`protocol`|Integer| The type of the protocol rules `ALL`, `ICMP`, `UDP`, `TCP` |
{: caption="Table 1. Available output parameters" caption-side="top"}






## `ibm_is_ssh_key`
{: #vpc-ssh-key}

Retrieve information about a VPC Gen 2 SSH key. 
{: shortdesc}

### Sample Terraform code
{: #vpc-ssh-key-sample}

```

data "ibm_is_ssh_key" "ds_key" {
    name = "test"
}

```

### Input parameters
{: #vpc-ssh-key-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required/ optional|Description|
|----|-----------|--------|----------------------|
|`name`|String|Required|The name of the SSH key.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #vpc-ssh-key-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`id`|String|The id of the SSH key.|
|`fingerprint`| String| The SHA256 fingerprint of the public key.|
|`length`|String|The length of the SSH key.|
|`type`|String|The cryptosystem that is used by this key.|
{: caption="Table 1. Available output parameters" caption-side="top"}


## `ibm_is_subnet`
{: #vpc-subnet}

Retrieve information about a VPC Gen 2 compute subnet. 
{: shortdesc}

### Sample Terraform code
{: #vpc-subnet-sample}

```
resource "ibm_is_vpc" "testacc_vpc" {
	name = "test"
}

resource "ibm_is_subnet" "testacc_subnet" {
	name = "test_subnet"
	vpc = ibm_is_vpc.testacc_vpc.id
	zone = "us-south-1"
	ipv4_cidr_block = "192.168.0.0/1"
}
data "ibm_is_subnet" "ds_subnet" {
	identifier = ibm_is_subnet.testacc_subnet.id
}

```

### Input parameters
{: #vpc-subnet-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required/ optional|Description|
|----|-----------|--------|----------------------|
|`identifier`|String|Required|The ID of the subnet.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #vpc-subnet-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`ipv4_cidr_block`| String|The IPv4 range of the subnet.|
|`ipv6_cidr_block`|String|The IPv6 range of the subnet.|
|`total_ipv4_address_count`|Integer|The total number of IPv4 addresses.|
|`ip_version`|String|The IP version.|
|`name`|String|The name of the subnet.|
|`network_acl`|String|The ID of the network ACL for the subnet.|
|`public_gateway`|String|The ID of the public gateway for the subnet.|
|`status`|String|The status of the subnet.|
|`vpc`|String|The ID of the VPC that the subnet belongs to.|
|`zone`|String|The subnet zone name.|
|`available_ipv4_address_count`|Integer|The total number of available IPv4 addresses.|
{: caption="Table 1. Available output parameters" caption-side="top"}


## `ibm_is_subnets`
{: #vpc-subnets}

Retrieve information about all existing VPC subnets in an IBM Cloud account. 
{: shortdesc}

### Sample Terraform code
{: #vpc-subnets-sample}

```
data "ibm_is_subnets" "ds_subnets" {
}
```
{: codeblock}

### Input parameters
{: #vpc-subnets-input}

This resource does not support any input parameters.

### Output parameters
{: #vpc-subnets-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`subnets`|List|A list of subnets in the IBM Cloud account.|
|`subnets.name`|String|The name of the subnet.|
|`subnets.id`|String|The ID of the subnet.|
|`subnets.ipv4_cidr_block`|String|The IPv4 CIDR block of this subnet.|
|`subnets.ipv6_cidr_block`|String|The IPv6 CIDR block of this subnet.|
|`subnets.status`|String|The status of the subnet.|
|`subnets.crn`|String|The CRN of the subnet.|
|`subnets.available_ipv4_address_count`|Integer|The number of IPv4 addresses that are available in the subnet.|
|`subnets.total_ipv4_address_count`|Integer|The total number of IPv4 addresses in the subnet.|
|`subnets.network_acl`|String|The access control list (ACL) that is attached to the subnet.|
|`subnets.public_gateway`|Boolean|If set to **true**, a public gateway is attached to the subnet. If set to **false**, no public gateway for this subnet exists.|
|`subnets.resource_group`|String|The resource group that the subnet belongs to.|
|`subnets.vpc`|String|The ID of the VPC that this subnet belongs to.|
|`subnets.zone`|String|The zone where the subnet was created.|

## `ibm_is_vpc`
{: #vpc}

Retrieve information about a Gen 2 compute VPC. 
{: shortdesc}

### Sample Terraform code
{: #vpc-sample}

```
resource "ibm_is_vpc" "testacc_vpc" {
    name = "test"
}

data "ibm_is_vpc" "ds_vpc" {
    name = "test"
}

```

### Input parameters
{: #vpc-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required/ optional|Description|
|----|-----------|--------|----------------------|
|`name`|String|Required|The name of the VPC.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #vpc-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`classic_access`|Boolean|Indicates whether this VPC is connected to Classic Infrastructure.|
|`crn`|String|The CRN of the VPC.|
| `cse_source_addresses`|List of Cloud Service Endpoints|A list of the cloud service endpoints that are associated with your VPC, including their source IP address and zone.|
|`cse_source_addresses.address`|String|The IP address of the cloud service endpoint.|
|`cse_source_addresses.zone_name`|String|The zone where the cloud service endpoint is located.|
|`default_network_acl`|String| The ID of the default network ACL.|
|`resource_group`|String|The resource group ID where the VPC created.|
|`subnets`|List of subnets|A list of subnets that are attached to a VPC.|
|`subnets.name`|String|The name of the subnet.|
|`subnets.id`|String|The ID of the subnet.|
|`subnets.zone`|String|The zone that the subnet belongs to.|
|`subnets.status`|String|The status of the subnet.|
|`subnets.total_ipv4_address_count`|Integer|The total number of IPv4 addresses in the subnet.|
|`subnets.available_ipv4_address_count`|Integer|The number of IPv4 addresses in the subnet that are available for you to be used.|
|`status`|String|The status of the VPC.|
|`tags`|Array|Tags associated with the instance.|
{: caption="Table 1. Available output parameters" caption-side="top"}


## `ibm_is_zone`
{: #vpc-zone}

Retrieve information about a Gen 2 compute zone. 
{: shortdesc}

### Sample Terraform code
{: #vpc-zone-sample}

```

data "ibm_is_zone" "ds_zone" {
    name = "us-south-1"
    region = "us-south"
}

```

### Input parameters
{: #vpc-zone-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required/ optional|Description|
|----|-----------|--------|----------------------|
|`name`|String|Required|The name of the zone.|
|`region`|String|Required| The name of the region.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #vpc-zone-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`status`|String|The status of the zone.|
{: caption="Table 1. Available output parameters" caption-side="top"}



## `ibm_is_zones`
{: #vpc-zones}

Retrieves information about Gen 2 compute zones. 
{: shortdesc}

### Sample Terraform code
{: #vpc-zones-sample}

```

data "ibm_is_zones" "ds_zones" {
    region = "us-south"
}

```

### Input parameters
{: #vpc-zones-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required/ optional|Description|
|----|-----------|--------|----------------------|
|`region`|String|Required|The name of the region.|
|`status`|String|Optional|Filter the list by status of zones.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #vpc-zones-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`zones`|String|The list of zones in an {{site.data.keyword.cloud_notm}} region.  For example, `us-south-1`,`us-south-2`.|
{: caption="Table 1. Available output parameters" caption-side="top"}





