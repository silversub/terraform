---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-31"

keywords: terraform provider plugin, terraform cloud databases, terraform databases, terraform postgres, terraform mysql, terraform compose

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

# {{site.data.keyword.databases-for}} resources
{: #databases-resources}

Review the {{site.data.keyword.databases-for}} resources that you can create, modify, or delete. You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 
{: shortdesc}

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## `ibm_database`
{: #db}

Create, update, or delete a {{site.data.keyword.databases-for}} instance. The `ibmcloud_api_key` that is defined in the `provider` block and used by Terraform must have sufficient IAM rights to create and modify {{site.data.keyword.databases-for}} instances and must have access to the resource group where you want to deploy the instance. For more information, see the [documentation](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-user-management).

To create a {{site.data.keyword.databases-for}} instance, you must specify the `region` and `ibmcloud_api_key` parameters in the `provider` block of your Terraform configuration file. The region must match the region where you want to deploy your instance. If the region is not specified `us-south` is used by default. 
{: note}

### Sample Terraform code
{: #db-sample}

To find an example for configuring a virtual server instance that connects to a PostgresDB database, see [here](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-database).

```
data "ibm_resource_group" "group" {
  name = "<your_group>"
}

resource "ibm_database" "<your_database>" {
  name              = "<your_database_name>"
  plan              = "standard"
  location          = "eu-gb"
  service           = "databases-for-etcd"
  resource_group_id = data.ibm_resource_group.group.id
  tags              = ["tag1", "tag2"]

  adminpassword                = "password12"
  members_memory_allocation_mb = 3072
  members_disk_allocation_mb   = 61440
  users {
    name     = "user123"
    password = "password12"
  }
  whitelist {
    address     = "172.168.1.1/32"
    description = "desc"
  }
}

output "ICD Etcd database connection string" {
  value = http://ibm_database.test_acc.connectionstrings[0].composed
}
```

```
provider "ibm" {
  ibmcloud_api_key = var.ibmcloud_api_key
  region           = "eu-gb"
}
```

### Input parameters
{: #db-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required|A descriptive name that is used to identify the database instance. The name must not include spaces.|
|`plan`|String|Required|The name of the service plan that you choose for your instance. Supported values are `standard`. |
|`location`|String|Required|The location where you want to deploy your instance. The location must match the `region` parameter that you specify in the `provider` block of your Terraform configuration file. |
|`resource_group_id`|String|Optional| The ID of the resource group where you want to create the instance. To retrieve this value, run `ibmcloud resource groups` or use the `ibm_resource_group` data source. If no value is provided, the `default` resource group is used.|
|`tags`|Array of strings|Optional|A list of tags that you want to add to your instance. |
|`service`|String|Required| The type of {{site.data.keyword.databases-for}} that you want to create. Only the following services are currently accepted: `databases-for-etcd`, `databases-for-postgresql`, `databases-for-redis`, `databases-for-elasticsearch`, `messages-for-rabbitmq`, and `databases-for-mongodb`.|
|`version`|String|Optional|The version of the database to be provisioned. If omitted, the database is created with the most recent major and minor version.|
|`adminpassword`|String|Optional| The password for the database administrator. If not specified, an empty string is provided for the password and the user ID cannot be used. In this case, additional users must be specified in a `user` block.|
|`members_memory_allocation_mb`|Integer|Optional| The amount of memory in megabytes for the database, split across all members. If not specified, the default setting of the database service is used, which can vary by database type. |
|`members_disk_allocation_mb`|Integer|Optional|The amount of disk space for the database, split across all members. If not specified, the default setting of the database service is used, which can vary by database type. |
|`members_cpu_allocation_count`|Integer|Optional|Enables and allocates the number of specified dedicated cores to your deployment.|
|`backup_id`|String|Optional|The CRN of a backup resource to restore from. The backup must have been created by a database deployment with the same service ID. The backup is loaded after provisioning and the new deployment starts up that uses that data. A backup CRN is in the format `crn:v1:<…>:backup:`. If omitted, the database is provisioned empty.|
|`key_protect_key`|String|Optional|The CRN of a Key Protect root key that you want to use for disk encryption. A key protect CRN is in the format `crn:v1:<…>:key:`.|
|`key_protect_instance`|String|Optional|The CRN of a Key Protect instance that you want to use for disk encryption. A key protect CRN is in the format `crn:v1:<…>::`.|
|`guid`|String|Optional|The unique identifier of the database instance.|
|`remote_leader_id`|String|Optional|A CRN of the leader database to make the replica(read-only) deployment. The leader database must have been created by a database deployment with the same service ID. A read-only replica is set up to replicate all of your data from the leader deployment to the replica deployment using asynchronous replication. For more information, see [Configuring Read-only Replicas](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas).|
|`service_endpoints`|String|Optional|Specify if you want to enable the public, private, or both service endpoints. Supported values are `public`, `private`, or `public-and-private`. The default is `public`.|
|`users`|List of objects|Optional|A list of users that you want to create on the database. Multiple blocks are allowed. |
|`users.name`|String|Optional|The of the user ID to add to the database instance. The user ID must be between 5 and 32 characters.|
|`users.password`|String|Optional|The password for the user ID. The password must be between 10 and 32 characters.|
|`whitelist`|List of objects|Optional|A list of IP addresses to whitelist for the database. Multiple blocks are allowed. |
|`whitelist.address`|String|Optional|The IP address or range of database client addresses to be whitelisted in CIDR format. Example: `172.168.1.2/32`.|
|`whitelist.description`|String|Optional|A description for the whitelist range. |

### Output parameters
{: #db-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The CRN of the database instance.|
|`status`|String|The status of the instance. |
|`adminuser`|String|The user ID of the database administrator. Example: `admin` or `root`.|
|`version`|String|The database version.|
|`connectionstrings`|Array|A list of connection strings for the database for each user ID. For more information about how to use connection strings, see the [documentation](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-connection-strings). The results are returned in pairs of the userid and string: `connectionstrings.1.name = admin connectionstrings.1.string = postgres://admin:$PASSWORD@79226bd4-4076-4873-b5ce-b1dba48ff8c4.b8a5e798d2d04f2e860e54e5d042c915.databases.appdomain.cloud:32554/ibmclouddb?sslmode=verify-full` Individual string parameters can be retrieved using Terraform variables and outputs `connectionstrings.x.hosts.x.port` and `connectionstrings.x.hosts.x.host`|

### Timeouts
{: #db-timeout}

The following timeouts are defined for this resource. 
{: shortdesc}

- **Create:** The creation of the instance is considered failed when no response is received for 60 minutes.
- **Update:** The update of the instance is considered failed when no response is received for 20 minutes.
- **Delete:** The deletion of the instance is considered failed when no response is received for 10 minutes.

### Import
{: #db-import}

The database instance can be imported using the CRN. To import the resource, you must specify the `region` parameter in the `provider` block of your Terraform configuration file. If the region is not specified, `us-south` is used by default. 

```
terraform import ibm_database.my_db <crn>
```
{: pre}

Import requires a minimal Terraform config file to allow importing.

```
resource "ibm_database" "<your_database>" {
  name              = "<your_database_name>"
```

Run `terraform state show ibm_database.<your_database>` after import to retrieve the additional values to be included in the resource config file. Note that ICD only exports the admin userid. It does not export any additional user IDs and passwords configured on the instance. These values must be retrieved from an alternative source. If new passwords need to be configured or the connection string retrieved to use the service, a new users block must be defined to create new users. This limitation is due to a lack of ICD functionality.
