---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-16" 

keywords: terraform provider plugin, terraform resource group, terraform iam service, terraform resource management

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

# Resource management resources
{: #resource-mgmt-resources}

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## `ibm_resource_group`
{: #rg}

Create, update, or delete an IBM Cloud resource group. 
{: shortdesc}

### Sample Terraform code
{: #rg-sample}

```

resource "ibm_resource_group" "resourceGroup" {
  name     = "prod"
}

```

### Input parameters
{: #rg-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required|The name of the resource group.|
|`quota_id`|String|Removed|The ID of the quota. You can refer to a quota by name using the resource quota data source.|
|`tags`|Array of strings|Optional|Tags associated with the resource group instance. The tags are managed locally and not stored on the IBM Cloud service endpoint at this moment.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #rg-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the new resource group.|
|`default`|Boolean|Specifies whether its default resource group or not.|
|`state`|String|State of the resource group.|
{: caption="Table 1. Available output parameters" caption-side="top"}

### Import
{: #rg-import}

`ibm_resource_group` can be imported using resource group id. 

```
terraform import ibm_resource_group.example <resource_group_ID>
```
{: pre}



## `ibm_resource_instance`
{: #resource-instance}

Create, update, or delete an IAM-enabled service instance. 

### Sample Terraform code
{: #resource-instance-sample}

```
data "ibm_resource_group" "group" {
  name = "test"
}

resource "ibm_resource_instance" "resource_instance" {
  name              = "test"
  service           = "cloud-object-storage"
  plan              = "lite"
  location          = "global"
  resource_group_id = data.ibm_resource_group.group.id
  tags              = ["tag1", "tag2"]

  parameters {
    "HMAC" = true
  }
  //User can increase timeouts 
  timeouts {
    create = "15m"
    update = "15m"
    delete = "15m"
  }
}
```

### Input parameters
{: #resource-instance-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required|A descriptive name used to identify the resource instance.|
|`service`|String|Required|The name of the service offering. You can retrieve the value by running the `ibmcloud catalog service-marketplace` or `ibmcloud catalog search` command.|
|`plan`|String|Required|The name of the plan type supported by service. You can retrieve the value by running the `ibmcloud catalog service <servicename>` command.|
|`location`|String|Required|Target location or environment to create the resource instance.|
|`resource_group_id`|String|Optional|The ID of the resource group where you want to create the service. You can retrieve the value from data source `ibm_resource_group`. If not provided creates the service in default resource group.|
|`tags`|Array of strings|Optional|Tags associated with the instance.|
|`parameters`|Map|Optional|Arbitrary parameters to create instance. The value must be a JSON object.|
|`guid`|String|Optional|The GUID of the resource instance.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #resource-instance-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the new resource instance.|
|`status`|String|The status of resource instance.|
{: caption="Table 1. Available output parameters" caption-side="top"}


### Timeouts
{: #resource-instance-timeout}

`ibm_resource_instance` provides the following timeouts:

|Name|Description|
|----|-----------|
|`create`|(Default 10 minutes) Used for Creating Instance.|
|`update`|(Default 10 minutes) Used for Updating Instance.|
|`delete`|(Default 10 minutes) Used for Deleting Instance.|
{: caption="Table. Available timeout configuration options" caption-side="top"}




## `ibm_resource_key`
{: #resource-key}

Create, update, or delete service credentials for an IAM-enabled service. 
{: shortdesc}

By default, the `ibm_resource_key` resource creates service credentials that use the public service endpoint of a service. To create service credentials that use the private service endpoint instead, you must explicitly define that by using the `parameter` input paramter. Note that your service might not support private service endpoints yet. 
{: note}

### Sample Terraform code
{: #resource-key-sample}

#### Creating credentials for a resource without using a service ID

```
data "ibm_resource_instance" "resource_instance" {
  name = "myobjectstorage"
}

resource "ibm_resource_key" "resourceKey" {
  name                 = "mykey"
  role                 = "Viewer"
  resource_instance_id = data.ibm_resource_instance.resource_instance.id

  //User can increase timeouts
  timeouts {
    create = "15m"
    delete = "15m"
  }
}
```

#### Creating credentials for a service ID

The `ibm_resource_instance` resource does not support creating service credentials for a service ID. However, you can pass in a service ID as an additional parameter to create credentials for a service ID. 

```
data "ibm_resource_instance" "resource_instance" {
  name = "myobjectstorage"
}

resource "ibm_iam_service_id" "serviceID" {
  name        = "test"
  description = "New ServiceID"
}

resource "ibm_resource_key" "resourceKey" {
  name                 = "myobjectkey"
  role                 = "Viewer"
  resource_instance_id = data.ibm_resource_instance.resource_instance.id
  parameters {
    serviceid_crn = ibm_iam_service_id.serviceID.crn
  }

  //User can increase timeouts
  timeouts {
    create = "15m"
    delete = "15m"
  }
}
```

#### Creating credentials that use the private service endpoint
{: #resource-key-sample-private}

The following example shows how you can create a Databases for etcd service instance with service credentials that use the private service endpoint. 
{: shortdesc}

```
data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_resource_instance" "resource_instance" {
  name              = "tf-db"
  service           = "databases-for-etcd"
  plan              = "standard"
  resource_group_id = data.ibm_resource_group.group.id
  location = "us-south"
  
  parameters {
    
    service-endpoints = "private"
    
   }
}

resource "ibm_resource_key" "resourceKey" {
  name                 = "tfkey"
  role                 = "Viewer"
  resource_instance_id = ibm_resource_instance.resource_instance.id
  
  parameters {

     service-endpoints =  "private"
  
  }
}

```


### Input parameters
{: #resource-key-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required| A descriptive name used to identify a resource key.|
|`role`|String|Required|The name of the user role. Valid roles are `Writer`, `Reader`, `Manager`, `Administrator`, `Operator`, `Viewer`, and `Editor`.|
|`parameters`|Map|Optional|Arbitrary parameters to pass. Must be a JSON object.|
|`resource_instance_id`|String|Optional|The ID of the resource instance associated with the resource key. **NOTE**: Conflicts with `resource_alias_id`.|
|`resource_alias_id`|String|Optional|The ID of the resource alias associated with the resource key. **NOTE**: Conflicts with `resource_instance_id`.|
|`tags`|Array of strings|Optional|Tags associated with the resource key instance. Tags are managed locally and not stored on the IBM Cloud service endpoint at this moment.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #resource-key-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the new resource key.|
|`credentials`|String|The credentials associated with the key.|
|`status`|String|Status of resource key.|
{: caption="Table 1. Available output parameters" caption-side="top"}


### Timeouts
{: #resource-key-timeout}

`ibm_resource_key` provides the following timeouts:

|Name|Description|
|----|-----------|
|`create`|(Default 10 minutes) Used for Creating Key.|
|`delete`|(Default 10 minutes) Used for Deleting Key.|
{: caption="Table. Available timeout configuration options" caption-side="top"}

