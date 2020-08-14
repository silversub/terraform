---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-14"

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


## `ibm_dl_connection`
{: #dl-connection}

Create, update and delete for the transit gateway connection resource. 
{: shortdesc}

### Sample Terraform code
{: #dl-connection-sample}

The following example shows how to create, update and delete a transit gateway connection by using a transit gateway resource.

```
resource "ibm_tg_connection" "test_ibm_tg_connection"{
  gateway = ibm_tg_gateway.test_tg_gateway.id
  network_type = "vpc"
  name= "myconnection"
  network_id = ibm_is_vpc.test_tg_vpc.resource_crn
}

```
{: codeblock}

### Input parameters
{: #dl-connection-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description | Forces new resource |
| ------------- |-------------| ----- | -------------- | ------- |
| `gateway` | String | Required | Enter the transit gateway identifier. | Yes |
| `name` | String| Optional | Enter a name. If unentered, the default name is provided based on the network type, such as `vpc` for network type VPC and `classic` for network type classic. | No|
| `network_type` | String | Required | Enter the network type. Allowed values are `classic` and `vpc`. | Yes |
| `network_id` |  String | Optional | Enter the ID of the network being connected through this connection. This parameter is required for network type 'vpc', it is CRN of the VPC to be connected. This field is required to be unspecified for network type 'classic'. For example: crn:v1:bluemix:public:is:us-south:a/123456::vpc:4727d842-f94f-4a2d-824a-9bc9b02c523b | Yes |

### Output parameters
{: #dl-connection-output}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `id` | String | The unique identifier of the gateway ID or connection ID resource.|
| `connection_id` | String | The unique identifier for transit gateway connection to network. |
| `created_at` | String |The date and time the connection was created. | 
| `updated_at` | String | Last updated date and time of the connection. |
| `status` | String | The configuration status of the connection, such as **attached**, **failed**, **pending**, **deleting**. |

### Import
{: #connection-import}

`ibm_tg_connection` can be imported by using transit gateway id and connection id.

**Example**
```
terraform import ibm_tg_connection.example 5ffda12064634723b079acdb018ef308/cea6651a-bd0a-4438-9f8a-a0770bbf3ebb
```
{: pre}


