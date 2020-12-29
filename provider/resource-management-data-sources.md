---

copyright:
  years: 2017, 2020
lastupdated: "2020-12-28"

keywords: terraform provider plugin, terraform resource group, terraform resource management, terraform iam services

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

# Resource management data sources
{: #resource-management-data-sources}

Before you start working with your data source, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your IBM Cloud Provider plug-in for Terraform configuration file. 
{: important}

## `ibm_resource_group`
{: #resource-group}

Retrieve information about an {{site.data.keyword.cloud_notm}} resource group. 
{: shortdesc}

### Sample IBM Cloud Provider plug-in for Terraform code
{: #resource-group-sample}

The following two examples retrieve information about the `default` resource group. 

```
data "ibm_resource_group" "group" {
  name = "default"
}
```

```
data "ibm_resource_group" "group" {
  is_default = "true"
}
```

### Input parameters
{: #resource-group-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required / optional|Description|
|----|-----------|------------|-----------------------------|
|`name`|String|Optional|The name of the resource group. You can retrieve the value by running the `ibmcloud resource groups` command. If you set this option, do not specify `is_default` at the same time. |
|`is_default`|Boolean|Optional|If set to **true**, you retrieve information about the default resource group. If set to **false**, you retrieve information about a resource group other than default. The default value is `false`. If you set this option, do not specify `name` at the same time. |
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #resource-group-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
|`id`|String|The unique identifier of the resource group.  |
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_resource_instance`
{: #resouce-instance}

Retrieve information about an IAM-enabled service instance.
{: shortdesc}

### Sample IBM Cloud Provider plug-in for Terraform code
{: #resouce-instance-sample}

```
data "ibm_resource_group" "group" {
  name = "default"
}

data "ibm_resource_instance" "testacc_ds_resource_instance" {
  name              = "myobjectstore"
  location          = "global"
  resource_group_id = data.ibm_resource_group.group.id
  service           = "cloud-object-storage"
}
```

### Input parameters
{: #resouce-instance-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required / optional|Description|
|----|-----------|------------|-----------------------------|
|`name`|String|Required|The name of the resource instance.|
|`resource_group_id`|String|Optional|The ID of the resource group where the service instance exists. If no value is specified, the `default` resource group is used by default. |
|`location`|String|Optional|The location where the instance is deployed to.|
|`service`|String|Optional|The service type of the instance. You can retrieve the value by running the `ibmcloud catalog service-marketplace` or `ibmcloud catalog search` command.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #resouce-instance-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
|`id`|String|The unique identifier of the service instance.|
|`status`|String|The status of the service instance.|
|`plan`|String| The service plan that is used for the service instance.|
|`guid`|String|The GUID of the resource instance.|
|`extensions`|String|The extended metadata as a map associated with the resource instance.|
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_resource_key`
{: #resource-key}

Retrieve information about existing access keys for an IAM-enabled service instance.
{: shortdesc}

### Sample IBM Cloud Provider plug-in for Terraform code
{: #resource-key-sample}

The following example retrieves information about the `myobjectKey` access key. 

```
data "ibm_resource_key" "resourceKeydata" {
  name                  = "myobjectKey"
  resource_instance_id  = ibm_resource_instance.resource.id
}
```

### Input parameters
{: #resource-key-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required / optional|Description|
|----|-----------|------------|-----------------------------|
|`name`|String|Required|The name of the resource key. You can retrieve the value by running the `ibmcloud resource service-keys` command.|
|`resource_instance_id`|String|Optional|The ID of the resource instance that the resource key is associated with. You can retrieve the value by running the `ibmcloud resource service-instances` command. If you set this option, do not specify `resource_alias_id` at the same time. |
|`resource_alias_id`|String|Optional|The ID of the resource alias that the resource key is associated with. You can retrieve the value by running the `ibmcloud resource service-alias` command. If you set this option, do not specify `resource_instance_id` at the same time.|
|`most_recent`|Boolean|Optional|For multiple resource keys, you can set this argument to `true` to import only the most recently created key.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #resource-key-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
|`id`|String|The unique identifier of the resource key.|
|`credentials`|String|The credentials associated with the key.|
|`role`|String|The user role.|
|`status`|String|Status of the resource key.  |
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_resource_quota`
{: #resource-quota}

Retrieve information about the quota for an IAM-enabled service instance. 
{: shortdesc}

### Sample IBM Cloud Provider plug-in for Terraform code
{: #resource-quota-sample}

```
data "ibm_resource_quota" "rsquotadata" {
  name = "Trial Quota"
}
```

### Input parameters
{: #resource-quota-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required / optional|Description|
|----|-----------|------------|-----------------------------|
|`name`|String|Required|The name of the quota for the service instance. You can retrieve the value by running the `ibmcloud resource quotas` command.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #resource-quota-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
|`id`|String|The unique identifier of the quota.|
|`type`|String|The type of the quota.|
|`max_apps`|Integer|Defines the total app limit.|
|`max_instances_per_app`|Integer|Defines the total instances limit per app.|
|`max_app_instance_memory`|String|Defines the total memory of app instance.|
|`total_app_memory`|String|Defines the total memory for app.|
|`max_service_instances`|Integer|Defines the total service instances limit.|
|`vsi_limit`|Integer|Defines the VSI limit.|
{: caption="Table 1. Available output parameters" caption-side="top"}
