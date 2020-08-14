---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-14"

keywords: terraform transit gateway resource, terraform transit gateway, transit gateway resource, transit gateway

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


# Transit Gateway resources
{: #tg-resource}

The {{site.data.keyword.cloud_notm}} [Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started) provides to create,  manage gateways and connections and list available locations for gateways. You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}.

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## `ibm_tg_gateway`
{: #tg-gateway-resource}

Create, update and delete for the transit gateway resource. 
{: shortdesc}

### Sample Terraform code
{: #tg-gatewy-sample}

The following example shows how to create, update and delete a transit gateway by using a transit gateway resource.

```
resource "ibm_tg_gateway" "new_tg_gw"{
  name="transit-gateway-1"
  location="us-south"
  global=true
  resource_group="30951d2dff914dafb26455a88c0c0092"
}

```
{: codeblock}

### Input parameters
{: #tg-gateway-r-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description | Forces new resource |
| ------------- |-------------| ----- | -------------- | ------- |
| `name` | String | Required | The unique user-defined name for the gateway. For example: `myGateway` | No |
| `location` | Integer| Optional | The location of the transit gateway. For example, `us-south` | Yes |
| `global` | Boolean | Required | The gateways with global routing (true) to connect to the networks outside their associated region. | No |
| `resource_group` |  String | Optional | The resource group ID where the transit gateway to be created. | Yes |

### Output parameters
{: #tg-gateway-r-output}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `id` | String | The unique identifier of the gateway ID or connection ID resource.|
| `crn` | String | The CRN of the gateway.|
| `created_at` | String | The date and time the connection is created. | 
| `updated_at` | String | The date and time the connection is last updated. |
| `status` | String | The configuration status of the connection, such as **Available**, **pending**. |

### Import
{: #gateway-import}

`ibm_tg_gateway` can be imported by using transit gateway id and connection id.

**Example**
```
terraform import ibm_tg_gateway.example 5ffda12064634723b079acdb018ef308
```
{: pre}

