---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-15"

keywords:  direct link gateway, terraform direct link gateway, terraform direct link gateway data sources


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


# Direct Link Gateway resources
{: #dl-gateway-resource}

Use {{site.data.keyword.cloud_notm}} [Direct Link](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) to seamlessly connect your on-premises resources to your cloud resources. The speed and reliability of direct link extends your organization’s data center network. You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}.
{: shordesc}

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


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


# {{site.data.keyword.databases-for}} resources
{: #databases-resources}

Use [{{site.data.keyword.cloud_notm}} direct link](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) to seamlessly connect your on-premises resources to your cloud resources. The speed and reliability of Direct Link extends your organization’s data center network and offers more consistent, higher-throughput connectivity, keeping traffic within the IBM Cloud network. You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 
{: shortdesc}

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## `ibm_dl_gateway`
{: #dl-gwy}

Create, update, or delete a direct link gateway using the direck link gateway resource.
{: shortdesc}

### Sample Terraform code
{: #dl-gwy-sample}

```
data "ibm_dl_routers" "test_dl_routers" {
		offering_type = "dedicated"
		location_name = "dal10"
	}

resource ibm_dl_gateway test_dl_gateway {
  bgp_asn =  64999
  bgp_base_cidr =  "169.254.0.0/16"
  bgp_ibm_cidr =  "169.254.0.29/30"
  bgp_cer_cidr =  "169.254.0.30/30"
  global = true 
  metered = false
  name = "Gateway1"
  resource_group = "bf823d4f45b64ceaa4671bee0479346e"
  speed_mbps = 1000 
  type =  "dedicated" 
  cross_connect_router = data.ibm_dl_routers.test_dl_routers.cross_connect_routers[0].router_name
  location_name = data.ibm_dl_routers.test_dl_routers.location_name
  customer_name = "Customer1" 
  carrier_name = "Carrier1"

} 
```
{: pre}

### Input parameters
{: #dl-gwy-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}


|Name|Data type|Required/ optional|Description| Forces new resource |
|----|-----------|-----------|---------------------| --------|
|`bgp_asn`|Integer|Required|The BGP ASN of the gateway to be created. For example: `64999`.| Yes |
|`bgp_base_cidr`|String|Required|The BGP base CIDR of the gateway to be created. For example: `10.254.30.76/30` | No |
|`global`|Boolean|Required|Gateway with global routing as `true` can connect networks outside your associated region. | No |


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
|`connectionstrings`|Array|A list of connection strings for the database for each user ID. For more information about how to use connection strings, see the [documentation](/docs/databases-for-postgresql?topic=databases-for-postgresql-connection-strings). The results are returned in pairs of the userid and string: `connectionstrings.1.name = admin connectionstrings.1.string = postgres://admin:$PASSWORD@79226bd4-4076-4873-b5ce-b1dba48ff8c4.b8a5e798d2d04f2e860e54e5d042c915.databases.appdomain.cloud:32554/ibmclouddb?sslmode=verify-full` Individual string parameters can be retrieved using Terraform variables and outputs `connectionstrings.x.hosts.x.port` and `connectionstrings.x.hosts.x.host`|

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





