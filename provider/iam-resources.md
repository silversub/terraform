---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-25"

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

# Identity & Access (IAM) resources
{: #iam-resources}

Create, modify, or delete [{{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)](/docs/iam?topic=iam-iamoverview) resources. 
{: shortdesc}

## `ibm_iam_access_group`
{: #iam-access-group}

Create, modify, or delete an IAM access group. Access groups can be used to define a set of permissions that you want to grant to a group of users. 
{: shortdesc}

### Sample Terraform code
{: #iam-access-group-sample}

The following example creates an access gorup that is named `mygroup`. 

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name        = "mygroup"
  description = "New access group"
}
```

### Input parameters
{: #iam-access-group-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `description` | String | Optional | The description of the access group. |
| `name` | String | Required | The name of the access group. |
| `tags` | Array of strings | Optional|The list of tags that you want to associated with your access group. |

`Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.
{: note}

### Output parameters
{: #iam-access-group-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `id` | String | The unique identifier of the access group. |
| `version` | String | The version of the access group. |

{[white-space.md]}

## `ibm_iam_access_group_members`
{: #iam-access-group-members}

Add, update, or remove users from an IAM access group. 
{: shortdesc}

Multiple `ibm_iam_access_group_members` resources with the same group name produce inconsistent behavior. 
{: important}

### Sample Terraform code
{: #iam-access-group-members-sample}

The following example creates an IAM access group and a service ID. Then, the service ID and a user with the ID `user@ibm.com` is added to the access group. 

```hcl
resource "ibm_iam_access_group" "accgroup" {
  name = "testgroup"
}

resource "ibm_iam_service_id" "serviceID" {
  name = "testserviceid"
}

resource "ibm_iam_access_group_members" "accgroupmem" {
  access_group_id = "${ibm_iam_access_group.accgroup.id}"
  ibm_ids         = ["user@iibm.com"]
  iam_service_ids = ["${ibm_iam_service_id.serviceID.id}"]
}

```

### Input parameters
{: #iam-access-group-members-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `access_group_id` | String | Required | The ID of the access group. | 
| `ibm_ids` | Array of strings | Optional | A list of IBM IDs that you want to add to or remove from the access group. | 
| `iam_service_ids` | Array of strings | Optional | A list of service IDS that you want to add to or remove from the access group. |


### Output parameters
{: #iam-access-group-members-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `id` | String | The unique identifier of the access group members. The ID is returned in the format `<iam_access_group_ID\/<random_ID>`. | 
| `members` | Array of objects | A list of members that are included in the access group. |
| `members.iam_id` | String | The IBMid or service ID of the member. |
| `members.type` | String | The type of member. Supported values are `user` or `service`. 

### Import

`ibm_iam_access_group_members` can be imported using access group ID and random id, eg

```
$ terraform import ibm_iam_access_group_members.example AccessGroupId-5391772e-1207-45e8-b032-2a21941c11ab/2018-10-04 06:27:40.041599641 +0000 UTC
```

{[white-space.md]}

## `ibm_access_group_policy`
{: #iam-access-group-policy}

Create, update, or delete an IAM policy for an IAM access group. 
{: shortdesc}

### Sample Terraform code
{: #iam-access-group-policy}

#### Create a policy for all IAM-enabled resources
{: #all-iam-services}

The following example creates an IAM policy that grants members of the access group the IAM `Viewer` platform role to all IAM-enabled services. 
{: shortdesc}

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles        = ["Viewer"]
}
```

#### Create a policy for all IAM-enabled services within a resource group
{: #all-iam-services-resource-group}

The following example creates an IAM policy that grants members of the access group the IAM `Operator` platform role and the `Writer` service access role to all IAM-enabled services within a resource group. 
{: shortdesc}

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles        = ["Operator", "Writer"]

  resources = [{
    resource_group_id = "${data.ibm_resource_group.group.id}"
  }]
}
```

#### Create a policy for all instances of an {{site.data.keyword.cloud_notm}} service
{: #single-service}

The following example creates an IAM policy that grants members of the access group the IAM `Viewer` platform role to all service instances of {{site.data.keyword.cos_full_notm}}. 
{: shortdesc}

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles        = ["Viewer"]

  resources = [{
    service = "cloud-object-storage"
  }]
}

```
#### Create a policy for a service instance 
{: #single-service-instance}

The following example creates an IAM policy that grants members of the access group the IAM `Viewer` and `Administrator` platform role, and the `Manager` service access role to a single service instance. 
{: shortdesc}

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

resource "ibm_resource_instance" "instance" {
  name     = "test"
  service  = "kms"
  plan     = "tiered-pricing"
  location = "us-south"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles        = ["Manager", "Viewer", "Administrator"]

  resources = [{
    service              = "kms"
    resource_instance_id = "${element(split(":",ibm_resource_instance.instance.id),7)}"
  }]
}

```

#### Create a policy to all instances of an {{site.data.keyword.cloud_notm}} service within a resource group
{: #instances-resource-group}

The following example creates an IAM policy that grants members of the access group the IAM `Viewer` platform role to all instances of {{site.data.keyword.containerlong_notm}} that are created within a specific resource group. 
{: shortdesc}

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles        = ["Viewer"]

  resources = [{
    service           = "containers-kubernetes"
    resource_group_id = "${data.ibm_resource_group.group.id}"
  }]
}

```

#### Access Group Policy using resource and resource type 

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles        = ["Administrator"]

  resources = [{
    resource_type = "resource-group"
    resource      = "${data.ibm_resource_group.group.id}"
  }]
}

```

#### Access Group Policy using attributes

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles           = ["Viewer"]

  resources = [{
    service = "is"

    attributes = {
      "vpcId" = "*"
    }

    resource_group_id = "${data.ibm_resource_group.group.id}"
  }]
}

```

### Input parameters
{: #iam-access-group-policy-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`access_group_id`|String|Required|The ID of the access group.|
| `roles`|List|Required| A comma separated list of roles. Valid roles are `Writer`, `Reader`, `Manager`, `Administrator`, `Operator`, `Viewer`, and `Editor`.
|`resources` |List|Optional|A nested block describing the resource of this policy.|
|`resources.service`|String|Optional|The service name of the policy definition.  You can retrieve the value by running the `ibmcloud catalog service-marketplace` or `ibmcloud catalog search`.|
|`resources.resource_instance_id`|String|Optional|The ID of resource instance of the policy definition.|
|`resources.region` |String|Optional|The region of the policy definition.|
|`resources.resource_type` |String|Optional|The resource type of the policy definition.|
|`resources.resource` |String|Optional|The resource of the policy definition.|
|`resources.resource_group_id`|String|Optional|The ID of the resource group. To retrieve the ID, run `ibmcloud resource groups` or use the `ibm_resource_group` data source. 
|`resources.attributes`|Map|Optional|Set resource attributes in the form of `name=value,name=value`.  If you set this option, do not specify `account_management` at the same time. |
|`account_management`|Boolean|Optional|Gives access to all account management services if set to `true`. Default value `false`. If you set this option, do not specify `resources` at the same time. |
|`tags` |Array of Strings|Optional|A list of tags that you want to add to the access group policy. Tags are managed locally and not stored on the IBM Cloud service endpoint at this moment.|

### Output parameters
{: #iam-access-group-policy-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id`|String|The unique identifier of the access group policy. The ID is composed of `<access_group_id>/<access_group_policy_id>`.|
|`version`|String|The version of the access group policy.|

### Import
{: #iam-access-group-policy-input}

The access group policy can be imported by using the access group ID and the access group policy ID.

```
$ terraform import ibm_iam_access_group_policy.example <access_group_ID>/<access_group_policy_ID>
```

{[white-space.md]}

## `ibm_authorization_policy`
{: #iam-auth-policy}

Create or delete an IAM service authorization policy. 
{: shortdesc} 

### Sample Terraform code
{: #iam-auth-policy-sample}

#### Authorization policy between two services

```hcl
resource "ibm_iam_authorization_policy" "policy" {
  source_service_name = "cloud-object-storage"
  target_service_name = "kms"
  roles               = ["Reader"]
}
```

#### Authorization policy between two services with specific resource type

```hcl
resource "ibm_iam_authorization_policy" "policy" {
  source_service_name  = "is"
  source_resource_type = "image"
  target_service_name  = "cloud-object-storage"
  roles                = ["Reader"]
}
```

#### Authorization policy between two specific instances

```hcl
resource "ibm_resource_instance" "instance1" {
  name     = "mycos"
  service  = "cloud-object-storage"
  plan     = "lite"
  location = "global"
}

resource "ibm_resource_instance" "instance2" {
  name     = "mykms"
  service  = "kms"
  plan     = "tiered-pricing"
  location = "us-south"
}

resource "ibm_iam_authorization_policy" "policy" {
  source_service_name         = "cloud-object-storage"
  source_resource_instance_id = ibm_resource_instance.instance1.id
  target_service_name         = "kms"
  target_resource_instance_id = ibm_resource_instance.instance2.id
  roles                       = ["Reader"]
}
```

### Input parameters
{: #iam-auth-policy-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`source_service_name`|String|Required|The source service name.|
|`target_service_name`|String|Required|The target service name.|
|`roles`|List|Required|A comma separated list of roles. |
|`source_resource_instance_id`|String|Optional|The source resource instance ID.|
|`target_resource_instance_id`|String|Optional| The target resource instance ID.|
|`source_resource_type`|String|Optional| The resource type of the source service.|
|`target_resource_type`|String|Optional|The resource type of the target service.|
|`source_service_account`|String|Optional|The GUID of the account where the source servie is provisioned.|

### Output parametesr
{: #iam-auth-policy-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id`|String|The unique identifier of the authorization policy.|
|`version`|String|The version of the authorization policy.|

### Import
{: #iam-auth-policy-import}

The IAM authorization policy can be imported by using the ID. 

```
terraform import ibm_iam_authorization_policy.example 11aa1a11-11a1-11aa-1111-11111a11a11a
```

## `ibm_authorization_policy_detach`
{: #iam-auth-policy-detach}

Provides a resource for IAM Service Authorizations policy to be detached. This allows authorization policy to deleted.
{: shortdesc}

### Sample Terraform code
{: #iam-auth-policy-detach-sample}

```hcl
resource "ibm_iam_authorization_policy_detach" "policy" {
  authorization_policy_id = "11aa1a11-11a1-11aa-1111-11111a11a11a"
}
```

### Input parameters
{: #iam-auth-policy-detach-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`authorization_policy_id`|String|Requried|The authorization policy ID.|

### Output parameters
{: #iam-auth-policy-detach-output}

This resource does not provide output parameters. 
{: shortdesc}

{[white-space.md]}

## `ibm_iam_service_id`
{: #iam-service-id}

Create, update, or delete an IAM service ID. 
{: shortdesc}

### Sample Terraform code
{: #iam-service-id-sample}

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name        = "test"
  description = "New ServiceID"
}
```

### Input parameters
{: #iam-service-id-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required|The name of the service ID.|
|`description` |String|Optional|The description of the service ID.|
|`tags`|Array of strings|Optional| A list of tags that you want to add to the service ID. The tags are managed locally and not stored on the IBM Cloud service endpoint at this moment.|

### Output parameters
{: #iam-service-id-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id`|String|The unique identifier of the service ID.|
|`version` |String| The version of the service ID.|
|`crn` |String| The CRN of the service ID.|

{[white-space.md]}

## `ibm_iam_service_policy`
{: #iam-service-policy}

Create, update, or delete an IAM service policy. 
{: shortdesc}

### Sample Terraform code
{: #iam-service-policy-sample}

#### Service Policy for All Identity and Access enabled services 

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name = "test"
}

resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "${ibm_iam_service_id.serviceID.id}"
  roles        = ["Viewer"]
}

```

#### Service Policy using service with region

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name = "test"
}

resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "${ibm_iam_service_id.serviceID.id}"
  roles        = ["Viewer"]

  resources = [{
    service = "cloud-object-storage"
  }]
}

```
#### Service Policy using resource instance 

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name = "test"
}

resource "ibm_resource_instance" "instance" {
  name     = "test"
  service  = "kms"
  plan     = "tiered-pricing"
  location = "us-south"
}

resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "${ibm_iam_service_id.serviceID.id}"
  roles        = ["Manager", "Viewer", "Administrator"]

  resources = [{
    service              = "kms"
    resource_instance_id = "${element(split(":",ibm_resource_instance.instance.id),7)}"
  }]
}

```

#### Service Policy using resource group 

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "${ibm_iam_service_id.serviceID.id}"
  roles        = ["Viewer"]

  resources = [{
    service           = "containers-kubernetes"
    resource_group_id = "${data.ibm_resource_group.group.id}"
  }]
}

```

#### Service Policy using resource and resource type 

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "${ibm_iam_service_id.serviceID.id}"
  roles        = ["Administrator"]

  resources = [{
    resource_type = "resource-group"
    resource      = "${data.ibm_resource_group.group.id}"
  }]
}

```

#### Service Policy using attributes 

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "${ibm_iam_service_id.serviceID.id}"
  roles        = ["Administrator"]

  resources = [{
    service = "is"

    attributes = {
      "vpcId" = "*"
    }
    
  }]
}

```

### Input parameters
{: #iam-service-policy-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`iam_service_id`|String|Required|The UUID of the service ID.|
|`roles`|List|Required|A comma separated list of roles. Valid roles are `Writer`, `Reader`, `Manager`, `Administrator`, `Operator`, `Viewer`, and `Editor`.|
|`resources`|List of objects|Optional| A nested block describing the resource of this policy.
|`resources.service` |String|Optional|The service name of the policy definition. You can retrieve the value by running the `ibmcloud catalog service-marketplace` or `ibmcloud catalog search`.|
|`resources.resource_instance_id`|String|Optional| The ID of the resource instance of the policy definition.|
|`resources.region` |String|Optional|The region of the policy definition.|
|`resources.resource_type`|String|Optional| The resource type of the policy definition.|
|`resources.resource`|String|Optional|The resource of the policy definition.|
|`resources.resource_group_id`|String|Optional| The ID of the resource group. To retrieve the value, run `ibmcloud resource groups` or use the `ibm_resource_group` data source. |
|`resources.attributes`|Map|Optional| A set of resource attributes in the format `name=value,name=value`. If you set this option, do not specify `account_management` at the same time.|
|`account_management`|Boolean|Optional|Gives access to all account management services if set to `true`. Default value `false`. If you set this option, do not set `resources` at the same time. |
|`tags` |Array of strings|Optional| A list of tags that are associated with the service policy instance.  Tags are managed locally and not stored on the IBM Cloud service endpoint at this moment.|

### Output parameters
{: #iam-service-policy-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id` |String|The unique identifier of the service policy. The id is composed of `<iam_service_id>/<service_policy_id>`.|
|`version` |String|The version of the service policy.|

### Import
{: #iam-service-policy-import}

The service policy can be imported using the service ID and service policy ID.

```
$ terraform import ibm_iam_service_policy.example <service_ID>/<service_policy_ID>
```

{[white-space.md]}


## `ibm_iam_user_policy`
{: #iam-user-policy}

Create, update, or delete an IAM user policy. To assign a policy to one user, the user must exist in the account to which you assign the policy. 

### Sample Terraform code
{: #iam-user-policy-sample}

#### User Policy for All Identity and Access enabled services 

```hcl
resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Viewer"]
}

```

#### User Policy using service with region

```hcl
resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Viewer"]

  resources = [{
    service = "kms"
  }]
}

```
#### User Policy using resource instance 

```hcl
resource "ibm_resource_instance" "instance" {
  name     = "test"
  service  = "kms"
  plan     = "tiered-pricing"
  location = "us-south"
}

resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Manager", "Viewer", "Administrator"]

  resources = [{
    service              = "kms"
    resource_instance_id = "${element(split(":",ibm_resource_instance.instance.id),7)}"
  }]
}

```

#### User Policy using resource group 

```hcl
data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Viewer"]

  resources = [{
    service           = "containers-kubernetes"
    resource_group_id = "${data.ibm_resource_group.group.id}"
  }]
}

```

#### User Policy using resource and resource type 

```hcl
data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Administrator"]

  resources = [{
    resource_type = "resource-group"
    resource      = "${data.ibm_resource_group.group.id}"
  }]
}

```

#### User Policy using attributes 

```hcl
data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Administrator"]

  resources = [{
    service = "is"

    attributes = {
      "vpcId" = "*"
    }
    
  }]
}

```


### Input parameters
{: #iam-user-policy-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`ibm_id`|String|Required| The IBMid or email address of the user.|
|`roles`|List|Required| A comma separated list of roles. Valid roles are `Writer`, `Reader`, `Manager`, `Administrator`, `Operator`, `Viewer`, and `Editor`.|
|`resources`|List of objects|Optional| A nested block describing the resource of this policy.
|`resources.service` |String|Optional|The service name of the policy definition. You can retrieve the value by running the `ibmcloud catalog service-marketplace` or `ibmcloud catalog search`.|
|`resources.resource_instance_id`|String|Optional| The ID of the resource instance of the policy definition.|
|`resources.region` |String|Optional|The region of the policy definition.|
|`resources.resource_type`|String|Optional| The resource type of the policy definition.|
|`resources.resource`|String|Optional|The resource of the policy definition.|
|`resources.resource_group_id`|String|Optional| The ID of the resource group. To retrieve the value, run `ibmcloud resource groups` or use the `ibm_resource_group` data source. |
|`resources.attributes`|Map|Optional| A set of resource attributes in the format `name=value,name=value`. If you set this option, do not specify `account_management` at the same time.|
|`account_management`|Boolean|Optional|Gives access to all account management services if set to `true`. Default value `false`. If you set this option, do not set `resources` at the same time. |
|`tags` |Array of strings|Optional| A list of tags that are associated with the service policy instance.  Tags are managed locally and not stored on the IBM Cloud service endpoint at this moment.|


### Output parameters
{: #iam-user-policy-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id` |String|The unique identifier of the user policy. The id is composed of `<ibm_id>/<user_policy_id>`.|
|`version` |String|The version of the user policy.|


### Import
{: #iam-user-policy-import}

The user policy can be imported by using the IBMid and user policy ID.

```
$ terraform import ibm_iam_user_policy.example <ibm_id>/<user_policy_ID>
```

{[white-space.md]}

## `ibm_iam_user_invite`
{: #iam-user-invite}

Invite, update, or delete users to your IBM Cloud account. 
{: shortdesc}

### Sample Terraform code
{: #iam-user-invite-sample}

#### Inviting batch of users

```hcl
resource "ibm_iam_user_invite" "invite_user" {
    users = ["test@in.ibm.com"]
}
```

#### Inviting batch of users with access groups

```hcl
resource "ibm_iam_user_invite" "invite_user" {
    users = ["test@in.ibm.com"]
    access_groups = ["accessgroup-id-9876543210"]
}
```

#### Inviting batch of Users with Classic Infrastructure permissions

```hcl
resource "ibm_iam_user_invite" "invite_user" {
    users = ["test@in.ibm.com"]
    classic_infra_roles = {
      permissions = ["PORT_CONTROL", "DATACENTER_ACCESS"]
    }
}
```

#### Inviting batch of users with Classic Infrastructure permission set

```hcl
resource "ibm_iam_user_invite" "invite_user" {
    users = ["test@in.ibm.com"]
    classic_infra_roles = {
      permission_set = "superuser"
    }
}
```

#### Inviting batch of users with user policy for All Identity and Access enabled services

```hcl
resource "ibm_iam_user_invite" "invite_user" {
    users = ["test@in.ibm.com"]
    iam_policy =[{
      roles  = ["Viewer"]
    }]
}
```

#### Inviting batch of users with user policy using service with region

```hcl
resource "ibm_iam_user_invite" "invite_user" {
    users = ["test@in.ibm.com"]
    iam_policy =[{
      roles  = ["Viewer"]
      resources = [{
        service = "kms"
      }]
    }]
}
```

#### Inviting batch of users with user policy using resource instance

```hcl
resource "ibm_resource_instance" "instance" {
  name     = "test"
  service  = "kms"
  plan     = "tiered-pricing"
  location = "us-south"
}

resource "ibm_iam_user_invite" "invite_user" {
    users = ["test@in.ibm.com"]
    iam_policy =[{
      roles  = ["Manager", "Viewer", "Administrator"]
      resources = [{
        service              = "kms"
        resource_instance_id = "${element(split(":",ibm_resource_instance.instance.id),7)}"
      }]
    }]
}
```

#### Inviting batch of users with user policy using resource group

```hcl
data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_user_invite" "invite_user" {
    users = ["test@in.ibm.com"]
    iam_policy =[{
      roles  = ["Manager", "Viewer", "Administrator"]
      resources = [{
        service           = "containers-kubernetes"
        resource_group_id = "${data.ibm_resource_group.group.id}"
      }]
    }]
}
```

#### Inviting batch of users with user policy using resource and resource type

```hcl
data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_user_invite" "invite_user" {
    users = ["test@in.ibm.com"]
    iam_policy =[{
      roles  = ["Manager", "Viewer", "Administrator"]
      resources = [{
        resource_type = "resource-group"
        resource      = "${data.ibm_resource_group.group.id}"
      }]
    }]
}
```

#### Inviting batch of users with user policy using attributes

```hcl
data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_user_invite" "invite_user" {
    users = ["test@in.ibm.com"]
    iam_policy =[{
      roles  = ["Manager", "Viewer", "Administrator"]
      resources = [{
        service = "is"
        attributes = {
          "vpcId" = "*"
        }
      }]
    }]
}
```

#### User invite with access cloud foundry roles

```hcl
provider "ibm" {}

data "ibm_org" "org" {
  org = "${var.org}"
}

data "ibm_space" "space" {
  org   = "${var.org}"
  space = "${var.space}"
}

resource "ibm_iam_user_invite" "invite_user" {
    users = ["test@in.ibm.com"]
    cloud_foundry_roles = [{
      organization_guid = "${data.ibm_org.org.id}"
      org_roles = ["Manager", "Auditor"]
      spaces = [{
          space_guid = "${data.ibm_space.space.id}"
          space_roles = ["Manager", "Developer"]
      }]
    }]
}
```

### Input parameters
{: #iam-user-invite-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`users`|List|Required|A comma separated list of user email IDs.|
|`access_groups`|List|Optional|A comma seperated list of access group IDs.|
|`classic_infra_roles`|Map|Optional|A nested block describing the classic infrastrucre roles for the inviting users. </br></br>**Note**: If you have an IBM Cloud Lite account, you cannot set classic infrastructure roles. For more information about Lite accounts, see [What's available?](/docs/account?topic=account-accounts#lite-account-features).|
|`classic_infra_roles.permissions`|List|Optional|A comma seperated list of classic infrastructure permissions.|
|`classic_infra_roles.permission_set`|String|Optional|The permission set to be applied. The valid permission sets are `noacess`, `viewonly`, `basicuser`, and `superuser`.|
|`iam_policy`|List|Optional|A nested block describing the IAM Polocies for invited users. |
|`iam_policy_roles`|List|Required|A comma separated list of roles. Valid roles are `Writer`, `Reader`, `Manager`, `Administrator`, `Operator`, `Viewer`, and `Editor`.|
|`iam_policy.resources`|List of objects|Optional| A nested block describing the resource of this policy.
|`iam_policy.resources.service` |String|Optional|The service name of the policy definition. You can retrieve the value by running the `ibmcloud catalog service-marketplace` or `ibmcloud catalog search`.|
|`iam_policy.resources.resource_instance_id`|String|Optional| The ID of the resource instance of the policy definition.|
|`iam_policy.resources.region` |String|Optional|The region of the policy definition.|
|`iam_policy.resources.resource_type`|String|Optional| The resource type of the policy definition.|
|`iam_policy.resources.resource`|String|Optional|The resource of the policy definition.|
|`iam_policy.resources.resource_group_id`|String|Optional| The ID of the resource group. To retrieve the value, run `ibmcloud resource groups` or use the `ibm_resource_group` data source. |
|`iam_policy.resources.attributes`|Map|Optional| A set of resource attributes in the format `name=value,name=value`. If you set this option, do not specify `account_management` at the same time.|
|`iam_policy.account_management`|Boolean|Optional|Gives access to all account management services if set to `true`. Default value `false`. If you set this option, do not set `resources` at the same time. |
|`cloud_foundry_roles`|List|Optional|A nested block describing the cloud foundry roles of inviting user. |
|`cloud_foundry_rolesorganization_guid`|String|Required|The ID of the Cloud Foundry organization.|
|`cloud_foundry_roles.org_roles`|List|Required|The orgnization roles that are assigned to invited user. The supported roles are `Manager`, `Auditor`, `BillingManager`.|
|`cloud_foundry_roles.spaces`|List|Optional|A nested block describing the Cloud Foundry space roles and space details.
|`cloud_foundry_roles.spaces.space_guid`|String|Required|The ID of the Cloud Foundry space.|
|`cloud_foundry_roles.spaces.space_roles`|List|Required|The space roles that you want to assign to the invited user. The supported space roles are `Manager`, `Developer`, `Auditor`.|

### Output parameters
{: #iam-user-invite-output}

No output parameters are supported for this resource. 
{: shortdesc}
