---

copyright:
  years: 2017, 2020
lastupdated: "2020-09-23"

keywords: terraform provider plugin, terraform gen 1 compute, terraform gen 1 resources, terraform  generation 1 compute terraform

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
{:step: data-tutorial-type='step'}


# VPC infrastructure data sources (Gen 1 compute)
{: #vpc-gen1-data-sources}

Review the data sources that you can use to retrieve information about your [{{site.data.keyword.vpc_full}} (Gen 1 compute) infrastructure](/docs/vpc?topic=vpc-about-vpc). All data sources are imported as read-only information. You can reference the output parameters for each data source by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 
{: shortdesc}

Before you start working with your data source, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}



## ibm_is_floating_ip
{: #floating-ip-g1-ds}

Retrieve the information about VPC floating IP. 
{: shortdesc}

### Sample Terraform code
{: #floating-ip-g1ds-sample}

The following example retrieves information about the VPC floating IP.
{: shortdesc}

```

 data "ibm_is_floating_ip" "test" {
      name   = "test-fp"
 }

```

### Input parameters
{: #floating-ip-g1dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required / optional|Description|
|----|-----------|--------|----------------------|
|`name`|String|Required|The name of the floating IP.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #floating-ip-g1dsoutput}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`id`|String|The unique identifier of the floating IP.|
|`address`|String|The floating IP address that is created.|
|`status`|String|Provisioning status of the floating IP address.|
|`tags`|String|The tags associated with VPC.|
|`target`|String|The ID of the network interface used to allocate the floating IP address.|
|`zone`|String|The zone name where to create the floating IP address.|
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_is_image`
{: #image}

Retrieve the details of an image that you can use in your {{site.data.keyword.vsi_is_short}}. The image represents the version of the operation system that is installed in your {{site.data.keyword.vsi_is_short}} instance.
{: shortdesc}

### Sample Terraform code
{: #image-sample}

The following example shows how you can retrieve information about the `centos-7.x-amd64` image. 
{: shortdesc}

```
data "ibm_is_image" "ds_image" {
    name = "centos-7.x-amd64"
}
```

### Input parameters
{: #image-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
| `name` | String | Required | The name of the image. To list available names, run the `ibmcloud is images` command. |
| `visibility` | String | Optional | The visibility of the image. Images that are marked as `public` are provided by IBM. `Private` images are custom images that you uploaded to {{site.data.keyword.cloud_notm}}. |


### Output parameters
{: #image-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `architecture` | String | The CPU architecture that this image is designed for. | 
| `crn` | String | The CRN of the image. | 
| `id` | String | The unique identifier for the image. | 
| `os` | String | The name and version of the operating system that is installed with the image. | 
| `status` | String | The status of this image. |






## `ibm_is_images`
{: #images}

Retrieve a list of all images that you can use in your {{site.data.keyword.vsi_is_short}} instance. The image represents the version of the operation system that is installed in your {{site.data.keyword.vsi_is_short}} instance.
{: shortdesc}

### Sample Terraform code
{: #images-sample}

The following example shows how you can retrieve a list of supported images for a {{site.data.keyword.vsi_is_short}} instance.
{: shortdesc}

```
data "ibm_is_images" "ds_images" {
}
```

### Output parameters
{: #images-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `images` | Array | A list of all images that you can use in your {{site.data.keyword.vsi_is_short}} instance. |
| `images.architecture` | String | The CPU architecture that this image is designed for.  | 
| `images.crn` | String | The CRN of the image. |
| `images.id` | String | The unique identifier of the image. |
| `images.name` | String | The name of the image. |
| `images.os` | String| The name and version of the operating system that is installed with the image. |
| `images.status` | String | The status of this image. | 
| `images.visibility` | String | The visibility of the image. Images that are marked as `public` are provided by IBM. `Private` images are custom images that you uploaded to {{site.data.keyword.cloud_notm}}. |







## `ibm_is_instance`
{: #instance}

Retrieve the details for a Gen 1 {{site.data.keyword.vsi_is_short}} instance.
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

| Input parameter | Data type | Required / optional | Description |
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
|`network_interfaces`|Object|A list of more network interfaces that the instance uses.|
|`network_interfaces.id`|String|The ID of the more network interface.|
|`network_interfaces.name`|String|The name of the more network interface.|
|`network_interfaces.subnet`|String|The ID of the subnet that is used in the more network interface.|
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

Retrieve the details for all Gen 1 {{site.data.keyword.vsi_is_short}} instances in your account.
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
|`instances.network_interfaces`|Object|A list of more network interfaces that the instance uses.|
|`instances.network_interfaces.id`|String|The ID of the more network interface.|
|`instances.network_interfaces.name`|String|The name of the more network interface.|
|`instances.network_interfaces.subnet`|String|The ID of the subnet that is used in the more network interface.|
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
{: #instance-profile}

Retrieve the details for a profile that you can use in your {{site.data.keyword.vsi_is_short}} instance.
{: shortdesc}

### Sample Terraform code
{: #instance-profile-sample}

The following example shows how you can retrieve information about the `bc1-2x8` profile. 
{: shortdesc}

```
data "ibm_is_instance_profile" "profile" {
	name = "bc1-2x8"
}
```

### Input parameters
{: #instance-profile-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
| `name` | String | Required | The name of the profile. To list available profiles, run the `ibmcloud is instance-profiles` command.|

### Output parameters
{: #instance-profile-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `family` | String | The family that the profile belongs to. The family indicates what workloads are best suited for this type of profile. For more information, see [Profiles](/docs/vpc?topic=vpc-profiles).  |







## `ibm_is_instance_profiles`
{: #instance-profiles}

Retrieve a list of all profiles that you can use in your {{site.data.keyword.vsi_is_short}} instance. The profile represents the compute resources that are available to your {{site.data.keyword.vsi_is_short}} instance, such as CPU and memory.
{: shortdesc}

### Sample Terraform code
{: #instance-profiles-sample}

The following example shows how you can retrieve a list of supported {{site.data.keyword.vsi_is_short}} profiles. 
{: shortdesc}

```
data "ibm_is_instance_profiles" "ds_instance_profiles" {
}
```

### Input parameters
{: #instance-profiles-input}

No input parameters are required for this resource. 

### Output parameters
{: #instance-profiles-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `profiles` | Array | A list of profiles that you can use in a {{site.data.keyword.vsi_is_short}} instance. |
| `profiles.name` | String | The name of the profile. |
| `profiles.family` | String |The family that the profile belongs to. The family indicates what workloads are best suited for this type of profile. For more information, see [Profiles](/docs/vpc?topic=vpc-profiles).|






## `ibm_is_public_gateway`
{: #public-gwy}

Retrieve the details for a public gateway data source.
{: shortdesc}

### Sample Terraform code
{: #public-gwy-sample}

The following example shows how you can retrieve information about the `us-south` region. 
{: shortdesc}

```
resource "ibm_is_vpc" "testacc_vpc" {
  name = "test"
}

resource "ibm_is_public_gateway" "testacc_gateway" {
  name = "test-gateway"
  vpc  = ibm_is_vpc.testacc_vpc.id
  zone = "us-south-1"
}

data "ibm_is_public_gateway" "testacc_dspgw"{
  name = ibm_is_public_gateway.testacc_public_gateway.name
}
```

### Input parameters
{: #public-gwy-dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
| `name` | String | Required | The name of the gateway. | 
| `resource_group` | String | Optional | The resource group ID of the public gateway. **Note** This parameter is supported only for VPC Gen 2 infrastructure. |

### Output parameters
{: #public-gwy-dsoutput}

Review the output parameters that you can access after you retrieve your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `is` | String | The ID of the public gateway. | 
| `status` | String | The status of the gateway. |
| `vpc` | String | The VPC ID of the gateway. |
| `zone` | String | The public gateway zone name. |
| `floating_ip` | String | Lists the nested block describes the floating IP of the gateway.  with **id** and **address**. |
| `floating_ip.id` | String | The ID of the floating IP that is bound to the public gateway. |
| `floating_ip.address` | String | The IP address of the floating IP that is bound to the public gateway. |






## `ibm_is_lb`
{: #ibm-is-lb}

Import the details of an existing IBM VPC Load Balancer as a read only data source.
{: shortdesc}

### Sample Terraform code
{: #ibm-is-lb-sample}

The following example shows how you can retrieve information about the `us-south` region. 
{: shortdesc}

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

resource "ibm_is_lb" "testacc_lb" {
  name    = "testlb"
  subnets = [ibm_is_subnet.testacc_subnet.id]
}

data "ibm_is_lb" "ds_lb" {
  name = ibm_is_lb.testacc_lb.name
}
```

### Input parameters
{: #ibm-is-lb-dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
| `name` | String | Required | The name of the load balancer. | 


### Output parameters
{: #ibm-is-lb-dsoutput}

Review the output parameters that you can access after you retrieve your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `hostname` | String | Fully qualified domain name assigned to this load balancer.|
| `id` | String | The ID of the load balancer. |
| `listeners` | String | The ID of the listeners attached to this load balancer. |
| `operating_status` | String | The operating status of this load balancer.|
| `pools` | String | List all the Pools attached to this load balancer. |
| `pools.algorithm` | String | The load balancing algorithm. |
| `pools.created_at` | String |The date and time pool was created. |
| `pools.href` | String | The pool's canonical URL. |
| `pools.id` | String | The unique identifier for this load balancer pool. |
| `pools.name` | String | The user-defined name for this load balancer pool. |
| `pools.protocol` | String | The protocol used for this load balancer pool. |
| `pools.provisioning_status` | String | The provisioning status of this pool. |
| `pools.health_monitor` | String | The health monitor of this pool. |
| `pools.health_monitor.delay` | String | The health check interval in seconds. Interval must be greater than timeout value. |
| `pools.health_monitor.max_retries` | String | The health check max retries. |
| `pools.health_monitor.timeout` | String | The health check timeout in seconds. |
| `pools.health_monitor.type` | String | The protocol type of this load balancer pool health monitor. |
| `pools.health_monitor.url_path` | String | The health monitor of this pool. |
| `pools.health_monitor` | String | The health check URL. This is applicable only to HTTP type of health monitor. |
| `pools.instance_group` | String | The instance group that is managing this pool. |
| `pools.instance_group.crn` | String | The CRN for this instance group. |
| `pools.instance_group.href` | String |  The URL for this instance group. |
| `pools.instance_group.id` | String | The unique identifier for this instance group. |
| `pools.instance_group.name` | String | The user-defined name for this instance group. |
| `pools.members` | String | The backend server members of the pool. |
| `pools.members.href` | String | The canonical URL of the member. |
| `pools.members.id` | String | The unique identifier for this load balancer pool member. |
| `pools.session_persistence` | String | The session persistence of this pool. |
| `pools.session_persistence.type` | String | The session persistence type. |
| `public_ips` | String | The public IP addresses assigned to this load balancer.|
| `private_ips` | String | The private IP addresses assigned to this load balancer.|
| `resource_group` | String | The resource group where the load balancer is created. |
| `subnets` | String | The ID of the subnets to provision this load balancer. |
| `status` | String | The status of load balancer.|
| `tags` | String | The tags associated with the load balancer. |
| `type` | String | The type of the load balancer. |






## `ibm_is_region`
{: #region}

Retrieve the details for an {{site.data.keyword.vpc_full}} regions. 
{: shortdesc}

### Sample Terraform code
{: #region-sample}

The following example shows how you can retrieve information about the `us-south` region. 
{: shortdesc}

```
data "ibm_is_region" "ds_region" {
    name = "us-south"
}
```

### Input parameters
{: #region-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
| `name` | String | Required | The name of the region. To list available regions, run the `ibmcloud is regions` command. | 

### Output parameters
{: #region-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `status` | String | The status of the region. | 
| `endpoint` | String | The API endpoint of the region. |







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

|Name|Data type|Required / optional|Description|
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
|`rules`|List of objects|A nested block describes the rules of the attributes. |
|`rules.rule_id`| String|ID of the rule.  |
|`rules.direction`|String|Direction of traffic to enforce, either inbound or outbound. |
|`rules.ip_version`|String|IP version: IPv4 or IPv6.  |
|`rules.protocol`|String|The type of the protocol `all`, `icmp`, `tcp`, `udp`.   |
|`rules.type`|String|The ICMP traffic type to allow.  |
|`rules.code`|String|The ICMP traffic code to allow.  |
|`rules.port_max`|Integer|The TCP/UDP port range that includes maximum bound.  |
|`rules.port_min`|Integer|The TCP/UDP port range that includes minimum bound. |
|`protocol`|Integer| The type of the protocol rules `ALL`, `ICMP`, `UDP`, `TCP` |
{: caption="Table 1. Available output parameters" caption-side="top"}






## `ibm_is_ssh_key`
{: #ssh-key}

Retrieve the details for an SSH key that you uploaded in {{site.data.keyword.cloud_notm}}. SSH keys are used to access {{site.data.keyword.vsi_is_short}} instances. 
{: shortdesc}

### Sample Terraform code
{: #ssh-key-sample}

The following example shows how you can retrieve details about an SSH key that is named `mysshkey`. 
{: shortdesc}

```
data "ibm_is_ssh_key" "ds_key" {
    name = "mysshkey"
}
```

### Input parameters
{: #ssh-key-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
| `name` | String | Required | The name of the SSH key that you uploaded to {{site.data.keyword.cloud_notm}}. | 


### Output parameters
{: #ssh-key-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `fingerprint` | String | The SHA256 fingerprint of the public key. | 
| `id` | String | The ID of the SSH key. | 
| `length` | String | The length of the SSH key. | 
| `type` | String | The crypto system that is used by the SSH key. | 






## `ibm_is_subnet`
{: #subnet}

Retrieve details for an {{site.data.keyword.vpc_full}} subnet. 
{: shortdesc}

### Sample Terraform code
{: #subnet-sample}

The following example shows how you can retrieve information about a VPC subnet that you create with the `ibm_is_subnet` resource. 
{: shortdesc} 

```
resource "ibm_is_vpc" "myvpc_resource" {
	name = "myvpc"
}
resource "ibm_is_subnet" "mysubnet_resource" {
	name = "mysubnet"
	vpc = ibm_is_vpc.myvpc_resource.id
	zone = "us-south-1"
	ipv4_cidr_block = "192.168.0.0/1"
}
data "ibm_is_subnet" "ds_subnet" {
	identifier = ibm_is_subnet.mysubnet_resource.id
}
```
The following example retrieves the subnet information by the subnet ID.

```
resource "ibm_is_vpc" "testacc_vpc" {
  name = "test"
}

resource "ibm_is_subnet" "testacc_subnet" {
  name            = "test-subnet"
  vpc             = ibm_is_vpc.testacc_vpc.id
  zone            = "us-south-1"
  ipv4_cidr_block = "192.168.0.0/1"
}

data "ibm_is_subnet" "ds_subnet" {
  identifier = ibm_is_subnet.testacc_subnet.id
}
```

### Input parameters
{: #subnet-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
| `identifier` | String | Optional | The ID of the subnet. To list subnets in your account, run the `ibmcloud is subnets` command. | 
|`name`|String|Optional|The name of the subnet.|

### Output parameters
{: #subnet-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `available_ipv4_address_count` | Integer | The total number of available IPv4 addresses.
| `ipv4_cidr_block` | String | The IPv4 CIDR of the subnet. |
| `ipv6_cidr_block` | String | The IPv6 CIDR of the subnet. |
| `ip_version` | String | The IP Version. |
| `name` | String | The name of the subnet. |
| `network_acl` | String | The ID of the network access control list (ACL) that is set up for the subnet. |
| `public_gateway` | String | The ID of the public gateway that is attached to the subnet. |
| `status` | String | The status of the subnet. |
| `total_ipv4_address_count` | Integer | The total number of IPv4 addresses. |
| `vpc` | String | The ID of the VPC that the subnet is attached to. |
| `zone` | String | The name of the zone where the subnet is provisioned. |
|`resource_group`|String|The subnet resource group.|







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

Retrieve information about an {{site.data.keyword.vpc_full}}. 
{: shortdesc}

### Sample Terraform code
{: #vpc-sample}

The following example shows how you can retrieve information about a VPC that is named `myvpc`. 
{: shortdesc}

```
resource "ibm_is_vpc" "myvpc_resource" {
    name = "myvpc"
}

data "ibm_is_vpc" "ds_vpc" {
    name = "myvpc"
}

```

### Input parameters
{: vpc-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
| `name` | String | Required | The name of the VPC. To list available VPCs, run the `ibmcloud is vpcs` command. |

### Output parameters
{: #vpc-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `classic_access` | String | Indicates whether this VPC is set up with a connection to classic {{site.data.keyword.cloud_notm}} infrastructure. | 
|`crn`|String|The CRN of the VPC.|
| `cse_source_addresses`|List of Cloud Service Endpoints|A list of the cloud service endpoints that are associated with your VPC, including their source IP address and zone.|
|`cse_source_addresses.address`|String|The IP address of the cloud service endpoint.|
|`cse_source_addresses.zone_name`|String|The zone where the cloud service endpoint is located.|
| `default_network_acl` | String | The ID of the default network access control list (ACL) that was set up for the VPC. | 
| `resource_group` | String | The resource group ID where the VPC was created. |
|`subnets`|List of subnets|A list of subnets that are attached to a VPC.|
|`subnets.name`|String|The name of the subnet.|
|`subnets.id`|String|The ID of the subnet.|
|`subnets.zone`|String|The zone that the subnet belongs to.|
|`subnets.status`|String|The status of the subnet.|
|`subnets.total_ipv4_address_count`|Integer|The total number of IPv4 addresses in the subnet.|
|`subnets.available_ipv4_address_count`|Integer|The number of IPv4 addresses in the subnet that are available for you to be used.|
| `status` | String | The status of the VPC. |
| `tags` | Array | A list of tags that are associated with the VPC. |








## `ibm_is_zone`
{: #zone}

Retrieve details about a supported {{site.data.keyword.vpc_full}} zone.
{: shortdesc}


### Sample Terraform code
{: #zone-sample}

The following example shows how you can retrieve details about the `us-south-1` zone. 
{: shortdesc}

```

data "ibm_is_zone" "ds_zone" {
    name = "us-south-1"
    region = "us-south"
}

```

### Input parameters
{: #zone-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
| `name` | String | Required | The name of the zone. |
| `region` | String | Required | The name of the region where the zone is located. |

### Output parameters
{: #zone-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `status` | String | The status of zone. |







## `ibm_is_zones`
{: #zones}

Retrieve a list of supported {{site.data.keyword.vpc_full}} zones in an {{site.data.keyword.cloud_notm}} region. 
{: shortdesc}

### Sample Terraform code
{: #zones-sample}

The following example shows how you can list all zones in the `us-south` region. 
{: shortdesc}

```

data "ibm_is_zones" "ds_zones" {
    region = "us-south"
}

```

### Input parameters
{: #zones-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
| `region` | String | Required | The name of the region. |
| `status` | String | Optional | The status of the zone that you want to filter for.  |

### Output parameters
{: #zones-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `zones` | Array | The list of zones in an {{site.data.keyword.cloud_notm}} region.  For example, `us-south-1`,`us-south-2`.|
