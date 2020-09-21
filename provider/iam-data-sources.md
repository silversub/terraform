---

copyright:
  years: 2017, 2020
lastupdated: "2020-09-18"

keywords: terraform identity and access, terraform iam, terraform permissions, terraform iam policy

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
}

# Identity & Access (IAM) data sources
{: #iam-data-sources}

Review the data sources that you can use to retrieve information about your Identity and Access Management (IAM) resources. All data sources are imported as read-only information. You can reference the output parameters for each data source by using Terraform interpolation syntax.

Before you start working with your data source, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## `ibm_iam_access_group`
{: #access_group}

Retrieve information about an IAM access group. 
{: shortdesc}

### Sample Terraform code
{: #access-group-sample}

```
data "ibm_iam_access_group" "accgroup" {
  access_group_name = ibm_iam_access_group.accgroup.name
}
```

### Input parameters
{: ##access-group-input}

Review the input parameters that you can specify for your data source.

|Name|Data type|Required / optional|Description|
|----|-----------|-------|----------|
|`access_group_name`|String|Optional|The name of the access group that you want to retrieve details for. If no access group is specified, all access groups that exist in the {{site.data.keyword.cloud_notm}} account are returned.| 

### Output parameters
{: #access-group-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|---------|
|`groups`|List of access groups|A list of IAM access groups that are set up for an {{site.data.keyword.cloud_notm}} account.|
|`groups.id`|String|The ID of the IAM access group.|
|`groups.name`|String|The name of the IAM access group.|
|`groups.description`|String|The description of the IAM access group.|
|`groups.ibm_ids`|Array of strings|A list of IBM ID that belong to the access group.|
|`groups.iam_service_ids`|Array of strings|A list of service IDs that belong to the access group.|
|`groups.rules`|List of access group rules|A list of dynamic rules that are applied to the IAM access group.|
|`groups.rules.name`|String|The name of the dynamic rule. |
|`groups.rules.expiration`|Integer|The number of hours that authenticated users can work in IBM Cloud before they must refresh their access.|
|`groups.rules.identity_provider`|String|The URI of your identity provider. This is the SAML "entity ID" field, which is sometimes referred to as the issuer ID, for the identity provider as part of the federation configuration for onboarding with IBMid. |
|`groups.rules.conditions`|List of rule conditions|A list of conditions that the rule must satisfy.|
|`groups.rules.conditions.claim`|String|The key value to evaluate the condition against. The key depends on what key-value pairs your identity provider provides. For example, your identity provider might include a key that is named `blueGroups` and that holds all the user groups that have access. To apply a condition for a specific user group within the `blueGroups` key, you specify `blueGroups` as your claim and add the value that you are looking for in `conditions.value`. |
|`groups.rules.conditions.operator`|String|The operation to perform on the claim. Supported values are `EQUALS`, `QUALS_IGNORE_CASE`, `IN`, `NOT_EQUALS_IGNORE_CASE`, `NOT_EQUALS`, and `CONTAINS`.|
|`groups.rules.conditions.value`|String|The value that the claim is compared to using the `groups.rules.conditions.operator`.|
|`groups.rules.rule_id`|String|The ID of the dynamic rule.|


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

## `ibm_iam_role_actions`
{: #iam-role-actions}

Retrieve a list of actions for an {{site.data.keyword.cloud_notm}} service that are included in an IAM service access role. 

### Sample Terraform code
{: #iam-role-actions-sample}

```
data "ibm_iam_role_actions" "test" {
  service = "kms"
}
```
{: codeblock}

### Input parameters
{: #iam-role-actions-input}

Review the input parameters that you can specify for your data source.

|Name|Data type|Required / optional|Description|
|----|-----------|-------|----------|
|`service`|String|Required|The name of the {{site.data.keyword.cloud_notm}} service for which you want to list supported actions. For account management services, you can find supported values in the [documentation](/docs/account?topic=account-account-services#api-acct-mgmt). For other services, run the `ibmcloud catalog service-marketplace` command and retrieve the value from the **Name** column of your CLI output.|

### Output parameters
{: #iam-role-actions-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|----------|
|`id`|String|The unique identifier of the service.|
|`manager`|List of strings|A list of supported actions that require the **Manager** service access role.|
|`reader`|List of strings|A list of supported actions that require the **Reader** service access role.|
|`reader_plus`|List of strings|A list of supported actions that require the **Reader plus** service access role.|
|`writer`|List of strings|A list of supported actions that require the **Writer** service access role.|

## `ibm_iam_roles`
{: #iam-roles}

Retrieve information about supported IAM roles for an {{site.data.keyword.cloud_notm}} service. 
{: shortdesc}

### Sample Terraform code
{: #iam-roles-sample}

```
data "ibm_iam_roles" "test" {
  service = "kms"
}
```
{: codeblock}

### Input parameters
{: #iam-roles-input}

Review the input parameters that you can specify for your data source.

|Name|Data type|Required / optional|Description|
|----|-----------|-------|----------|
|`service`|String|Required|The name of the {{site.data.keyword.cloud_notm}} service for which you want to list supported IAM roles. For account management services, you can find supported values in the [documentation](/docs/account?topic=account-account-services#api-acct-mgmt). For other services, run the `ibmcloud catalog service-marketplace` command and retrieve the value from the **Name** column of your CLI output.|

### Output parameters
{: #iam-roles-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|----------|
|`id`|String|The ID of your {{site.data.keyword.cloud_notm}} account.|
|`roles`|List of supported IAM roles|A list of supported IAM service access, platform, and custom roles for an {{site.data.keyword.cloud_notm}} service. |
|`roles.name`|String|The name of the role.|
|`roles.description`|String|The description of the role.|
|`roles.type`|String|The type of role. Supported values are `service`, `platform`, and `custom`.|


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

|Name|Data type|Required / optional|Description|
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
    resource_instance_id = element(split(":",ibm_resource_instance.instance.id),7)
  }
  ]
}

data "ibm_iam_service_policy" "testacc_ds_service_policy" {
  iam_service_id = ibm_iam_service_policy.policy.iam_service_id
}

```

### Input parameters
{: #iam-service-policy-input}

Review the input parameters that you can specify for your data source.

|Name|Data type|Required / optional|Description|
|----|-----------|---------|---------------------|
|`iam_service_id`|String|Required| The UUID of the service ID.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #iam-service-policy-output}

Review the output parameters that you can access after you retrieved your data source.

|Name|Data type|Description|
|----|-----------|-------------|
|`policies`|List of objects|A nested block describes IAM service policies that are assigned to a service ID. |
|`policies.id`|String|The unique identifier of the IAM service policy. The id is composed of \<iam_service_id\>/\<service_policy_id\>  |
|`policies.roles`| String|The roles that are assigned to the policy.|
|`policies.resources`| List of objects| A nested block describes the resources in the policy.|`policies.resources.service`|The service name of the policy definition. |
|`policies.resources.resource_instance_id`|The ID of resource instance of the policy definition.|
|`policies.resources.region`|The region of the policy definition.|
|`policies.resources.resource_type`|The resource type of the policy definition.|
|`policies.resources.resource`|The resource of the policy definition.|
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
  }
  ]
}

data "ibm_iam_user_policy" "testacc_ds_user_policy" {
  ibm_id = ibm_iam_user_policy.policy.ibm_id
}

```

### Input parameters
{: #iam-user-policy-input}

Review the input parameters that you can specify for your data source.

|Name|Data type|Required / optional|Description|
|----|-----------|---------------|-------------------|
|`ibm_id`|String|Required| The IBMid or email address of the user.|
{: caption="Table. Available input parameters" caption-side="top"}


### Output parameters
{: #iam-user-policy-output}

The following attributes are exported:

|Name|Data type|Description|
|----|-----------|-------------|
|`policies`|List|A nested block describes IAM Policies assigned to user. |
|`policies.id`|String|The unique identifier of the IAM user policy. The ID is composed of \<ibm_id\>/\<user_policy_id\>.  |
|`policies.roles`| String|The roles that are assigned to the policy.	|
|`policies.resources`| List of objects| A nested block describes the resources in the policy.|
|`policies.resources.service`|String|The service name of the policy definition. 		|
|`policies.resources.resource_instance_id`|String}The ID of resource instance of the policy definition.		|
|`policies.resources.region`|String|The region of the policy definition.		|
|`policies.resources.resource_type`|String|The resource type of the policy definition.		|
|`policies.resources.resource`|String|The resource of the policy definition.		|
|`policies.resources.resource_group_id`|String|The ID of the resource group.|
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_iam_user_profile`
{: #iam-user-profile}

Retrieve information about an IAM user profile. 
{: shortdesc}

### Sample Terraform code
{: #iam-user-profile-sample}

```
resource "ibm_iam_user_settings" "user_setting" {
  iam_id = "example@in.ibm.com"
  allowed_ip_addresses = ["192.168.0.2","192.168.0.3","192.168.0.4"]
}

data "ibm_iam_user_profile" "user_profle" {
  iam_id = ibm_iam_user_settings.user_setting.iam_id
}
```

### Input parameters
{: #iam-user-profile-input}

Review the input parameters that you can specify for your data source.

|Name|Data type|Required / optional|Description|
|----|-----------|---------------|-------------------|
|`id`|String|Required| The IBMid or email address of the user.|
{: caption="Table. Available input parameters" caption-side="top"}


### Output parameters
{: #iam-user-profile-output}

The following attributes are exported:

|Name|Data type|Description|
|----|-----------|-------------|
|`allowed_ip_addresses`|List|List of invited users IPs to access the IBM cloud console. |
|`id`|String|The unique identifier or email address of the IAM user. |
|`firstname`| String|The first name of the user. |
|`lastname`| String| The last name of the user.|
|`state`|String|The state of the user.|
|`phonenumber`|String|The contact number of the user. |
|`email`|String|The email address of the user. |


## `ibm_iam_users`
{: #iam-users}

Retrieve information about an IAM user profile on IBM Cloud as a read-only data source.
{: shortdesc}

### Sample Terraform code
{: #iam-users-sample}

```
	data "ibm_iam_users" "users_profiles"{
  
	}
```

### Output parameters
{: #iam-users-output}

The following attributes are exported:

|Name|Data type|Description|
|----|-----------|-------------|
|`id`|String|The unique identifier user. |
|`users`| String|List of all IAM users. Each user profile has following list of arguments. |
|`users.iam_id`| String|The Id of the IAM user. |
|`users.realm`| String|The realm of the user.  |
|`users.user_id`| String|The user ID used for log in. |
|`users.firstname`| String|The first name of the user. |
|`users.lastname`| String|The last name of the user. |
|`users.state`| String|The state of the user. |
|`users.email`| String|The email of the user. |
|`users.phonenumber`| String|The phone for the user. |
|`users.altphonenumber`| String|The alternative phone number of the user.|
|`users.account_id`| String|The account ID of the user. |
