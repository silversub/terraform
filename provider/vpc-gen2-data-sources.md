---

copyright:
  years: 2017, 2020
lastupdated: "2020-10-05"

keywords: terraform provider plugin, terraform gen 2, terraform gen 2 compute

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


# VPC infrastructure data sources (Gen 2 compute) 
{: #vpc-gen2-data-sources}

Before you start working with your data source, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}.
{: note}

## ibm_is_floating_ip
{: #floating-ip-g2-ds}

Retrieve the information about VPC floating IP. 
{: shortdesc}

### Sample Terraform code
{: #floating-ip-g2-dssample}

The following example retrieves information about the VPC floating IP.
{: shortdesc}

```

 data "ibm_is_floating_ip" "test" {
      name   = "test-fp"
 }

```

### Input parameters
{: #floating-ip-g2-dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required / optional|Description|
|----|-----------|--------|----------------------|
|`name`|String|Required|The name of the floating IP.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #floating-ip-g2-dsoutput}

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

## `ibm_is_flow_logs`
{: #ibm-is-flow-logs}

Import the details of an existing {{site.data.keyword.cloud_notm}} infrastructure flow logs as a read only data source.
{: shortdesc}

### Sample Terraform code
{: #ibm-is-flow-sample}


```
data "ibm_is_flow_logs" "ds_flow_logs" {
}

```

### Input parameters
{: #ibm-is-lb-dsinput}

There is no input parameters.
{: shortdesc}


### Output parameters
{: #ibm-is-flow-dsoutput}

Review the output parameters that you can access after you retrieve your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `flow_log_collectors` | String | Lists all the flow logs in the {{site.data.keyword.cloud_notm}}. |
| `flow_log_collectors.active` | String | Indicates whether the collector is active. |
| `flow_log_collectors.created_at` | String | The data and time the flow log created. |
| `flow_log_collectors.crn` | String | The CRN of the flow log collector. |
| `flow_log_collectors.href` | String | The URL of the flow log collector. |
| `flow_log_collectors.id` | String | The unique identifier of the flow log collector. |
| `flow_log_collectors.lifecycle_state` | String | The lifecycle state of the flow log collector. |
| `flow_log_collectors.name` | String | The flow log collector name. |
| `flow_log_collectors.resource_group` | String | The resource group of the flow log. |
| `flow_log_collectors.storage_bucket` | String | The COS bucket name where the flow logs are logged. |
| `flow_log_collectors.target` | String | The target ID that the flow log collector collects the flow logs. |
| `flow_log_collectors.vpc` | String | The VPC of the flow log collector that are associated. |

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

|Name|Data type| Required / optional|Description|
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


## `ibm_is_instance_group`
{: #instance-group}

Retrieve the details for all Gen 2 {{site.data.keyword.vsi_is_short}} instance group in your account.
{: shortdesc}

### Sample Terraform code
{: #instance-group-sample}

```
data "ibm_is_instance_group" "instance_group_data" {
	name =  ibm_is_instance_group.instance_group.name
}
```
{: codeblock}

### Input parameters
{: #instance-group-dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required / optional|Description|
|----|-----------|--------|----------------------|
|`name`|String|Required|The name of an instance group.|


### Output parameters
{: #instance-group-dsoutput}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id`|Object|The ID of an instance group. |
| `managers` | String | List of managers associated with the instance group.|
| `vpc` | String | The VPC ID. |
| `status` | String | The status of an instance group. |
| `instance_template` | String |The ID of an instance template to create an instance group. |
| `instance_count` | String | The number of instances created in an instance group. |
| `resource_group`| String | The resource group ID. |
| `subnets`| String | The list of subnet IDs used by an instances. |
| `application_port` | String | Scales an instances to supply the port for the Load Balancer pool member. |
| `load_balancer_pool` | String | The Load Balancer pool ID.|


## `ibm_is_instance_group_managers`
{: #instance-group-managers}

Retrieve all the instance group managers information of an instance group.
{: shortdesc}

### Sample Terraform code
{: #instance-group-managers-sample}

In the following example, you can retrieve a list of instance group managers information.

```
data "ibm_is_instance_group_managers" "instance_group_managers" {
    instance_group = "r006-76740f94-fcc4-11e9-96e7-a77723715315"
}
```
{: codeblock}

### Input parameters
{: #instance-group-managers-dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required / optional|Description|
|----|-----------|--------|----------------------|
|`instance_group`|String|Required|The instance group ID where the instance group manager is created.|

### Output parameters
{: #instance-group-managers-dsoutput}

Review the output parameters that you can access after you retrieved your data source.
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `instance_group_managers` | List of String | Nested block with list of instance manager properties. |
|`instance_group_managers.id`|Object|This ID is the combination of instance group ID, and instance group manager ID. |
|`instance_group_managers.manager_type`|String| The type of an instance group manager. |
|`instance_group_managers.aggregation_window`|String|The time window in seconds to aggregate metrics prior to evaluation. |
|`instance_group_managers.manager_id`|String|The instance group manager ID. |
|`instance_group_managers.policies`|String|List of policies associated with the instance group manager. |
|`instance_group_managers.cooldown`|The duration of time in seconds to pause further scale actions after scaling has taken place. |
|`instance_group_managers.max_membership_count`|String|The maximum number of members in a managed instance group. |
|`instance_group_managers.min_membership_count`|String|The minimum number of members in a managed instance group. |

## `ibm_is_instance_group_manager_policy`
{: #instance-group-managerpolicy}

Retrieve the policy information of an instance group manager.
{: shortdesc}

### Sample Terraform code
{: #instance-group-manager-policy-sample}

In the following example, you can retrieve a policy information of an instance group manager.

```
data "ibm_is_instance_group_manager_policy" "instance_group_manager_policy" {
  instance_group = "r006-76770f94-f7654-11e9-96e7-a77724435315"
  instance_group_manager = "r006-76770f94-f8764-11e9-96e7-a77726534315"
	name = "testpolicy
}
```
{: codeblock}

### Input parameters
{: #instance-group-manager-policy-dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required / optional|Description|
|----|-----------|--------|----------------------|
|`name`|String|Required|The name of the policy.|
|`instance_group`|String|Required|The instance group ID.|
|`instance_group_manager`|String|Required|The instance group manager ID|


### Output parameters
{: #instance-group-manager-policy-dsoutput}

Review the output parameters that you can access after you retrieved your data source.
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id`|Object|This ID is the combination of instance group ID, instance group manager ID and instance group manager policy ID. |
| `policy_type` | String | The type of metric to evaluate. |
| `metric_type` | String | The type of metric to evaluate. The possible values are `cpu`, `memory`, `network_in` and `network_out`. |
| `metric_value` | String |The metric value to evaluate. |
| `policy_id` | String | The policy ID. |

## `ibm_is_instance_group_manager_policies`
{: #instance-group-managerpolicies}

Retrieve the details for all Gen 2 {{site.data.keyword.vsi_is_short}} instance group manager policies in your account.
{: shortdesc}

### Sample Terraform code
{: #instance-group-manager-sample}

In the following example, you can retrieve a policy information of an instance group manager.

```
data "ibm_is_instance_group_manager_policy" "instance_group_manager_policy" {
  instance_group = "r006-76770f94-f7654-11e9-96e7-a77724435315"
  instance_group_manager = "r006-76770f94-f8764-11e9-96e7-a77726534315"
}
```
{: codeblock}

### Input parameters
{: #instance-group-manager-dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required / optional|Description|
|----|-----------|--------|----------------------|
|`instance_group`|String|Required|The instance group ID.|
|`instance_group_manager`|String|Required|The instance group manager ID|


### Output parameters
{: #instance-group-manager-dsoutput}

Review the output parameters that you can access after you retrieved your data source.
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id`|Object|This ID is the combination of instance group ID, instance group manager ID and instance group manager policy ID. |
| `name` | String | The policy name.|
| `policy_type` | String | The type of metric to evaluate. |
| `metric_type` | String | The type of metric to evaluate. The possible values are `cpu`, `memory`, `network_in` and `network_out`. |
| `metric_value` | String |The metric value to evaluate. |
| `policy_id` | String | The policy ID. |

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
	name = "b-2x8"
}

```

### Input parameters
{: #vpc-instance-profile-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type| Required / optional|Description|
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

This data source does not support any input parameters.
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






## `ibm_is_public_gateway`
{: #public-gwy-g2}

Retrieve the details for a public gateway data source.
{: shortdesc}

### Sample Terraform code
{: #public-gwy-g2-sample}

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
{: #public-gwy-g2-dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
| `name` | String | Required | The name of the gateway. |
| `resource_group` | String | Optional | The resource group ID of the public gateway. **Note** This parameter is supported only for VPC Gen 2 infrastructure. |

### Output parameters
{: #public-gwy-g2-dsoutput}

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






## `ibm_is_lbs`
{: #ibm-is-lbs}

Import the details of an existing IBM VPC Load Balancers as a read only data source.
{: shortdesc}

### Sample Terraform code
{: #ibm-is-lbs-sample}

The following example shows how you can declare the data. 
{: shortdesc}

```
data "ibm_is_lbs" "ds_lbs" {
 }
```

### Input parameters
{: #ibm-is-lbs-dsinput}

There is no input parameters for bim-is-lbs data source. 
{: shortdesc}


### Output parameters
{: #ibm-is-lbs-dsoutput}

Review the output parameters that you can access after you retrieve your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`load_balancers`|String|The Collection of load balancers.|
|`loadbalancers.id`|String|The unique identifier of the load balancer.|
|`loadbalancers.created_at`|String|The date and time this load balancer was created.|
|`loadbalancers.crn`|String|The load balancer's CRN.|
|`loadbalancers.name`|String|Name of the loadbalancer.|
|`loadbalancers.subnets`|String|The subnets this load balancer is part of.|
|`loadbalancers.subnets.crn`|String|The CRN for the subnet.|
|`loadbalancers.subnets.id`|String|The unique identifier for this subnet.|
|`loadbalancers.subnets.href`|String|The URL for this subnet.|
|`loadbalancers.subnets.name`|String|The user-defined name for this subnet.|
|`loadbalancers.hostname`|String|The Fully qualified domain name assigned to this load balancer.|
|`loadbalancers.listeners`|String|The listeners of this load balancer.|
|`loadbalancers.listeners.id`|String|The unique identifier for this load balancer listener.|
|`loadbalancers.listeners.href`|String|The listener's canonical URL.|
|`loadbalancers.operating_status`|String|The operating status of this load balancer.|
|`loadbalancers.pools`|String|The pools of this load balancer.|
|`loadbalancers.pools.href`|String|The pool's canonical URL.|
|`loadbalancers.pools.id`|String|The unique identifier for this load balancer pool.|
|`loadbalancers.pools.name`|String|The user-defined name for this load balancer pool.|
|`loadbalancers.profile`|String|The profile to use for this load balancer.|
|`loadbalancers.profile.family`|String|The product family this load balancer profile belongs to.|
|`loadbalancers.profile.href`|String|The URL for this load balancer profile.|
|`loadbalancers.profile.name`|String|The name for this load balancer profile.|
|`loadbalancers.private_ips`|String|The private IP addresses assigned to this load balancer.|
|`loadbalancers.provisioning_status`|String|The provisioning status of this load balancer. Possible values are: **active**, **create_pending**, **delete_pending**, **failed**, **maintenance_pending**, **update_pending**|
|`loadbalancers.public_ips`|String|The public IP addresses assigned to this load balancer.|
|`loadbalancers.resource_group`|String|The resource group where the load balancer is created.|
|`loadbalancers.status`|String|The status of the load balancers.|
|`loadbalancers.type`|String|The type of the load balancer.|
|`loadbalancers.tags`|String|Tags associated with the load balancer.|






## `ibm_is_lb_profiles`
{: #ibm-is-lb-profiles}

Retrieve information of an existing {{site.data.keyword.cloud_notm}} Infrastructure load balancer profiles.
{: shortdesc}

### Sample Terraform code
{: #ibm-is-lb-profiles-sample}

```

data "ibm_is_lb_profiles" "ds_lb_profiles" {
}

```

### Input parameters
{: #ibm-is-lb-profiles-input}

There is no input parameters that you can specify for your data source. 
{: shortdesc}

### Output parameters
{: #ibm-is-lb-profiles-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`lb_profiles`|String|List of all load balancer profiles in the IBM Cloud Infrastructure.|
|`lb_profiles.family`|String|The product family this load balancer profile belongs to.|
|`lb_profiles.href`|String|The URL for this load balancer profile.|
|`lb_profiles.name`|String|The name for this load balancer profile.|






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

|Name|Data type| Required / optional|Description|
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
|`rules.port_max`|Integer|The TCP/UDP port range that includes the maximum bound. |
|`rules.port_min`|Integer|The TCP/UDP port range that includes the minimum bound. |
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

|Name|Data type| Required / optional|Description|
|----|-----------|--------|----------------------|
|`name`|String|Required|The name of the SSH key.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #vpc-ssh-key-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`id`|String|The ID of the SSH key.|
|`fingerprint`| String| The SHA256 fingerprint of the public key.|
|`length`|String|The length of the SSH key.|
|`type`|String|The crypto system that is used by this key.|
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

|Name|Data type| Required / optional|Description|
|----|-----------|--------|----------------------|
|`identifier`|String|Optional|The ID of the subnet.|
|`name`|String|Optional|The name of the subnet.|
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

## `ibm_is_instance_templates`
{: #vpc-instance-templates}

Retrieve information about all the instance template in an account. 
{: shortdesc}

### Sample Terraform code
{: #vpc-instance-templates-sample}

The following example, you can fetch information of list of the instance templates VPC gen-2 infrastructure.
{: shortdesc}

```
data "ibm_is_instance_templates" "instancetemplates" {	   
}
```

### Input parameters
{: #vpc-instance-templates-input}

This data source does not support any input parameters.
{: shortdesc}

### Output parameters
{: #vpc-instance-templates-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|-------------|
|`templates`|List of objects|List of templates.  |
|`templates.id`|String|The ID of the instance template. |
|`templates.name`|String|The name of the instance template. |
|`templates.image`|String|The ID of the image to create the template.|
|`templates.profile`|String|The number of instances created in the instance group. |
|`templates.vpc`|String|The VPC ID that the instance templates needs to be created.|
|`templates.zone`|String|The name of the zone. |
|`templates.keys`|String|List of SSH key IDs used to allow log in user to the instances. |
|`templates.resource_group`|String|The resource group ID. |
|`templates.primary_network_interfaces`|String|A nested block describes the primary network interface for the template. |
|`templates.primary_network_interfaces.subnet`|String|The VPC subnet to assign to the interface. |
|`templates.primary_network_interfaces.name`|String|The name of the interface. |
|`templates.primary_network_interfaces.security_groups`|String|List of security groups of the subnet. |
|`templates.primary_network_interfaces.primary_ipv4_address`|String|The IPv4 address assigned to the primary network interface. |
|`templates.network_interfaces`|String|A nested block describes the network interfaces for the template. |
|`templates.network_interfaces.subnet`|String|The VPC subnet to assign to the interface. |
|`templates.network_interfaces.name`|String|The name of the interface. |
|`templates.network_interfaces.security_groups`|String|List of security groups of  the subnet. |
|`templates.network_interfaces.primary_ipv4_address`|String|The IPv4 address assigned to the network interface. |
|`templates.boot_volume`|String|A nested block describes the boot volume configuration for the template. |
|`templates.boot_volume.encryption`|String|The encryption key CRN to encrypt the boot volume attached. |
|`templates.boot_volume.name`|String|The name of the boot volume. |
|`templates.boot_volume.size`|String|The boot volume size to configure in giga bytes. |
|`templates.boot_volume.iops`|String|The IOPS for the boot volume. |
|`templates.boot_volume.profile`|String|The profile for the boot volume configuration. |
|`templates.boot_volume.delete_volume_on_instance_delete`|String|You can configure to delete the boot volume based on instance deletion. |
|`templates.volume_attachments`|String|A nested block describes the storage volume configuration for the template. |
|`templates.volume_attachments.name`|String|The name of the boot volume. |
|`templates.volume_attachments.volume`|String|The storage volume ID created in VPC. |
|`templates.volume_attachments.delete_volume_on_instance_delete`|Boolean|You can configure to delete the storage volume to delete based on instance deletion. |
|`templates.user_data` | String|The user data provided for the instance. |
{: caption="Table 1. Available output parameters" caption-side="top"}






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

|Name|Data type| Required / optional|Description|
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

|Name|Data type| Required / optional|Description|
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

|Name|Data type| Required / optional|Description|
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





