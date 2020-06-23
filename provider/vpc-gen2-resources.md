---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-23" 

keywords: terraform provider plugin, terraform vpc gen 2 resources, terraform vpc generation 2, terraform vpc subnet, terraform vpc generation 2 compute

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

# VPC infrastructure resources (Gen 2 compute)
{: #vpc-gen2-resources}

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## `ibm_is_floating_ip`
{: #provider-floating-ip}

Create a floating IP address that you can associate with a {{site.data.keyword.vsi_is_short}} instance. You can use the floating IP address to access your instance from the public network, independent of whether the subnet is attached to a public gateway. 
{: shortdesc}

For more information, see [About Networking for VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-about-networking-for-vpc). 

### Sample Terraform code
{: #floating-ip-sample}

The following example shows how to create a {{site.data.keyword.vsi_is_short}} instance and associate a floating IP address to the primary network interface of the virtual server instance. 

```
resource "ibm_is_instance" "testacc_instance" {
  name    = "testinstance"
  image   = "7eb4e35b-4257-56f8-d7da-326d85452591"
  profile = "bx2-2x8"

  primary_network_interface {
    port_speed = "1000"
    subnet     = "70be8eae-134c-436e-a86e-04849f84cb34"
  }

  vpc  = "01eda778-b822-43a2-816d-d30713df5e13"
  zone = "us-south-1"
  keys = ["eac87f33-0c00-4da7-aa66-dc2d972148bd"]
}

resource "ibm_is_floating_ip" "testacc_floatingip" {
  name   = "testfip1"
  target = ibm_is_instance.testacc_instance.primary_network_interface.0.id
}
```
{: codeblock}

### Input parameters
{: #floating-ip-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `name` | String | Required | Enter a name for the floating IP address. | 
| `target` | String | Optional | Enter the ID of the network interface that you want to use to allocate the IP address. If you specify this option, do not specify `zone` at the same time. | 
| `zone` | String | Optional | Enter the name of the zone where you want to create the floating IP address. To list available zones, run `ibmcloud is zones`. If you specify this option, do not specify `target` at the same time. | 
| `resource_group`|String|Optional| The resource group ID where you want to create the floating IP.|
| `tags`|Array of strings|Optional|Enter any tags that you want to associate with your VPC. Tags might help you find your VPC more easily after it is created. Separate multiple tags with a comma (`,`). |

### Output parameters
{: #floating-ip-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `address` | String | The floating IP address that was created. | 
| `id` | String | The unique identifier of the floating IP address. | 
| `resource_controller_url` | String | The URL of the {{site.data.keyword.cloud_notm}} dashboard that you can use to explore and view details about the floating IP address. | 
| `status` | String | The provisioning status of the floating IP address. |

### Timeouts
{: #floating-ip-timeout}

The following timeouts are defined for this resource. 
{: shortdesc}

- **create**: The creation of the floating IP address is considered `failed` if no response is received for 10 minutes. 
- **delete**: The deletion of the floating IP address is considered `failed` if no response is received for 10 minutes. 

## `ibm_is_ike_policy`
{: #provider-ike-policy}

Create, update, or cancel an Internet Key Exchange (IKE) policy. 
{: shortdesc}

IKE is an IPSec (Internet Protocol Security) standard protocol that is used to ensure secure communication over the VPC VPN service. For more information, see [Using VPC with your VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc). 

### Sample Terraform code
{: #ike-sample}

```
resource "ibm_is_ike_policy" "example" {
    name = "test"
    authentication_algorithm = "md5"
    encryption_algorithm = "triple_des"
    dh_group = 2
    ike_version = 1
}
```
{: codeblock}

### Input parameters
{: #ike-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `authentication_algorithm` | String | Required | Enter the algorithm that you want to use to authenticate IPSec peers. Available options are `md5`, `sha1`, or `sha256`. |
| `dh_group` | Integer | Required | Enter the Diffie-Hellman group that you want to use for the encryption key. Available options are `2`, `5`, or `14`. |
| `encryption_algorithm` | String | Required | Enter the algorithm that you want to use to encrypt data. Available options are: `triple_des`, `aes128`, or `aes256`. | 
| `ike_version` | Integer | Optional | Enter the IKE protocol version that you want to use. Available options are `1`, or `2`. |
| `key_lifetime` | Integer | Optional | Enter the time in seconds that your encryption key can be used before it expires. You must enter a number between 300 and 86400. If you do not specify this option, 28800 seconds is used. | 
| `name` | String | Required | Enter a name for your IKE policy. | 
| `resource_group` | String | Optional | Enter the ID of the resource group where you want to create the IKE policy. To list available resource groups, run `ibmcloud resource groups`. If you do not specify a resource group, the IKE policy is created in the `default` resource group. | 

### Output parameters
{: #ike-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `href`| String| The canonical URL that was assigned to your IKE policy. | 
| `id` | String | The unique identifier of the IKE policy that you created. |
| `negotiation_mode` | String | The negotiation mode that was set for your IKE policy. Only `main` is supported. | 
| `resource_controller_url` | String | The URL of the {{site.data.keyword.cloud_notm}} dashboard that you can use to explore and view details about the IKE policy. | 
| `vpn_connections`| List | A collection of VPN connections that use the IKE policy. Every connection is listed with a VPC connection `name`, `id`, and `canonical URL`. | 

## `ibm_is_instance`
{: #provider-instance}

Create, update, or delete a {{site.data.keyword.vsi_is_short}} instance. 
{: shortdesc}

### Sample Terraform code
{: #instance-sample}

#### Example for creating an instance in a VPC

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
  public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCKVmnMOlHKcZK8tpt3MP1lqOLAcqcJzhsvJcjscgVERRN7/9484SOBJ3HSKxxNG5JN8owAjy5f9yYwcUg+JaUVuytn5Pv3aeYROHGGg+5G346xaq3DAwX6Y5ykr2fvjObgncQBnuU5KHWCECO/4h8uWuwh/kfniXPVjFToc+gnkqA+3RKpAecZhFXwfalQ9mMuYGFxn+fwn8cYEApsJbsEmb0iJwPiZ5hjFC8wREuiTlhPHDgkBLOiycd20op2nXzDbHfCHInquEe/gYxEitALONxm0swBOwJZwlTDOB7C6y2dzlrtxr1L59m7pCkWI4EtTRLvleehBoj3u7jB4usR"
}

resource "ibm_is_instance" "testacc_instance" {
  name    = "testinstance"
  image   = "7eb4e35b-4257-56f8-d7da-326d85452591"
  profile = "bx2-2x8"

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

  //User can configure timeouts
  timeouts {
    create = "90m"
    delete = "30m"
  }
}
```

#### Example for creating an instance with custom security group rules
{: #custom-sec-group-rules}

The following example shows how you can create a virtual server instance with custom security group rules. Note that the security group, security group rules, and the virtual server instance must be created in a specific order to meet the dependencies of the individual resources. To force the creation in a specific order, you use the [`depends_on` parameter](https://www.terraform.io/docs/configuration/resources.html){: external}. If you do not provide this parameter, all resources are created at the same time which might lead to resource dependency errors during the provisioning of your virtual server, such as `The security group to attach to is not available`.

```
resource "ibm_is_vpc" "testacc_vpc" {
    name = "test"
}

resource "ibm_is_security_group" "testacc_security_group" {
    name = "test"
    vpc = ibm_is_vpc.testacc_vpc.id
}

resource "ibm_is_security_group_rule" "testacc_security_group_rule_all" {
    group = ibm_is_security_group.testacc_security_group.id
    direction = "inbound"
    remote = "127.0.0.1"
    depends_on = [ibm_is_security_group.testacc_security_group]
 }

 resource "ibm_is_security_group_rule" "testacc_security_group_rule_icmp" {
    group = ibm_is_security_group.testacc_security_group.id
    direction = "inbound"
    remote = "127.0.0.1"
    icmp {
        code = 20
        type = 30
    }
    depends_on = [ibm_is_security_group_rule.testacc_security_group_rule_all]

 }

 resource "ibm_is_security_group_rule" "testacc_security_group_rule_udp" {
    group = ibm_is_security_group.testacc_security_group.id
    direction = "inbound"
    remote = "127.0.0.1"
    udp {
        port_min = 805
        port_max = 807
    }
    depends_on = [ibm_is_security_group_rule.testacc_security_group_rule_icmp]
 }

 resource "ibm_is_security_group_rule" "testacc_security_group_rule_tcp" {
    group = ibm_is_security_group.testacc_security_group.id
    direction = "outbound"
    remote = "127.0.0.1"
    tcp {
        port_min = 8080
        port_max = 8080
    }
    depends_on = [ibm_is_security_group_rule.testacc_security_group_rule_udp]
 }

resource "ibm_is_instance" "testacc_instance" {
  name    = "testinstance"
  image   = "7eb4e35b-4257-56f8-d7da-326d85452591"
  profile = "b-2x8"

  primary_network_interface {
    subnet = ibm_is_subnet.testacc_subnet.id
    security_groups = [ibm_is_security_group.testacc_security_group.id]
  }

  vpc  = ibm_is_vpc.testacc_vpc.id
  zone = "us-south-1"
  keys = [ibm_is_ssh_key.testacc_sshkey.id]
  depends_on = [ibm_is_security_group_rule.testacc_security_group_rule_tcp]

  //User can configure timeouts
  timeouts {
    create = "90m"
    delete = "30m"
  }
}
```
{: codeblock}

### Input parameters
{: #instance-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
|`name`|String|Optional|The instance name.|
|`vpc`|String|Required|The ID of the VPC where you want to create the instance.|
|`zone`|String|Required|The name of the VPC zone where you want to create the instance.|
|`profile`|String|Required|The name of the profile that you want to use for your instance. To list supported profiles, run `ibmcloud is instance-profiles`.|
|`image`|String|Required|The ID of the virtual server image that you want to use. To list supported images, run `ibmcloud is images`.|
|`boot_volume`|List|Optional|A list of boot volumes for an instance.|
|`boot_volume.name`|String|Optional|The name of the boot volume.|
|`boot_volume.encryption`|String|Optional|The type of encryption to use for the boot volume.|
|`keys`|List|Required|A comma separated list of SSH keys that you want to add to your instance.|
|`primary_network_interface`|List|Required|A nested block describing the primary network interface of this instance. Only one primary network interface can be specified for an instance.|
|`primary_network_interface.name`|String|Optional|The name of the network interface.|
|`primary_network_interface.subnet`|String|Required|The ID of the subnet.|
|`primary_network_interface.security_groups`|List of strings|Optional|A comma separated list of security groups to add to the primary network interface.|
|`network_interfaces`|List|Optional|A list of additional network interfaces that are set up for the instance.|
|`network_interfaces.name`|String|Optional|The name of the network interface.|
|`network_interfaces.subnet`|String|Required|The ID of the subnet.|
|`network_interfaces.security_groups`|List of strings|Optional|A comma separated list of security groups to add to the primary network interface.|
|`volumes`|List|Optional|A comma separated list of volume IDs to attach to the instance.|
|`user_data`|String|Optional|User data to transfer to the instance.|
|`resource_group`|String|Optional|The ID of the resource group where you want to create the instance.|
|`tags`|Array of strings|Optional|A list of tags that you want to add to your instance. Tags can help you find your instance more easily later.|

### Output parameters
{: #instance-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id`|String|The ID of the instance.|
|`memory`|Integer|The amount of memory that is allocated to the instance in gigabytes.|
|`status`|String|The status of the instance.|
|`vcpu`|List of virtual CPUs|A list of virtual CPUs that are allocated to the instance.|
|`vcpu.architecture`|String|The architecture of the CPU.|
|`vcpu.count`|Integer|The number of virtual CPUS that are assigned to the instance.|
|`gpu`|List of GPUs|A list of GPUs that are assigned to the instance.|
|`gpu.cores`|Integer|The number of cores of the GPU.|
|`gpu.count`|Integer|The count of the GPU.|
|`gpu.manufacture`|String|The manufacturer of the GPU.|
|`gpu.memory`|Integer|The amount of memory of the GPU in gigabytes.|
|`gpu.model`|String|The model of the GPU.|
|`primary_network_interface`|List of primary network interfaces|A list of primary network interfaces that are attached to the instance.|
|`primary_network_interface.id`|String|The ID of the primary network interface.|
|`primary_network_interface.name`|String|The name of the primary network interface.|
|`primary_network_interface.subnet`|String|The ID of the subnet that the primary network interface is attached to.
|`primary_network_interface.security_groups`|List of strings|A list of security groups that are used in the primary network interface.|
|`primary_network_interface.primary_ipv4_address`|String|The primary IPv4 address.|
|`network_interfaces`|List of additional network interfaces|A list of additional network interfaces that are attached to the instance.|
|`network_interfaces.id`|String|The ID of the network interface.|
|`network_interfaces.name`|String|The name of the network interface.|
|`network_interfaces.subnet`|String|The ID of the subnet.|
|`network_interfaces.security_groups`|List of strings|A list of security groups that are used in the network interface.|
|`network_interfaces.primary_ipv4_address`|String|The primary IPv4 address.|
|`boot_volume`|List of boot volumes|A list of boot volumes that the instance uses.|
|`boot_volume.name`|String|The name of the boot volume.|
|`boot_volume.size`|Integer|The capacity of the volume in gigabytes.|
|`boot_volume.iops`|Integer|The number of input and output operations per second of the volume.|
|`boot_volume.profile`|String|The profile of the volume.|
|`boot_volume.encryption`|String|The type of encryption that is used for the boot volume.|
|`volume_attachments`|List of volume attachments|A list of volume attachments for the instance.|
|`volume_attachments.id`|String|The ID of the volume attachment.|
|`volume_attachments.name`|String|The name of the volume attachment.|
|`volume_attachments.volume_id`|String|The ID of the volume that is used in the volume attachment.|
|`volume_attachments.volume_name`|String|The name of the volume that is used in the volume attachment.|
|`volume_attachments.volume_crn`|String|The CRN of the volume that is used in the volume attachment.|
|`resource_controller_url`|String|The URL of the {{site.data.keyword.cloud_notm}} dashboard that can be used to explore and view details about this instance.|

### Timeout
{: #instance-timeout}

The following timeouts are defined for the instance: 

- **create**: The creation of the instance is considered failed when no response is received for 30 minutes. 
- **update**: The update of the instance or the attachment of a volume to an instance is considered failed when no response is received for 30 minutes. 
- **delete**: The deletion of the instance is considered failed when no response is received for 30 minutes. 

### Import
{: #instance-import}

`ibm_is_instance` can be imported by using the instance ID

```
terraform import ibm_is_instance.example a1aaa111-1111-111a-1a11-a11a1a11a11a
```
{: pre}

## `ibm_is_ipsec_policy`
{: #provider-ipsec}

Create, update, or cancel an IPSec policy. 
{: shortdesc}

### Sample Terraform code
{: #ipsec-policy-sample}

```
resource "ibm_is_ipsec_policy" "example" {
    name = "test"
    authentication_algorithm = "md5"
    encryption_algorithm = "triple_des"
    pfs = "disabled"
}
```
{: codeblock}

### Input parameters
{: #ipsec-policy-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `authentication_algorithm` | String | Required | Enter the algorithm that you want to use to authenticate IPSec peers. Available options are `md5`, `sha1`, or `sha256`. |
| `encryption_algorithm` | String | Required | Enter the algorithm that you want to use to encrypt data. Available options are: `triple_des`, `aes128`, or `aes256`. | 
| `key_lifetime` | Integer | Optional | Enter the time in seconds that your encryption key can be used before it expires. You must enter a number between 300 and 86400. If you do not specify this option, 3600 seconds is used. | 
| `name` | String | Required | Enter the name for your IPSec policy. |
| `pfs` | String | Required | Enter the Perfect Forward Secrecy (PFS) protocol that you want to use during a session. Available options are `disabled`, `group_2`, `group_5`, and `group_14`. | 
| `resource_group` | String | Optional | Enter the ID of the resource group where you want to create the IPSec policy. To list available resource groups, run `ibmcloud resource groups`. If you do not specify a resource group, the IPSec policy is created in the `default` resource group. | 

### Output parameters
{: #ike-policy-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `encapsulation_mode` | String | The encapsulation mode that was set for your IPSec policy. Only `tunnel` is supported. |
| `id` | String | The unique identifier of the IPSec policy that you created. |
| `resource_controller_url` | String | The URL of the {{site.data.keyword.cloud_notm}} dashboard that you can use to explore and view details about the IPSec policy. | 
| `transform_protocol` | String | The transform protocol that is used in your IPSec policy. Only the `esp` protocol is supported that uses the triple DES (3DES) encryption algorithm to encrypt your data. |
| `vpn_connections`| List | A collection of VPN connections that use the IPSec policy. Every connection is listed with a VPC connection `name`, `id`, and `canonical URL`. | 

## `ibm_is_image`
{: #image}

Upload, update, or delete a custom virtual server instance image. For more information about how to create a custom image, see the [VPC documentation](/docs/vpc?topic=vpc-managing-images).
{: shortdesc}

### Sample Terraform code
{: #image-sample}

```
resource "ibm_is_image" "test_is_images" {
 name                   = "test_image"
 href                   = "test_image_path"
 operating_system       = "test_os_info"
}
```

### Input parameters
{: #image-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required|The descriptive name used to identify an image.|
|`href`|String|Required| The path of an image to be uploaded.|
|`operating_system`|String|Required|Description of underlying OS of an image.|
|`resource_group`|String|Optional|The resource group ID for this image.|
|`tags`|Array of strings|Optional|A list of tags that you want to your image. Tags can help you find the image more easily later.|

### Output parameters
{: #image-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the image.|
|`architecture`|String|The processor architecture that this image is based on.|
|`crn`|String| The CRN for the image.|
|`file`|String| The file.|
|`format`|String| The format of an image.|
|`resourceGroup`|String| The resource group to which the image belongs to.|
|`status`|String| - The status of an image such as `corrupt`, or `available`.|
|`visibility`|String|The access scope of an image such as `private` or `public`.|

## `ibm_is_lb`
{: #lb}

Create, update, or delete a VPC load balancer. 
{: shortdesc}

### Sample Terraform code
{: #lb-sample}

```
resource "ibm_is_lb" "lb" {
  name    = "loadbalancer1"
  subnets = ["04813493-15d6-4150-9948-6cc646cb67f2"]
}
```

### Input parameters
{: #lb-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required|The name of the VPC load balancer.|
|`subnets`|Array|Required|List of the subnets IDs to connect to the load balancer.
|`type`|String|Optional|The type of the load balancer. Default value `public`. Supported values `public` and `private`.|
|`resource_group`|String|Optional| The resource group where the load balancer to be created.|
|`tags`|List of strings|Optional|A list of tags that you want to add to your load balancer. Tags can help you find the load balancer more easily later. |

### Output parameters
{: #lb-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the load balancer.|
|`public_ips`|String|The public IP addresses assigned to this load balancer.|
|`private_ips`|String|The private IP addresses assigned to this load balancer.|
|`status`|String|The status of the load balancer.|
|`operating_status`|String|The operating status of this load balancer.|
|`hostname`|String|The fully qualified domain name assigned to this load balancer.|
|`resource_controller_url`|String|The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.|

### Import
{: #lb-import}

`ibm_is_lb` can be imported using the load balancer ID. 

```
terraform import ibm_is_lb.example <lb_ID>
```
{: pre}

### Timeouts
{: #lb-timeout}

`ibm_is_lb` provides the following Timeouts. 

- **create** - (Default 10 minutes) Used for creating Instance.
- **delete** - (Default 10 minutes) Used for deleting Instance.

## `ibm_is_lb_listener`
{: #lb-listener}

Create, update, or delete a listener for a VPC load balancer.

When provisioning the load balancer listener along with load balancer pool or pool member, use [explicit dependencies](https://learn.hashicorp.com/terraform/getting-started/dependencies#implicit-and-explicit-dependencies){: external} on the resources or perform the terraform apply with parallelism 1. 
{: note}

### Sample Terraform code
{: #lb-listener-sample}

```
resource "ibm_is_lb_listener" "testacc_lb_listener" {
  lb       = "8898e627-f61f-4ac8-be85-9db9d8bfd345"
  port     = "9080"
  protocol = "http"
}

resource "ibm_is_lb_pool" "webapptier-lb-pool" {
  lb                 = "8898e627-f61f-4ac8-be85-9db9d8bfd345"
  name               = "a-webapptier-lb-pool"
  protocol           = "http"
  algorithm          = "round_robin"
  health_delay       = "5"
  health_retries     = "2"
  health_timeout     = "2"
  health_type        = "http"
  health_monitor_url = "/"
  depends_on         = [ibm_is_lb_listener.testacc_lb_listener]
}

resource "ibm_is_lb_pool_member" "webapptier-lb-pool-member-zone1" {
  count          = "2"
  lb             = "8898e627-f61f-4ac8-be85-9db9d8bfd345"
  pool           = element(split("/", ibm_is_lb_pool.webapptier-lb-pool.id), 1)
  port           = "80"
  target_address = "192.168.0.1"
  depends_on     = [ibm_is_lb_listener.testacc_lb_listener]
}
```

### Input parameters
{: #lb-listener-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`lb`|String|Required|The load balancer unique identifier.|
|`port`|Integer|Required|The listener port number. Valid range 1 to 65535.|
|`protocol`|String|Required|The listener protocol. Supported values are `http`, `tcp`, and `https`.|
|`default_pool`|String|Optional| The load balancer pool unique identifier.|
|`certificate_instance`|String|Optional|The CRN of the certificate instance.|
|`connection_limit`|Integer|Optional|The connection limit of the listener. Valid range 1 to 15000.|

### Output parameters
{: #lb-listener-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the load balancer listener.|
|`status`|String|The status of load balancer listener.|

### Import
{: #lb-listener-import}

`ibm_is_lb_listener` can be imported using the load balancer ID and listener ID.

```
terraform import ibm_is_lb_listener.example <loadbalancer_ID>/<listener_ID>
```
{: pre}

### Timeouts
{: #lb-listener-timeout}

`ibm_is_lb_listener` provides the following timeouts:

- **create** - (Default 10 minutes) Used for creating Instance.
- **update** - (Default 10 minutes) Used for updating Instance.
- **delete** - (Default 10 minutes) Used for deleting Instance.

## `ibm_is_lb_listener_policy`
{: #lb-listener-policy}

Create, update, or delete a load balancer listener policy.  
{: shortdesc}

### Sample Terraform code
{: #lb-listener-policy-sample}


#### Creating a load balancer listener policy for a `redirect` action
{: #redirect-sample}

```
resource "ibm_is_lb" "lb2"{
  name    = "mylb"
  subnets = ["<subnet_ID>"]
}

resource "ibm_is_lb_listener" "lb_listener2"{
  lb       = ibm_is_lb.lb2.id
  port     = "9086"
  protocol = "http"
}
resource "ibm_is_lb_listener_policy" "lb_listener_policy" {
  lb = ibm_is_lb.lb2.id
  listener = ibm_is_lb_listener.lb_listener2.listener_id
  action = "redirect"
  priority = 2
  name = "mylistener8"
  target_http_status_code = 302
  target_url = "https://www.redirect.com"
  rules{
      condition = "contains"
      type = "header"
      field = "1"
      value = "2"
  }
}
```
{: codeblock}

#### Creating a load balancer listener policy for a `forward` action
{: #forward-sample}

```
resource "ibm_is_lb" "lb2"{
  name    = "mylb"
  subnets = ["<subnet_ID>"]
}

resource "ibm_is_lb_listener" "lb_listener2"{
  lb       = ibm_is_lb.lb2.id
  port     = "9086"
  protocol = "http"
}
resource "ibm_is_lb_listener_policy" "lb_listener_policy" {
  lb = ibm_is_lb.lb2.id
  listener = ibm_is_lb_listener.lb_listener2.listener_id
  action = "forward"
  priority = 2
  name = "mylistener8"
  target_id = "r006-beafdff0-4fe0-4db4-8f0c-b0b4ad828712"
  rules{
      condition = "contains"
      type = "header"
      field = "1"
      value = "2"
  }
}
```
{: codeblock}

### Input parameters
{: #lb-listener-policy-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
|`lb`|String|Required|The ID of the load balancer for which you want to create a load balancer listener policy.| 
|`listener`|String|Required|The ID of the load balancer listener.|
|`action`|String|Required|The action that you want to specify for your policy. Supported values are `forward`, `redirect`, and `reject`.|
|`priority`|Integer|Required|The priority of the load balancer policy. Low values indicate a high priority. The value must be between 1 and 10.|
|`name`|String|Optional|The name for the load balancer policy. Names must be unique within a load balancer listener.|
|`rules`|List of policy rules|Required|A list of rules that you want to apply to your load balancer policy. Note that rules can be created only. You cannot update the rules for a load balancer policy.|
|`rules.condition`|String|Required|The condition that you want to apply to your rule. Supported values are `contains`, `equals`, and `matches_regex`.|
|`rules.type`|String|Required|The data type where you want to apply the rule condition. Supported values are `header`, `hostname`,  and `path`|
|`rules.value`|Integer|Required|The value that must be found in the HTTP header, hostname or path to apply the load balancer listener rule. The value that you define can be between 1 and 128 characters long.|
|`rules.field`|Integer|Required|If you selected `header` as the data type where you want to apply the rule condition, enter the name of the HTTP header that you want to check. The name of the header can be between 1 and 128 characters long. |
|`target_id`|Integer|Optional|When `action` is set to **forward**, specify the ID of the load balancer pool that the load balancer forwards network traffic to. |
|`target_http_status_code`|Integer|Optional|When `action` is set to **redirect**, specify the HTTP response code that must be returned in the redirect response. Supported values are `301`, `302`, `303`, `307`, and `308`.  
|`target_url`|Integer|Optional|When `action` is set to **redirect**, specify the URL that is used in the redirect response.| 

### Output parameters
{: #lb-listener-policy-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id`|String|The ID of the load balancer listener policy. The ID is composed of `<lb_ID>/<listener_ID>/<policy_ID>`. |
|`status`|String|The status of the load balancer listener policy.|
|`policy_id`|String|The ID of the load balancer listener policy.|

### Import
{: #lb-listener-policy-import}

The resource can be imported by using the ID. The ID is composed of `<lb_ID>/<listener_ID>/<policy_ID>`.

```
terraform import ibm_is_lb_listener_policy.example <lb_ID>/<listener_ID>/<policy_ID>
```
{: pre}

## `ibm_is_lb_listener_policy_rule`
{: #lb-listener-policy-rule}

Create, update, or delete a VPC load balancer listener policy rule.
{: shortdesc}

### Sample Terraform code
{: #lb-listener-policy-rule-sample}

```
resource "ibm_is_lb" "lb2"{
  name    = "mylb"
  subnets = ["35860fed-c911-4936-8c94-f0d8577dbe5b"]
}

resource "ibm_is_lb_listener" "lb_listener2"{
  lb       = ibm_is_lb.lb2.id
  port     = "9086"
  protocol = "http"
}
resource "ibm_is_lb_listener_policy" "lb_listener_policy" {
  lb = ibm_is_lb.lb2.id
  listener = ibm_is_lb_listener.lb_listener2.listener_id
  action = "redirect"
  priority = 2
  name = "mylistener8"
  target_http_status_code = 302
  target_url = "https://www.redirect.com"
  rules{
      condition = "contains"
      type = "header"
      field = "1"
      value = "2"
  }
}

resource "ibm_is_lb_listener_policy_rule" "lb_listener_policy_rule" {
  lb        = ibm_is_lb.lb2.id
  listener  = ibm_is_lb_listener.lb_listener2.listener_id
  policy    = ibm_is_lb_listener_policy.lb_listener_policy.policy_id
  condition = "equals"
  type      = "header"
  field     = "MY-APP-HEADER"
  value     = "New-value"
}
```
{: codeblock}

### Input parameters
{: #lb-listener-policy-rule-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`lb`|String|Required|The ID of the load balancer for which you want to create a listener policy rule.|
|`listener`|String|Required|The ID of the load balancer listener for which you want to create a policy rule.| 
|`policy`|String|Required|The ID of the load balancer listener policy for which you want to create a policy rule.| 
|`condition`|String|Required|The condition that you want to apply to your rule. Supported values are `contains`, `equals`, and `matches_regex`.|
|`type`|String|Required|The object where you want to apply the rule. Supported values are `header`, `hostname`, and `path`.|
|`value`|String|Required|The value that must match the rule condition. The value can be between 1 and 128 characters long. | 
|`field`|String|Optional|If you set `type` to `header`, enter the HTTP header field where you want to apply the rule condition. |

### Output parameters
{: #lb-listener-policy-rule-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the load balancer listener policy rule. The ID is composed of ` <loadbalancer_ID>/<listener_ID>/<policy>ID>`. |
|`status`|String|The status of the load balancer listener.|
|`rule`|String|The ID of the rule|

### Import
{: #lb-listener-policy-rule-import}

You can import the rule by using the ID.

```
terraform import ibm_is_lb_listener_policy.example <loadbalancer_ID>/<listener_ID>/<policy>ID>
```
{: pre}

### Timeout
{: #lb-listener-policy-rule-timeout}

The following timeouts are configured for the resource: 

- **Create**: The creation of the resource is considered failed if no response is received for 10 minutes. 
- **Update**: The updaet of the resource is considered failed if no response is received for 10 minutes. 
- **Delete**: The deletion of the resource is considered failed if no response is received for 10 minutes. 


## `ibm_is_lb_pool`
{: #lb-pool}

Create, update, or delete a VPC load balancer pool. 
{: shortdesc}

### Sample Terraform code
{: #lb-pool-sample}

```
resource "ibm_is_lb_pool" "testacc_pool" {
  name           = "test_pool"
  lb             = "addfd-gg4r4-12345"
  algorithm      = "round_robin"
  protocol       = "http"
  health_delay   = 60
  health_retries = 5
  health_timeout = 30
  health_type    = "http"
}
```

### Input parameters
{: #lb-pool-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required| The name of the pool.|
|`lb` |String|Required|The load balancer unique identifier.|
|`algorithm`|String|Required|The load balancing algorithm. Supported values are `round_robin`, `weighted_round_robin`, or `least_connections`.|
|`protocol`|String|Required|The pool protocol. Supported values are `http`, and `tcp`.|
|`health_delay`|Integer|Required|The health check interval in seconds. Interval must be greater than `timeout` value.|
|`health_retries`|Integer|Required|The health check max retries.|
|`health_timeout`|Integer|Required|The health check timeout in seconds.|
|`health_type`|String|Required|The pool protocol. Supported values are `http`, and `tcp`.|
|`health_monitor_url`|String|Optional|The health check url. This option is applicable only to the HTTP `health-type`.|
|`health_monitor_port`|Integer|Optional|The health check port number.|
|`session_persistence_type`|String|Optional|The session persistence type. Only `source_ip` is supported.|

### Output parameters
{: #lb-pool-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the load balancer pool. The ID is composed of `<lb_id>/<pool_id>`.|
|`provisioning_status`|String| The status of load balancer pool.|

### Import
{: #lb-pool-import}

`ibm_is_lb_pool` can be imported by using the load balancer ID and pool ID. 

```
terraform import ibm_is_lb_pool.example <loadbalancer_ID>/<pool_ID>
```
{: pre}

### Timeouts
{: #lb-pool-timeout}

`ibm_is_lb_pool` provides the following timeouts:

- **create** - (Default 10 minutes) Used for creating Instance.
- **update** - (Default 10 minutes) Used for updating Instance.
- **delete** - (Default 10 minutes) Used for deleting Instance.

## `ibm_is_lb_pool_member`
{: #lb-pool-member}

Create, update, or delete a pool member for a VPC load balancer. 
{: shortdesc}

### Sample Terraform code
{: #lb-pool-member-sample}

```
resource "ibm_is_lb_pool_member" "testacc_lb_mem" {
  lb             = "daac2b08-fe8a-443b-9b06-1cef79922dce"
  pool           = "f087d3bd-3da8-452d-9ce4-c1010c9fec04"
  port           = 8080
  target_address = "127.0.0.1"
  weight         = 60
}
```

### Input parameters
{: #lb-pool-member-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`pool`|String|Required| The load balancer pool unique identifier.|
|`lb`|String|Required| The load balancer unique identifier.|
|`port`|Integer|Required| The port number of the application running in the server member.|
|`target_address`|String|Required|The IP address of the pool member.|
|`weight`|Integer|Optional| Weight of the server member. This option takes effect only when the load balancing algorithm of its belonging pool is `weighted_round_robin`.|

### Output parameters
{: #lb-pool-member-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the load balancer pool member.|
|`href`|String|The member’s canonical URL.|
|`health`|String|Health of the server member in the pool.|

### Import
{: #lb-pool-member-import}

`ibm_is_lb_pool_member` can be imported using the load balancer ID, pool ID, pool member ID.

```
terraform import ibm_is_lb_pool_member.example <loadbalancer_ID>/<pool_ID>/<pool_member_ID>
```
{: pre}

### Timeouts
{: #lb-pool-member-timeout}

`ibm_is_lb_pool_member` provides the following timeouts.

- **create** - (Default 10 minutes) Used for creating Instance.
- **update** - (Default 10 minutes) Used for updating Instance.
- **delete** - (Default 10 minutes) Used for deleting Instance.


## `ibm_is_network_acl`
{: #network-acl}

Create, update, or delete a network access control list (ACL). 
{: shortdesc}

### Sample Terraform code
{: #network-acl-sample}

```
resource "ibm_is_vpc" "testacc_vpc" {
  name = "vpctest"
}

resource "ibm_is_network_acl" "isExampleACL" {
  name = "is-example-acl"
  vpc  = ibm_is_vpc.testacc_vpc.id
  rules {
    name        = "outbound"
    action      = "allow"
    source      = "0.0.0.0/0"
    destination = "0.0.0.0/0"
    direction   = "outbound"
    icmp {
      code = 1
      type = 1
    }
  }
  rules {
    name        = "inbound"
    action      = "allow"
    source      = "0.0.0.0/0"
    destination = "0.0.0.0/0"
    direction   = "inbound"
    icmp {
      code = 1
      type = 1
    }
  }
}
```

### Input parameters
{: #network-acl-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
|`name`|String|Required|The name of the network ACL.|
|`vpc`|String|Required|The VPC ID. This parameter is note required if you want to create a network ACL for a Gen 1 VPC.|
|`resource_group`|String|Optional|The ID of the resource group where you want to create the network ACL. |
|`rules`|List of rules|Optional|A list of rules for a network ACL. The order in which the rules are added to the list determines the priority of the rules. For example, the first rule that you want to enforce must be specified as the first rule in this list. |
|`rules.name`|String|Required|The user-defined name for this rule.|
|`rules.action`|String|Required|`Allow` or `deny` matching network traffic. |
|`rules.source`|String|Required|The source IP address or CIDR block.|
|`rules.destination`|String|Required|The destination IP address or CIDR block.|
|`rules.direction`|String|Required|Indicates whether the traffic to be matched is `inbound` or `outbound`.|
|`rules.icmp`|List of protocol information|Optional|The protocol ICMP.|
|`rules.icmp.code`|Integer|Optional|The ICMP traffic code to allow. Valid values from 0 to 255. If unspecified, all codes are allowed. This can only be specified if type is also specified.|
|`rules.icmp.type`|Integer|Optional|The ICMP traffic type to allow. Valid values from 0 to 254. If unspecified, all types are allowed by this rule.|
|`rules.tcp`|List of protocol information|Optional|The TCP protocol.|
|`rules.tcp.port_max`|Integer|Optional|The highest port in the range of ports to be matched; if unspecified, 65535 is used.|
|`rules.tcp.port_min`|Integer|Optional|The lowest port in the range of ports to be matched; if unspecified, 1 is used.|
|`rules.tcp.source_port_max`|Integer|Optional|The highest port in the range of ports to be matched; if unspecified, 65535 is used.|
|`rules.tcp.source_port_min`|Integer|Optional|The lowest port in the range of ports to be matched; if unspecified, 1 is used.|
|`rules.udp`|List of protocol information|Optional|The UDP protocol.|
|`rules.udp.port_max`|Integer|Optional|The highest port in the range of ports to be matched; if unspecified, 65535 is used.|
|`rules.udp.port_min`|Integer|Optional|The lowest port in the range of ports to be matched; if unspecified, 1 is used.|
|`rules.udp.source_port_max`|Integer|Optional|The highest port in the range of ports to be matched; if unspecified, 65535 is used.|
|`rules.udp.source_port_min`|Integer|Optional|The lowest port in the range of ports to be matched; if unspecified, 1 is used.|

### Output parameters
{: #network-acl-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id`|String|The ID of the network ACL.|
|`rules`|List of rules |The rules for a network ACL.| 
|`rules.id`|String|The rule ID.|
|`rules.ip_version`|String|The IP version of the rule.|
|`rules.subnets`|String|The subnets for the ACL rule.|
|`resource_controller_url`|String|The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.|

### Import
{: #network-acl-import}

The resource can be imported by using the network ACL ID. 

```
terraform import ibm_is_network_acl.example <network_acl_id>
```
{: pre}

## `ibm_is_public_gateway`
{: #provider-public-gateway}

Create, update, or delete a public gateway for a VPC subnet. 
{: shortdesc}

Public gateways enable a VPC subnet and all the instances that are connected to the subnet to connect to the internet. For more information, see [Use a Public Gateway for external connectivity of a subnet](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-about-networking-for-vpc#use-a-public-gateway). 

To attach a public gateway that you created to a subnet, use the `public_gateway` input parameter in the [`ibm_is_subnet` resource](#subnet).
{: note}

### Sample Terraform code
{: #public-gateway-sample}

The following example shows how you can create a public gateway for all the subnets that are located in a specific zone. 

```
resource "ibm_is_vpc" "testacc_vpc" {
    name = "test"
}

resource "ibm_is_public_gateway" "testacc_gateway" {
    name = "test_gateway"
    vpc = ibm_is_vpc.testacc_vpc.id
    zone = "us-south-1"

    //User can configure timeouts
    timeouts {
        create = "90m"
    }
}
```
{: codeblock}

### Input parameters
{: #public-gateway-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `name` | String| Required | Enter a name for your public gateway. |
| `vpc` | String | Required | Enter the ID of the VPC, for which you want to create a public gateway. To list available VPCs, run `ibmcloud is vpcs`.  | 
| `zone` | String | Required | Enter the zone where you want to create the public gateway. To list available zones, run `ibmcloud is zones`. |
| `resource_group`|String|Optional|Enter the ID of the resource group where you want to create the public gateway. To list available resource groups, run `ibmcloud resource groups`. If you do not specify a resource group, the public gateway is created in the `default` resource group. |
| `tags`|Array of strings|Optional|Enter any tags that you want to associate with your VPC. Tags might help you find your VPC more easily after it is created. Separate multiple tags with a comma (`,`). |

### Output parameters
{: #public-gateway-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `floating_ip` | List | A collection of floating IP addresses that are bound to the public gateway. Every floating IP address is listed with the floating IP `id` and `address`. |
| `id` | String | The unique identifier that was assigned to your public gateway. |
| `resource_controller_url` | String | The URL of the {{site.data.keyword.cloud_notm}} dashboard that you can use to explore and view details about the public gateway. | 
| `status` | String | The provisioning status of your public gateway. |

### Timeouts
{: #public-gateway-timeout}

The following timeouts are defined for this resource. 
{: shortdesc}

- **create**: The creation of the public gateway is considered `failed` when no response is received for 10 minutes. 
- **delete**: The deletion of the public gateway is considered `failed` when no response is received for 10 minutes.

## `ibm_is_security_group`
{: #sec-group}

Create, update, or delete a security group for your VPC. 
{: shortdesc}

When you want to create a security group and security group rule for a virtual server instance in your VPC, you must create these resources in a specific order to avoid errors during the creation of your virtual server instance. For an example, see [Example for creating an instance with custom security group rules](#custom-sec-group-rules). 
{: note}


### Sample Terraform code
{: #sec-group-sample}

```
resource "ibm_is_vpc" "testacc_vpc" {
	name = "test"
}

resource "ibm_is_security_group" "testacc_security_group" {
	name = "test"
	vpc = ibm_is_vpc.testacc_vpc.id
}
```

### Input parameters
{: #sec-group-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Optional|The security group name.|
|`vpc`|String|Required|The VPC ID. |
|`resource_group`|String|Optional|The resource group ID where the security group to be created.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #sec-group-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the security group.|
|`rules`|List of objects|A nested block describing the rules of this security group. |
|`rules.direction`| String|The direction of the traffic either `inbound` or `outbound`.  |
|`rules.ip_version`|String|IP version either `ipv4` or `ipv6`.  |
|`rules.remote`|String|Security group id, an IP address, a CIDR block, or a single security group identifier.  |
|`rules.protocol`|String|The type of the protocol `all`, `icmp`, `tcp`, `udp`.   |
|`rules.type`|String|The ICMP traffic type to allow.  |
|`rules.code`|String|The ICMP traffic code to allow.  |
|`rules.port_max`|Integer|The inclusive upper bound of TCP/UDP port range.  |
|`rules.port_min`|Integer|The inclusive lower bound of TCP/UDP port range. |
|`resource_controller_url`|String|The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.   |
{: caption="Table 1. Available output parameters" caption-side="top"}


### Import
{: #sec-group-import}

`ibm_is_security_group` can be imported using load balancer ID. 

```
terraform import ibm_is_security_group.example a1aaa111-1111-111a-1a11-a11a1a11a11a
```
{: pre}


## `ibm_is_security_group_rule`
{: #sec-group-rule}

Create, update, or delete a security group rule. 
{: shortdesc}

When you want to create a security group and security group rule for a virtual server instance in your VPC, you must create these resources in a specific order to avoid errors during the creation of your virtual server instance. For an example, see [Example for creating an instance with custom security group rules](#custom-sec-group-rules). 
{: note}

### Sample Terraform code
{: #sec-group-rule-sample}

In the following example, you create a different type of protocol rules `ALL`, `ICMP`, `UDP` and `TCP`.

```
resource "ibm_is_vpc" "testacc_vpc" {
	name = "test"
}

resource "ibm_is_security_group" "testacc_security_group" {
	name = "test"
	vpc = ibm_is_vpc.testacc_vpc.id
}

resource "ibm_is_security_group_rule" "testacc_security_group_rule_all" {
	group = ibm_is_security_group.testacc_security_group.id
	direction = "inbound"
	remote = "127.0.0.1"
 }
 
 resource "ibm_is_security_group_rule" "testacc_security_group_rule_icmp" {
	group = ibm_is_security_group.testacc_security_group.id
	direction = "inbound"
	remote = "127.0.0.1"
	icmp {
		code = 20
		type = 30
	}

 }

 resource "ibm_is_security_group_rule" "testacc_security_group_rule_udp" {
	group = ibm_is_security_group.testacc_security_group.id
	direction = "inbound"
	remote = "127.0.0.1"
	udp {
		port_min = 805
		port_max = 807
	}
 }

 resource "ibm_is_security_group_rule" "testacc_security_group_rule_tcp" {
	group = ibm_is_security_group.testacc_security_group.id
	direction = "outbound"
	remote = "127.0.0.1"
	tcp {
		port_min = 8080
		port_max = 8080
	}
 }
```

### Input parameters
{: #sec-group-rule-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`group`|String|Required|The security group ID.|
|`direction`|String|Required|The direction of the traffic either `inbound` or `outbound`.|
|`remote`|String|Optional|Security group id, an IP address, a CIDR block, or a single security group identifier.|
|`ip_version`|String|Optional|The IP version either `IPv4` or `IPv6`. Default `IPv4`.|
|`icmp`|List of objects|Optional|A nested block describing the `icmp` protocol of this security group rule.  |
|`icmp.type`|Integer|Required|The ICMP traffic type to allow. Valid values from 0 to 254.  |
|`icmp.code`|Integer|Optional|The ICMP traffic code to allow. Valid values from 0 to 255.|
|`tcp`|List of objects|Optional|A nested block describing the `tcp` protocol of this security group rule.  |
|`tcp.port_min`|Integer|Required| The inclusive lower bound of TCP port range. Valid values are from 1 to 65535.  |
|`tcp.port_max`|Integer|Required| The inclusive upper bound of TCP port range. Valid values are from 1 to 65535.|
|`udp`|List of objects| Optional| A nested block describing the `udp` protocol of this security group rule.  |
|`udp.port_min`|Integer|Required|The inclusive lower bound of UDP port range. Valid values are from 1 to 65535.  |
|`udp.port_max`|Integer|Required|The inclusive upper bound of UDP port range. Valid values are from 1 to 65535. **NOTE**: If any of the `icmp` , `tcp` or `udp` is not specified it creates a rule with protocol `ALL`. |
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #sec-group-rule-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the security group rule. The ID is composed of `<security_group_id>/<security_group_rule_id>`.|
|`rule_id`|String|The unique identifier of the rule.|
{: caption="Table 1. Available output parameters" caption-side="top"}

### Import
{: #sec-group-rule-import}

`ibm_is_security_group_rule` can be imported using security group ID and security group rule ID.

```
terraform import ibm_is_security_group_rule.example d7bec597-4726-451f-8a63-e62e6f19c32c/cea6651a-bc0a-4438-9f8a-a0770bbf3ebb
```
{: pre}


## `ibm_is_security_group_network_interface_attachment`
{: #sec-group-netint}

Create, update, or delete a security group network interface attachment. 
{: shortdesc}


### Sample Terraform code
{: #sec-group-netint-sample}

```
resource "ibm_is_security_group_network_interface_attachment" "sgnic" {
  security_group    = "2d364f0a-a870-42c3-a554-000001352417"
  network_interface = "6d6128aa-badc-45c4-bb0e-7c2c1c47be55"
}
```

### Input parameters
{: #sec-group-netint-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`security_group`|String|Required|The security group ID.|
|`network_interface`|String|Required|The network interface ID. |
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #sec-group-netint-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the security group network interface. The ID is composed of `<security_group_id>/<network_interface_id>`.|
|`instance_network_interface`|String|The instance network interface ID.|
|`name`|String|The user-defined name for this network interface.|
|`port_speed`|Integer|The network interface port speed in Mbps.|
|`primary_ipv4_address`|String|The primary IPv4 address.|
|`primary_ipv6_address`|String|The primary IPv6 address in compressed notation as specified by RFC 5952.|
|`secondary_address`|Array|Collection secondary IP addresses.|
|`status`|String|The status of the volume.|
|`subnet`|String|The Subnet ID.|
|`type`|String|The type of this network interface as it relates to a instance.|
|`security_groups`|List of objects|A nested block describing the security groups of this network interface.|
|`security_groups.id`|String|The ID of this security group.	|
|`security_groups.crn`|String|The CRN of this security group.	|
|`security_groups.name`|String|The name of this security group.|
|`floating_ips`|List of objects|A nested block describing the floating IPs of this network interface. |
|`floating_ips.id`|String|The ID of this floating IP.  |
|`floating_ips.crn`|String|The CRN of this floating IP.  |
|`floating_ips.name`|String|The name of this floating IP.  |
|`floating_ips.address`|String|The globally unique IP address|
{: caption="Table 1. Available output parameters" caption-side="top"}


### Import
{: #sec-group-netint-import}

`ibm_is_security_group_network_interface_attachment` can be imported using security group ID and network interface ID.

```
terraform import ibm_is_security_group_network_interface_attachment.example <security_group_ID>/<network_interface_ID>
```

## `ibm_is_subnet`
{: #subnet}

Create, update, or delete a subnet.
{: shortdesc}

### Sample Terraform code
{: #subnet-sample}

```
resource "ibm_is_vpc" "testacc_vpc" {
	name = "test"
}

resource "ibm_is_subnet" "testacc_subnet" {
	name = "test_subnet"
	vpc = ibm_is_vpc.testacc_vpc.id
	zone = "us-south-1"
	ipv4_cidr_block = "192.168.0.0/1"

	//User can configure timeouts
  	timeouts {
      	create = "90m"
      	delete = "30m"
    }
}
```

### Input parameters
{: #subnet-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`ipv4_cidr_block`|String|Optional|The IPv4 range of the subnet.|
|`total_ipv4_address_count`|String|Optional|The total number of IPv4 addresses.|
|`ip_version`|String|Optional|The IP Version. The default is `ipv4`.|
|`name`|String|Required| The name of the subnet.|
|`network_acl`|String|Optional|The ID of the network ACL for the subnet.|
|`public_gateway`|String|Optional|The ID of the public gateway for the subnet that you want to attach to the subnet. You create the public gateway with the [`ibm_is_public_gateway` resource](#provider-public-gateway).|
|`resource_group`|String|Optional|The ID of the resource group where you want to create the subnet.|
|`vpc`|String|Required|The VPC ID.|
|`zone`|String|Required|The subnet zone name.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #subnet-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the subnet.|
|`ipv6_cidr_block`|String|The IPv6 range of the subnet.|
|`status`|String|The status of the subnet.|
|`available_ipv4_address_count`|String|The total number of available IPv4 addresses.|
|`resource_controller_url`|String|The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.|
{: caption="Table 1. Available output parameters" caption-side="top"}

### Import
{: #subnet-import}

`ibm_is_subnet` can be imported using the ID. 

```
terraform import ibm_is_subnet.example <subnet_ID>
```
{: pre}


### Timeouts
{: #subnet-timeout}

`ibm_is_subnet` provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

|Name|Description|
|----|-----------|
|`create`|(Default 10 minutes) Used for creating Instance.|
|`update`|(Default 10 minutes) Used for creating Instance.|
|`delete`|(Default 10 minutes) Used for deleting Instance.|
{: caption="Table. Available timeout configuration options" caption-side="top"}


## `ibm_is_ssh_key`
{: #ssh-key}

Create, update, or delete an SSH key. The SSH key is used to access a Gen 2 virtual server instance.
{: shortdesc}


### Sample Terraform code
{: #ssh-key-sample}

```
resource "ibm_is_ssh_key" "isExampleKey" {
	name = "test_key"
	public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCKVmnMOlHKcZK8tpt3MP1lqOLAcqcJzhsvJcjscgVERRN7/9484SOBJ3HSKxxNG5JN8owAjy5f9yYwcUg+JaUVuytn5Pv3aeYROHGGg+5G346xaq3DAwX6Y5ykr2fvjObgncQBnuU5KHWCECO/4h8uWuwh/kfniXPVjFToc+gnkqA+3RKpAecZhFXwfalQ9mMuYGFxn+fwn8cYEApsJbsEmb0iJwPiZ5hjFC8wREuiTlhPHDgkBLOiycd20op2nXzDbHfCHInquEe/gYxEitALONxm0swBOwJZwlTDOB7C6y2dzlrtxr1L59m7pCkWI4EtTRLvleehBoj3u7jB4usR"
}
```

### Input parameters
{: #ssh-key-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required| The user-defined name for this key.|
|`public_key`|String|Required|The public SSH key.|
|`resource_group`|String|Optional|The resource group ID where the SSH is created.|
|`tags`|List of strings|A list of tags that you want to add to your SSH key. Tags can help you find the SSH key more easily later. |
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #ssh-key-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the SSH key.|
|`fingerprint`| String|The SHA256 fingerprint of the public key.|
|`length`|String|The length of this key.|
|`type`|String|The crypto system used by this key.|
|`resource_controller_url`|String|The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.|
{: caption="Table 1. Available output parameters" caption-side="top"}

### Import

`ibm_is_ssh_key` can be imported using the SSH key ID. 

```
terraform import ibm_is_ssh_key.example <ssh_key_ID>
```
{: pre}


## `ibm_is_volume`
{: #volume}

Create, update, or delete a VPC block storage volume. 
{: shortdesc}

### Sample Terraform code
{: #volume-sample}

The following example creates a volume with 10 IOPS. 

```
resource "ibm_is_volume" "testacc_volume" {
  name     = "test_volume"
  profile  = "10iops-tier"
  zone     = "us-south-1"
  iops     = 10000
  capacity = 100
}

```

The following example creates a custom volume. 

```
resource "ibm_is_volume" "testacc_volume" {
  name     = "test_volume"
  profile  = "custom"
  zone     = "us-south-1"
  iops     = 1000
  capacity = 200
}
```

### Input parameters
{: #volume-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required|The user-defined name for this volume.|
|`profile`|String|Required|The profile to use for this volume.|
|`zone`|String|Required|The location of the volume.|
|`iops`|Integer|Required for `custom` storage profiles only| The total input/ output operations per second (IOPS) for your storage. This value is required for `custom` storage profiles only. |
|`capacity`|Integer|Optional|(The capacity of the volume in gigabytes. This defaults to `100`.|
|`encryption_key`|String|Optional|The key to use for encrypting this volume.|
|`resource_group`|String|Optional|The resource group ID for this volume.|
|`resource_controller_url`|String|Optional|The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.|
|`tags`|List of strings|A list of tags that you want to add to your volume. Tags can help you find your volume more easily later.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #volume-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the volume.|
|`status`|String|The status of volume.|
|`crn`|String|The CRN for the volume.|
{: caption="Table 1. Available output parameters" caption-side="top"}


### Timeouts
{: #volume-timeout}

`ibm_is_volume` provides the following timeouts:

|Name|Description|
|----|-----------|
|`create`|(Default 10 minutes) Used for Creating Instance.|
|`delete`|(Default 10 minutes) Used for Deleting Instance.|
{: caption="Table. Available timeout configuration options" caption-side="top"}

## `ibm_is_vpc` 
{: #provider-vps}

Create, update, or delete a Virtual Private Cloud (VPC). VPCs allow you to create your own space in {{site.data.keyword.cloud_notm}} to run an isolated environment within the public cloud. VPC gives you the security of a private cloud, with the agility and ease of a public cloud.
{: shortdesc}

For more information, see [About Virtual Private Cloud](/docs/vpc-on-classic?topic=vpc-on-classic-about). 

### Sample Terraform code
{: #vpc-sample}

```
resource "ibm_is_vpc" "testacc_vpc" {
    name = "test"
}
```
{: codeblock}

### Input parameters
{: #vpc-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `classic_access` | Boolean | Optional | Specify if you want to create a VPC that can connect to classic infrastructure resources. Enter **true** to set up private network connectivity from your VPC to classic infrastructure resources that are created in the same {{site.data.keyword.cloud_notm}} account, and **false** to disable this access. If you choose to not set up this access, you cannot enable it after the VPC is created. Make sure to review the [prerequisites](/docs/vpc-on-classic-network?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc#vpc-prerequisites) before you create a VPC with classic infrastructure access. Note that you can enable one VPC for classic infrastructure access per {{site.data.keyword.cloud_notm}} account only. |
|`address_prefix_management`|String|Optional|Indicates whether a default address prefix should be created automatically (`auto`) or manually (`manual`) for each zone in this VPC. Default value `auto`.|
| `name` | String | Required | Enter a name for your VPC. | 
| `resource_group` | String | Optional | Enter the ID of the resource group where you want to create the VPC. To list available resource groups, run `ibmcloud resource groups`. If you do not specify a resource group, the VPC is created in the `default` resource group. | 
| `tags` | Array of Strings | Optional | Enter any tags that you want to associate with your VPC. Tags might help you find your VPC more easily after it is created. Separate multiple tags with a comma (`,`). | 

### Output parameters
{: #vpc-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`crn`|String|The CRN of the VPC.|
| `default_security_group` | String | The unique identifier of the default security group that was created for your VPC. | 
| `id` | String | The unique identifier of the VPC that you created. |
| `resource_controller_url` | String | The URL of the {{site.data.keyword.cloud_notm}} dashboard that you can use to explore and view details about the VPC. |
|`subnets`|List of subnets|A list of subnets that are attached to a VPC.|
|`subnets.name`|String|The name of the subnet.|
|`subnets.id`|String|The ID of the subnet.|
|`subnets.status`|String|The status of the subnet.|
|`subnets.total_ipv4_address_count`|Integer|The total number of IPv4 addresses in the subnet.|
|`subnets.available_ipv4_address_count`|Integer|The number of IPv4 addresses in the subnet that are available for you to be used.|
| `status` | String | The provisioning status of your VPC. | 
| `cse_source_addresses`|List of Cloud Service Endpoints|A list of the cloud service endpoints that are associated with your VPC, including their source IP address and zone.|
|`cse_source_addresses.address`|String|The IP address of the cloud service endpoint.|
|`cse_source_addresses.zone_name`|String|The zone where the cloud service endpoint is located.|

### Timeout
{: #vpc-timeout}

The following timeouts are specified for this resource: 

- **create**: The creation of the VPC is considered `failed` when no response is received for 10 minutes.  
- **delete**: The deletion of the VPC is considered `failed` when no response is received for 10 minutes.  

## `ibm_is_vpc_address_prefix`
{: #address-prefix}

Create, update, or delete an IP address prefix. 
{: shortdesc}

### Sample Terraform code
{: #address-prefix-sample}

```
resource "ibm_is_vpc" "testacc_vpc" {
  name = "testvpc"
}

resource "ibm_is_vpc_address_prefix" "testacc_vpc_address_prefix" {
  name = "test"
  zone   = "us-south-1"
  vpc         = ibm_is_vpc.testacc_vpc.id
  cidr        = "10.240.0.0/24"
}

```

### Input parameters
{: #address-prefix-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required| The address prefix name.|
|`vpc`|String|Required|The VPC ID. |
|`zone`|String|Required|The name of the zone. |
|`cidr`|String|Required|The CIDR block for the address prefix. |
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #address-prefix-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the address prefix.|
|`has_subnets`|Boolean|Indicates whether subnets exist with addresses from this prefix.|
{: caption="Table 1. Available output parameters" caption-side="top"}


### Import
{: #address-prefix-import}

`ibm_is_vpc_address_prefix` can be imported using the ID.

```
terraform import ibm_is_vpc_address_prefix.example a1aaa111-1111-111a-1a11-a11a1a11a11a
```

## `ibm_is_vpc_route`
{: #vpc-route}

Create, update, or delete a VPC route. For more information about VPC routes, see [Setting up advanced routing in VPC](/docs/vpc?topic=vpc-advanced-routing).
{: shortdesc}

### Sample Terraform code
{: #vpc-route-sample}

```
resource "ibm_is_vpc" "myvpc" {
  name = "myvpc"
}

resource "ibm_is_vpc_route" "myroute" {
  name        = "routetest"
  vpc         = ibm_is_vpc.myvpc.id
  zone        = "us-south-1"
  destination = "192.168.4.0/24"
  next_hop    = "10.0.0.4"
}
```

### Input parameters
{: #vpc-route-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required|The name of the route that you want to create.| 
|`vpc`|String|Requied|The ID of the VPC where you want to create the route. | 
|`zone`|String|Required|The name of the VPC zone where you want to create the route.|
|`destination`|String|Required|The destination IP address or CIDR that network traffic from your VPC must match to be routed to the `next_hop`.|
|`next_hop`|String|Required|The IP address where network traffic is sent next.|

### Output parameters
{: #vpc-route-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the VPC route. The ID is composed of `<vpc_id>/<vpc_route_id>`.|
|`status`|Strign|The status of the VPC route.|

### Import
{: #vpc-route-import}

The resource can be imported by using the VPC and route IDs. 

```
terraform import ibm_is_vpc_route.example <vpc_ID>/<vpc_route_ID>
```
{: pre}

### Timeouts
{: #vpc-route-timeout}

The resource is set up with the following timeouts: 

- **create**: The creation of the route is considered `failed` when no response is received for 10 minutes. 
- **delete**: The deletion of the route is considered `failed` when no response is received for 10 minutes. 


## `ibm_is_vpn_gateway`
{: #vpn-gateway}

Create, update, or delete a VPC gateway. 
{: shortdesc}

### Sample Terraform code
{: #vpn-gateway-sample}

```
resource "ibm_is_vpn_gateway" "testacc_vpn_gateway" {
  name   = "test"
  subnet = "a1aa111a-a11a-1111-11aa-111a1aa1aaa1"
}
```

### Input parameters
{: #vpn-gateway-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required|The name of the VPN gateway.|
|`subnet`|String|Required|The unique identifier for this subnet.|
|`resource_group`|String|Optional| The resource group where the VPN gateway to be created.|
|`tags`|List of strings|Optional|A list of tags that you want to add to your VPN gatway. Tags can help you find your VPN gateway more easily later.|

### Output parameters
{: #vpn-gateway-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the VPN gateway.|
|`status`|String|The status of VPN gateway.|
|`public_ip_address`|String|The IP address assigned to this VPN gateway.|
|`resource_controller_url`|String|The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.|

### Import
{: #vpn-gateway-import}

`ibm_is_vpn_gateway` can be imported using the VPN gateway ID. 

```
terraform import ibm_is_vpn_gateway.example <vpn_gateway_ID>
```
{: pre}

### Timeouts
{: #vpn-gateway-timeout}

The following timeouts are specified for this resource: 

- **create**: The creation of the VPN gateway is considered `failed` when no response is received for 10 minutes. 
- **delete**: The deletion of the VPN gateway is considered `failed` when no response is received for 10 minutes. 

## `ibm_is_vpn_gateway_connection`
{: #vpn-gateway-connection}

Create, update, or delete a VPN gateway connection. 
{: shortdesc}

### Sample Terraform code
{: #vpn-gateway-connection-sample}

```
resource "ibm_is_vpn_gateway_connection" "VPNGatewayConnection" {
  name          = "test2"
  vpn_gateway   = ibm_is_vpn_gateway.testacc_VPNGateway2.id
  peer_address  = ibm_is_vpn_gateway.testacc_VPNGateway2.public_ip_address
  preshared_key = "VPNDemoPassword"
  local_cidrs = [ibm_is_subnet.testacc_subnet2.ipv4_cidr_block]
  peer_cidrs = [ibm_is_subnet.testacc_subnet1.ipv4_cidr_block]
}
```

### Input parameters
{: #vpn-gateway-connection-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required|The name of the VPN gateway connection.|
|`vpn_gateway`|String|Required| The unique identifier of the VPN gateway.|
|`peer_address`|String|Required|The IP address of the peer VPN gateway.|
|`preshared_key`|String|Required| The preshared key.|
|`local_cidrs`|Array|Optional|List of local CIDRs for this resource.|
|`peer_cidrs`|Array|Optional|List of peer CIDRs for this resource.|
|`admin_state_up`|Boolean|Optional|The VPN gateway connection status. Default false. If set to false, the VPN gateway connection is shut down.|
|`action`|String|Optional| Dead peer detection actions. Supported values are `restart`, `clear`, `hold`, `none`. Default: `none`.|
|`interval`|Integer|Optional| Dead peer detection interval in seconds. Default 30.|
|`timeout`|Integer|Optional| Dead peer detection timeout in seconds. Default 120.|
|`ike_policy`|String|Optional|The ID of the IKE policy.|
|`ipsec_policy`|String|Optional| The ID of the IPSec policy.|

### Output parameters
{: #vpn-gateway-connection-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the VPN gateway connection. The ID is composed of `<vpn_gateway_id>/<vpn_gateway_connection_id>`.|
|`status`|String|The status of the VPN gateway connection.|

### Import
{: #vpn-gateway-connection-import}

`ibm_is_vpn_gateway_connection` can be imported using the VPN gateway ID and the VPN gateway connection ID. 

```
terraform import ibm_is_vpn_gateway_connection.example <vpn_gateway_ID>/<vpn_gateway_connection_ID>
```
{: pre}

### Timeouts
{: #vpn-gateway-connection-timeout}

`ibm_is_vpn_gateway_connection` provides the following Timeouts:

- **delete** - (Default 10 minutes) Used for deleting Instance.




