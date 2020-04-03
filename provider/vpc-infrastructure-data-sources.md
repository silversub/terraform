---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-03"

keywords: terraform provider plugin, terraform vpc gen 1 compute, terraform vpc, terraform gen 1 resources, terraform vpc subnet, generation 1 compute terraform

subcollection: terraform

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}

# VPC infrastructure data sources (Gen 1 compute)
{: #vpc-gen1-data-sources}

Review the data sources that you can use to retrieve information about your [{{site.data.keyword.vpc_full}} (Gen 1 compute) infrastructure](/docs/vpc?topic=vpc-about-vpc). All data sources are imported as read-only information. You can reference the output parameters for each data source by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 
{: shortdesc}

Before you start working with your data source, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


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

| Input parameter | Data type | Required/ optional | Description |
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

| Input parameter | Data type | Required/ optional | Description |
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







## `ibm_is_region`
{: #region}

Retrieve the details for a {{site.data.keyword.vpc_full}} regions. 
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

| Input parameter | Data type | Required/ optional | Description |
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

| Input parameter | Data type | Required/ optional | Description |
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
| `type` | String | The cryptosystem that is used by the SSH key. | 
| `resource_controller_url` | String | The URL of the {{site.data.keyword.cloud_notm}} dashboard that you can use to view details about the SSH key. |






## `ibm_is_subnet`
{: #subnet}

Retrieve details for a {{site.data.keyword.vpc_full}} subnet. 
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

### Input parameters
{: #subnet-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `identifier` | String | Required | The ID of the subnet. To list subnets in your account, run the `ibmcloud is subnets` command. | 

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
| `resource_controller_url` | String | The URL of the {{site.data.keyword.cloud_notm}} dashboard that you can use to view details about the subnet. |
| `status` | String | The status of the subnet. |
| `total_ipv4_address_count` | Integer | The total number of IPv4 addresses. |
| `vpc` | String | The ID of the VPC that the subnet is attached to. |
| `zone` | String | The name of the zone where the subnet is provisioned. |







## `ibm_is_vpc`
{: #vpc}

Retrieve information about a {{site.data.keyword.vpc_full}}. 
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

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `name` | String | Required | The name of the VPC. To list available VPCs, run the `ibmcloud is vpcs` command. |

### Output parameters
{: #vpc-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `classic_access` | String | Indicates whether this VPC is set up with a connection to classic {{site.data.keyword.cloud_notm}} infrastructure. | 
| `cse_source_addresses`|List of Cloud Service Endpoints|A list of the cloud service endpoints that are associated with your VPC, including their source IP address and zone.|
|`cse_source_addresses.address`|String|The IP address of the cloud service endpoint.|
|`cse_source_addresses.zone_name`|String|The zone where the cloud service endpoint is located.|
| `default_network_acl` | String | The ID of the default network access control list (ACL) that was set up for the VPC. | 
| `resource_controller_url` | String | The URL of the {{site.data.keyword.cloud_notm}} dashboard that you can use to view details about the VPC. |
| `resource_group` | String | The resource group ID where the VPC was created. |
|`subnets`|List of subnets|A list of subnets that are attached to a VPC.|
|`subnets.name`|String|The name of the subnet.|
|`subnets.id`|String|The ID of the subnet.|
|`subnets.status`|String|The status of the subnet.|
|`subents.total_ipv4_address_count`|Integer|The total number of IPv4 addresses in the subnet.|
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

| Input parameter | Data type | Required/ optional | Description |
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

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `region` | String | Required | The name of the region. |
| `status` | String | Optional | The status of the zone that you want to filter for.  |

### Output parameters
{: #zones-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `zones` | Array | The list of zones in an {{site.data.keyword.cloud_notm}} region. |






