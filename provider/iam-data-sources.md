---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-19"

keywords: terraform identity and access, terraform iam, terraform permissions, terraform iam policy

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

# Identity & Access (IAM) data sources
{: #identity-&-access-data-sources}

Review the data sources that you can use to retrieve information about your Identity and Access Management (IAM) resources. All data sources are imported as read-only information. You can reference the output parameters for each data source by using Terraform interpolation syntax.

## `ibm_iam_auth_token`
{: #iam-token}

Retrieve information about your IAM access token. You can use this token to authenticate with the {{site.data.keyword.cloud_notm}} platform.
{: shortdesc}

### Sample Terraform code
{: #iam-token-sample}

```
data "ibm_iam_auth_token" "tokendata" {}
```


### Input parameters
{: #iam-token-input}

No input parameters are required for this resource.
{: shortdesc}

### Output parameters
{: #iam-token-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`iam_access_token` |String|The IAM access token. |
|`iam_refresh_token`|String|The IAM refresh token. |
|`uaa_access_token`|String| The UAA access token. |
|`uaa_refresh_token`|String|The UAA refresh token. |


## `ibm_iam_service_id`
{: #iam-service}

Retrieve information about an IAM service ID. 
{: shortdesc}

### Sample Terraform code
{: #iam-service-sample}

The following example retrieves information about the `myservice` service. 

```
data "ibm_iam_service_id" "ds_serviceID" {
  name = "myservice"
}
```

### Input parameters
{: #iam-service-input}

Review the input parameters that you can specify for your data source.

|Name|Data type|Required/ optional|Description|
|----|-----------|-------|----------|
|`name`|String| Required|The name of the service.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #iam-service-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|----------|
|`service_ids`|List of objects| A nested block list of IAM service IDs. |
|`service_ids.id`|String|The unique identifier of the service ID.  |
|`service_ids.bound_to`| String|The service the service ID is bound to.  |
|`service_ids.crn`| String|The CRN of the service ID.  |
|`service_ids.description`| String|A description of the service ID.  |
|`service_ids.version`| String|The version of the service ID.  |
|`service_ids.locked`|Boolean| If set to **true**, the service ID is locked. |
{: caption="Table 1. Available output parameters" caption-side="top"}


## `ibm_iam_service_policy`
{: #iam-service-policy}

Retrieve information about an IAM service policy. 
{: shortdesc}

### Sample Terraform code
{: #iam-service-policy-sample}

```
resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "ServiceId-a1aaa111-1111-111a-1a11-a11a1a11a11a"
  roles        = ["Manager", "Viewer", "Administrator"]

  resources = [{
    service              = "kms"
    region               = "us-south"
    resource_instance_id = "${element(split(":",ibm_resource_instance.instance.id),7)}"
  }]
}

data "ibm_iam_service_policy" "testacc_ds_service_policy" {
  iam_service_id = "${ibm_iam_service_policy.policy.iam_service_id}"
}

```

### Input parameters
{: #iam-service-policy-input}

Review the input parameters that you can specify for your data source.

|Name|Data type|Required/ optional|Description|
|----|-----------|---------|---------------------|
|`iam_service_id`|String|Required| The UUID of the service ID.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #iam-service-policy-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|-------------|
|`policies`|List of objects|A nested block describing IAM service policies that are assigned to a service ID. |
|`policies.id`|String|The unique identifier of the IAM service policy. The id is composed of \<iam_service_id\>/\<service_policy_id\>  |
|`policies.roles`| String|The roles that are assigned to the policy.	|
|`policies.resources`| List of objects| A nested block describing the resources in the policy.		|
|`policies.resources.service`|The service name of the policy definition. 		|
|`policies.resources.resource_instance_id`|The ID of resource instance of the policy definition.		|
|`policies.resources.region`|The region of the policy definition.		|
|`policies.resources.resource_type`|The resource type of the policy definition.		|
|`policies.resources.resource`|The resource of the policy definition.		|
|`policies.resources.resource_group_id`|The ID of the resource group.|
{: caption="Table 1. Available output parameters" caption-side="top"}


## `ibm_iam_user_policy`
{: #iam-user-policy}

Retrieve information about an IAM user policy. 
{: shortdesc}

### Sample Terraform code
{: #iam-user-policy-sample}

```
resource "ibm_iam_user_policy" "policy" {
  ibm_id = "user@us.ibm.com"
  roles  = ["Viewer"]

  resources = [{
    service = "kms"
    region  = "us-south"
  }]
}

data "ibm_iam_user_policy" "testacc_ds_user_policy" {
  ibm_id = "${ibm_iam_user_policy.policy.ibm_id}"
}

```

### Input parameters
{: #iam-user-policy-input}

Review the input parameters that you can specify for your data source.

|Name|Data type|Required/ optional|Description|
|----|-----------|---------------|-------------------|
|`ibm_id`|String|Required| The IBMid or email address of the user.|
{: caption="Table. Available input parameters" caption-side="top"}


### Output parameters
{: #iam-user-policy-output}

The following attributes are exported:

|Name|Data type|Description|
|----|-----------|-------------|
|`policies`|List|A nested block describing IAM Policies assigned to user. |
|`policies.id`|String|The unique identifier of the IAM user policy. The ID is composed of \<ibm_id\>/\<user_policy_id\>.  |
|`policies.roles`| String|The roles that are assigned to the policy.	|
|`policies.resources`| List of objects| A nested block describing the resources in the policy.		|
|`policies.resources.service`|String|The service name of the policy definition. 		|
|`policies.resources.resource_instance_id`|String}The ID of resource instance of the policy definition.		|
|`policies.resources.region`|String|The region of the policy definition.		|
|`policies.resources.resource_type`|String|The resource type of the policy definition.		|
|`policies.resources.resource`|String|The resource of the policy definition.		|
|`policies.resources.resource_group_id`|String|The ID of the resource group.|
{: caption="Table 1. Available output parameters" caption-side="top"}
