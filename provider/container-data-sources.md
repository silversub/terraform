---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-17"
 
keywords: terraform provider plugin, terraform kubernetes service, terraform container service, terraform cluster, terraform worker nodes, terraform iks, terraform kubernetes

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


# Kubernetes Service data sources
{: #container-data-sources}

You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 

Before you start working with your data source, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## `ibm_container_cluster`
{: #container-cluster}

Retrieve information about an existing {{site.data.keyword.containerlong_notm}} cluster. 
{: shortdesc}

### Sample Terraform code
{: #container-cluster-sample}

The following example retrieves information about a cluster that is named `mycluster`. 

```
data "ibm_container_cluster" "cluster" {
  cluster_name_id = "mycluster"
}
```

### Input parameters
{: #container-cluster-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `cluster_name_id` | String | Required | The name or ID of the cluster that you want to retrieve.  |
| `resource_group_id` | String | Optional | The ID of the resource group where your cluster is provisioned into. To list resource groups, run `ibmcloud resource groups` or use the `ibm_resource_group` data source.|

### Output parameters
{: #container-cluster-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `albs` | List of objects | A list of Ingress application load balancers (ALBs) that are attached to the cluster. |
| `albs.id` | String | The unique identifier of the Ingress ALB. |
| `albs.name` | String | The name of the Ingress ALB. |
| `albs.alb_type` | String | The type of ALB. Supported values are `public` and `private`. 
| `albs.enable` | Boolean | Indicates if the ALB is enabled (**true**) or disabled (**false**) in the cluster. |
| `albs.state` | String | The state of the ALB. Supported values are `enabled` or `disabled`. | 
| `albs.num_of_instances`| Integer | The number of ALB replicas. | 
| `albs.alb_ip` | String | BYOIP VIP to use for application load balancer (ALB). Currently supported only for private application load balancer (ALB).| 
| `albs.resize` | Boolean |  Indicate whether resizing should be done. | 
| `albs.disable_deployment` | Boolean |  Indicate whether to disable deployment only on disable application load balancer (ALB).|
| `bounded_services` | List of strings | A list of {{site.data.keyword.cloud_notm}} services that are bounded to the cluster. |
| `crn` | String | The CRN of the cluster. |
| `id` | String | The unique identifier of the cluster. |
|`ingress_hostname`|String|The Ingress host name.|
|`ingress_secret`|String|The name of the Ingress secret.|
| `name` | String | The name of the cluster. |
| `public_service_endpoint` | Boolean | Indicates if the public service endpoint is enabled (**true**) or disabled (**false**) for a cluster. | 
| `public_service_endpoint_url` | String | The URL of the public service endpoint for your cluster. |
| `private_service_endpoint` | Boolean | Indicates if the private service endpoint is enabled (**true**) or disabled (**false**) for a cluster. | 
| `private_service_endpoint_url` | String | The URL of the private service endpoint for your cluster.|
| `vlans`| List of objects | A list of VLANs that are attached to the cluster. | 
| `vlans.id` | String | The ID of the VLAN. | 
| `vlans.subnets` | List of objects | A list of subnets that belong to the cluster. |
| `vlans.subnets.id` | String | The ID of the subnet. | 
| `vlans.subnets.cidr` | String | The IP address CIDR of the subnet. |
| `vlans.subnets.ips` | List of strings | The IP addresses that belong to the subnet. |
| `vlans.subnets.isbyoip`| Boolean | If set to **true**, you provided your own IP address range for the subnet. If set to **false**, the default IP address range is used. |
| `vlans.subnets.is_public` | Boolean | If set to **ture**, the VLAN is public. If set to **false**, the VLAN is private. | 
| `workers` | List of objects | A list of worker nodes that belong to the cluster. | 
| `worker_pools` | List of objects | A list of worker pools that exist in the cluster.|
| `worker_pools.machine_type` | String | The machine type that is used for the worker nodes in the worker pool. |
| `worker_pools.name` | String | The name of the worker pool. |
| `worker_pools.size_per_zone` | Integer | The number of worker nodes per zone. |
| `worker_pools.hardware` | String | The level of hardware isolation that is used for the worker node of the worker pool.|
| `worker_pools.id` | String | The ID of the worker pool. |
| `worker_pools.state` | String | The state of the worker pool. |
| `worker_pools.labels` | List of strings | A list of labels that are added to the worker pool. |
| `worker_pools.zones` | List of objects | A list of zones that are attached to the worker pool. |
| `worker_pools.zones.zone` | String | The name of the zone. |
| `worker_pools.zones.private_vlan` | String | The ID of the private VLAN that is used in that zone. |
| `worker_pools.zones.public_vlan` | String | The ID of the private VLAN that is used in that zone. |
| `worker_pools.zones.worker_count` | Integer | The number of worker nodes that are attached to the zone. |
| `workers_info` | List of objects | A list of worker nodes that belong to the cluster. |
| `workers_info.pool_name` | String | The name of the worker pool the worker node belongs to. | 


## `ibm_container_cluster_config`
{: #container-cluster-config}

Download the Kubernetes configuration files and certificates to access your cluster. 
{: shortdesc}

### Sample Terraform code
{: #container-cluster-config-sample}

```
data "ibm_container_cluster_config" "cluster_foo" {
  cluster_name_id = "mycluster"
  config_dir      = "/home/mycluster_config"
}
```

#### Example for downloading the TLS certificates and permission files for the cluster administrator in a classic or VPC  {{site.data.keyword.containerlong_notm}} cluster

```
data "ibm_container_cluster_config" "mycluster" {
  cluster_name_id = "mycluster"
  admin           = true
}

provider "kubernetes" {
  load_config_file       = "false"
  host                   = data.ibm_container_cluster_config.mycluster.host
  client_certificate     = data.ibm_container_cluster_config.mycluster.admin_certificate
  client_key             = data.ibm_container_cluster_config.mycluster.admin_key
  cluster_ca_certificate = data.ibm_container_cluster_config.mycluster.ca_certificate
}

resource "kubernetes_namespace" "example" {
  metadata {
    name = "terraform-example-namespace"
  }
}
```

#### Example for connecting to the cluster by using the cluster host and token in a classic or VPC  {{site.data.keyword.containerlong_notm}} cluster

```
data "ibm_container_cluster_config" "mycluster" {
  cluster_name_id = "mycluster"
}

provider "kubernetes" {
  load_config_file       = "false"
  host                   = data.ibm_container_cluster_config.mycluster.host
  token                  = data.ibm_container_cluster_config.mycluster.token
  cluster_ca_certificate = data.ibm_container_cluster_config.mycluster.ca_certificate
}

resource "kubernetes_namespace" "example" {
  metadata {
    name = "terraform-example-namespace"
  }
}
```

#### Example for downloading the TLS certificates and permission files for the cluster administrator in a classic {{site.data.keyword.openshiftlong_notm}} cluster

```
data "ibm_container_cluster_config" "mycluster" {
  cluster_name_id = "mycluster"
  admin           = true
}

provider "kubernetes" {
  load_config_file       = "false"
  host                   = data.ibm_container_cluster_config.mycluster.host
  client_certificate     = data.ibm_container_cluster_config.mycluster.admin_certificate
  client_key             = data.ibm_container_cluster_config.mycluster.admin_key
}

resource "kubernetes_namespace" "example" {
  metadata {
    name = "terraform-example-namespace"
  }
}
```

#### Example for connecting to the cluster by using the cluster host and token in a classic {{site.data.keyword.openshiftlong_notm}} cluster

```
data "ibm_container_cluster_config" "mycluster" {
  cluster_name_id = "mycluster"
}

provider "kubernetes" {
  load_config_file       = "false"
  host                   = data.ibm_container_cluster_config.mycluster.host
  token                  = data.ibm_container_cluster_config.mycluster.token
}

resource "kubernetes_namespace" "example" {
  metadata {
    name = "terraform-example-namespace"
  }
}
```

### Input parameters
{: #container-cluster-config-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `admin` | Boolean | Optional | If set to **true**, the Kubernetes configuration for cluster administrators is downloaded. The default is **false**. |
| `cluster_name_id` | String | Required | The name or ID of the cluster that you want to log in to. | 
| `config_dir` | String | Required | The directory on your local machine where you want to download the Kubernetes config files and certificates. |
| `download` | Boolean | Optional | Set the value to **false** to skip downloading the configuration for the administrator. The default value is **true**. The configuration files and certificates are downloaded to the directory that you specified in `config_dir` every time that you run your infrastructure code.|
| `network` | Boolean | Optional | If set to **true**, the Calico configuration file, TLS certificates, and permission files that are required to run `calicoctl` commands in your cluster are downloaded in addition to the configuration files for the administrator. The default value is **false**. | 
| `resource_group_id` | String | Optional | The ID of the resource group where your cluster is provisioned into. To find the resource group, run `ibmcloud resource groups` or use the `ibm_resource_group` data source. If this parameter is not provided, the `default` resource group is used. |


### Output parameters
{: #container-cluster-config-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `calico_config_file_path` | String | The path on your local machine where your Calico configuration files and certificates are downloaded to. |
| `config_file_path` | String | The path on your local machine where the cluster configuration file and certificates are downloaded to.| 
| `id` | String | The unique identifier of the cluster configuration. |
| `admin_key`|String|The admin key of the cluster configuration. Note that this key is case sensitive. |
|`admin_certificate`|String|The admin certificate of the cluster configuration.|
|`ca_certificate`|String|The cluster CA certificate of the cluster configuration.|
|`host`|String|The host name of the cluster configuration.|
|`token`|String|The token of the cluster configuration.|


## `ibm_container_cluster_worker`
{: #container-worker}

Retrieve information about the worker nodes of your {{site.data.keyword.containerlong_notm}} cluster. 
{: shortdesc}

### Sample Terraform code
{: #container-worker-sample}

The following example retrieves information about a worker node with the ID `dal10-1112222abd111222`. 

```
data "ibm_container_cluster_worker" "cluster" {
  worker_id    = "dal10-1112222abd111222"
}
```

### Input parameters
{: #container-worker-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `resource_group_id` | String | Optional | The ID of the resource group where your cluster is provisioned into. To find the resource group, run `ibmcloud resource groups` or use the `ibm_resource_group` data source. If this parameter is not provided, the `default` resource group is used.|
| `worker_id` | String | Required | The ID of the worker node for which you want to retrieve information. To find the ID, run `ibmcloud ks worker ls --cluster <cluster_name_or_ID>`. 

### Output parameters
{: #container-worker-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `private_ip` | String | The private IP address that is assigned to the worker node. |
| `private_vlan` | String | The ID of the private VLAN that the worker node is attached to. |
| `public_ip` | String | The public IP address that is assigned to the worker node. | 
| `public_vlan` | String | The ID of the public VLAN that the worker node is attached to. |
| `state` | String | The state of the worker node. | 
| `status` | String | The status of the worker node. |

## `ibm_container_cluster_versions`
{: #container-cluster-versions}

Retrieve information about supported Kubernetes versions in {{site.data.keyword.containerlong_notm}} clusters. 
{: shortdesc}

To find a list of supported Kubernetes versions, see the [{{site.data.keyword.containerlong_notm}} documentation](/docs/containers?topic=containers-cs_versions).

### Sample Terraform code
{: #container-cluster-versions-sample}

The following example shows how to retrieve information about supported Kubernetes versions for the resource group `11222333111abc111`. 

```
data "ibm_container_cluster_versions" "cluster_versions" {
  resource_group_id          = "11222333111abc111"
}
```

### Input parameters
{: #container-cluster-versions-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `resource_group_id` | String | Optional | The ID of the resource group where your cluster is provisioned into. To find the resource group, run `ibmcloud resource groups` or use the `ibm_resource_group` data source. If this parameter is not provided, the `default` resource group is used.|

### Output parameters
{: #container-cluster-versions-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `id` | String | The unique identifier of the cluster. | 
| `valid_kube_versions` | String | The supported Kubernetes version in {{site.data.keyword.containerlong_notm}} clusters. | 
| `valid_openshift_versions` | String | The supported OpenShift Container Platform version in {{site.data.keyword.openshiftlong_notm}} clusters.

## `ibm_container_worker_pool`
{: #container-worker-pool}

Import information about Kubernetes cluster on an {{site.data.keyword.cloud_notm}} as a read only data source.
{: shortdesc}

### Sample Terraform code
{: #container-worker-pool-sample}

The following example shows how to import information about Kubernetes clusters.

```
data "ibm_container_worker_pool" "testacc_ds_worker_pool"{
  worker_pool_name = ibm_container_worker_pool.test_pool.worker_pool_name
  cluster          = ibm_container_cluster.testacc_cluster.id
}
```

### Input parameters
{: #container-worker-pool-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `cluster` | String | Required | The name or ID of the cluster.|
| `worker_pool_name` | String | Required | The name of the worker pool that need to be retrieved.|


### Output parameters
{: #container-worker-pool-output}

Review the output parameters that are exported.
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `id` | String | The unique identifier of the worker pool. | 
| `state` | String | Worker pool state. | 
| `zones` | String | List of zones attached to the worker_pool. |
| `zones.zone` | String | Zone name. |
| `zones.private_vlan` | String | The ID of the private VLAN. |
| `zones.public_vlan` | String | The ID of the public VLAN. |
| `zones.worker_count` | String | Number of workers attached to this zone.|
| `machine_type` | String | The machine type of the worker node. |
| `size_per_zone` | String | Number of workers per zone in this pool. |
|`hardware` | String | The level of hardware isolation for your worker node. |
|`disk_encryption` | String | Disk encryption on a worker. |
|`labels` | String | Labels on all the workers in the worker pool.|
|`resource_group_id` | String | The ID of the worker pool resource group. |

## ibm_container_vpc_alb
{: #container-vpc-alb}

Import the details of a Kubernetes cluster ALB on an {{site.data.keyword.cloud_notm}} as a read only data source.
{: shortdesc}

### Sample Terraform code
{: #container-vpc-alb-sample1}

In the following example you can configure an ALB.

```
data "ibm_container_vpc_cluster_alb" "alb" {
  alb_id = "public-cr083d810e501d4c73b42184eab5a7ad56-alb"
}
```

### Input parameters
{: #container-vpc-alb-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `alb_id` | String | Required | The name or ID of the application load balancer. |

### Output parameters
{: #container-vpc-alb-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `alb_type` | String | The ALB type. |
| `cluster` | String | The name of the cluster. |
| `name` | String | The name of the ALB. |
| `id` | String | The ALB ID. |
| `load_balancer_hostname` | String | The name of the load balancer. |
| `resize` | String | Resize of the ALB. | 
| `state` | String | ALB state. |
| `status` | String | The status of the ALB. |
| `zone` | String | The name of the zone. |
| `enable` | String | Enable an ALB for the cluster. | 
| `disable_deployment` | String | Disables the ALB deployment details. |

## `ibm_container_vpc_cluster`
{: #container-vpc-cluster}

Retrieve information about a VPC cluster in {{site.data.keyword.containerlong_notm}}. 
{: shortdesc}

### Sample Terraform code
{: #container-vpc-cluster-sample}

The following example shows how to retrieve informaion about a VPC cluster that is named `mycluster`. 

```
data "ibm_container_vpc_cluster" "cluster" {
  cluster_name_id   = "mycluster"
  resource_group_id = data.ibm_resource_group.group.id
}
```

### Input parameters
{: #container-vpc-cluster-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `cluster_name_id` | String | Required | The name or ID of the VPC cluster that you want to retrieve.  |
| `resource_group_id` | String | Optional | The ID of the resource group where your cluster is provisioned into. To list resource groups, run `ibmcloud resource groups` or use the `ibm_resource_group` data source.|

### Output parameters
{: #container-vpc-cluster-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `albs` | List of objects | A list of Ingress application load balancers (ALBs) that are attached to the cluster. |
| `albs.id` | String | The unique identifier of the Ingress ALB. |
| `albs.name` | String | The name of the Ingress ALB. |
| `albs.alb_type` | String | The type of ALB. Supported values are `public` and `private`. 
| `albs.enable` | Boolean | Indicates if the ALB is enabled (**true**) or disabled (**false**) in the cluster. |
| `albs.state` | String | The state of the ALB. Supported values are `enabled` or `disabled`. | 
| `albs.load_balancer_hostname` | String | The host name of the ALB. |
| `albs.resize` | Boolean |  Indicate whether resizing should be done. | 
| `albs.disable_deployment` | Boolean |  Indicate whether to disable deployment only on disable application load balancer (ALB).|
| `crn` | String | The CRN of the cluster. |
| `health` | String | The health of the cluster master. |
| `id` | String | The unique identifier of the cluster. |
| `ingress_hostname`| String|The hostname that was assigned to your Ingress subdomain.| 
|`ingress_secret`|String|The name of the Kubernetes secret that was created for your Ingress subdomain.|
| `kube_version` | String | The Kubernetes version of the cluster, including the major.minor version. |
| `master_url` | String | The URL of the cluster master. |
| `name` | String | The name of the cluster. |
| `public_service_endpoint` | Boolean | Indicates if the public service endpoint is enabled (**true**) or disabled (**false**) for a cluster. | 
| `public_service_endpoint_url` | String | The URL of the public service endpoint for your cluster. |
| `private_service_endpoint` | Boolean | Indicates if the private service endpoint is enabled (**true**) or disabled (**false**) for a cluster. | 
| `private_service_endpoint_url` | String | The URL of the private service endpoint for your cluster.|
| `status` | String | The status of the cluster master. |
| `worker_count` | Integer | The number of worker nodes per zone in the default worker pool. Default value ‘1’. |
| `workers` | List of objects | A list of worker nodes that belong to the cluster. | 
| `worker_pools` | List of objects | A list of worker pools that exist in the cluster.|
| `worker_pools.flavor` | String | The flavor that is used for the worker nodes in the worker pool. |
| `worker_pools.name` | String | The name of the worker pool. |
| `worker_pools.worker_count` | Integer | The total number of worker nodes in this worker pool. |
| `worker_pools.isolation` | String | The level of hardware isolation for the worker node. For VPC clusters, this value is always `shared`. |
| `worker_pools.id` | String | The ID of the worker pool. |
| `worker_pools.labels` | List of strings | A list of labels that are added to the worker pool. |
| `worker_pools.zones` | List of objects | A list of zones that are attached to the worker pool. |
| `worker_pools.zones.zone` | String | The name of the zone. |
| `worker_pools.zones.worker_count` | Integer | The number of worker nodes in this worker pool. |
| `worker_pools.zones.subnets` | String | The ID of the subnet that the worker pool is attached to in the zone. |
| `worker_pools.zones.subnets.id` | String | The ID of the subnet that the worker pool is attached to in the zone. |
| `worker_pools.zones.subnets.primary` | Boolean | If set to **true**, the subnet is used as the primary subnet. |

## `ibm_container_vpc_cluster_worker`
{: #container-vpc-worker}

Retrieve information about the worker nodes of your {{site.data.keyword.containerlong_notm}} VPC cluster. 
{: shortdesc}

### Sample Terraform code
{: #container-vpc-worker-sample}

The following example retrieves information about a worker node with the ID `dal10-1112222abd111222` in the `mycluster` cluster. 

```
data "ibm_container_cluster_worker" "worker_foo" {
  worker_id       = "dal10-1112222abd111222"
  cluster_name_id = "mycluster"
}
```

### Input parameters
{: #container-vpc-worker-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `cluster_name_id` | String | Required | The name or ID of the cluster that the worker node belongs to. |
| `worker_id` | String | Required | The ID of the worker node for which you want to retrieve information. To find the ID, run `ibmcloud ks worker ls --cluster <cluster_name_or_ID>`. 
| `flavor` | String | Optional | The flavor of the worker node. |
| `resource_group_id` | String | Optional | The ID of the resource group where your cluster is provisioned into. To find the resource group, run `ibmcloud resource groups` or use the `ibm_resource_group` data source. If this parameter is not provided, the `default` resource group is used.|


### Output parameters
{: #container-vpc-worker-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `cidr` | String | The CIDR of the network. |
| `ip_address` | String | The IP address of the worker pool that the worker node belongs to. |
| `network_interfaces` | String | The network interface of the cluster. |
| `pool_id` | String | The ID of the worker pool that the worker node belongs to. |
| `pool_name` | String | The name of the worker pool that the worker node belongs to. |
| `state` | String | The state of the worker node. | 
| `subnet_id` | String | The ID of the worker pool subnet that the worker node is attached to. |


## ibm_container_vpc_worker_pool
{: #container-vpc-workerpool}

Import the details of a Kubernetes cluster worker pool on an {{site.data.keyword.cloud_notm}} as a read only data source.
{: shortdesc}

### Sample Terraform code
{: #container-vpc-alb-sample2}

In the following example, you can create a worker pool for a VPC cluster.

```
data "ibm_container_vpc_cluster_worker_pool" "testacc_ds_worker_pool" {
    cluster = "cluster_name"
    worker_pool_name = i"worker_pool_name
}
```

### Input parameters
{: #container-vpc-workerpool-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `worker_pool_name` | String | Required | The name of the worker pool. |
| `cluster` | String | Required | The name or id of the cluster. |

### Output parameters
{: #container-vpc-workerpool-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `id` | String | The unique identifier of the worker pool resource, as <cluster_name_id>/<worker_pool_id>.|
| `vpc_id` | String | The ID of the VPC.  |
| `worker_count` | String | The number of worker nodes per zone in the worker pool. |
| `flavor` | String | The flavour of the worker node. |
| `zones` | String | The name of the load balancer. |
| `resize` | String | Resize of the ALB. | 
| `state` | String | ALB state. |
| `status` | String | The status of the ALB. |
| `zone` | String | A nested block describes the zones of the worker_pool. Nested zones blocks has `subnet-id` and `name`.|
| `zone.subnet-id` | String | The worker pool subnet to assign the cluster.|
| `zone.subnet-name` | String | Name of the zone.|
| `labels` | String | Labels on all the workers in the worker pool. | 
| `resource_group_id` | String | The ID of the resource group. |
| `provider` | String | Provider Details of the worker Pool. |
| `isolation` | String | Isolation for the worker node.|


* `id` - The unique identifier of the worker pool resource. The id is composed of \<cluster_name_id\>/\<worker_pool_id\>.<br/>
* `vpc_id` -  The Id of VPC 
* `worker_count` - The number of worker nodes per zone in the worker pool.
* `flavor` - The flavour of the worker node.
* `zones` - A nested block describing the zones of this worker_pool. Nested zones blocks have the following structure:
  * `subnet-id` -  The worker pool subnet to assign the cluster. 
  * `name` -  Name of the zone.
* `labels` -  Labels on all the workers in the worker pool.
* `resource_group_id` -  The ID of the resource group.
* `provider` -  Provider Details of the worker Pool.
* `isolation` -  Isolation for the worker node
