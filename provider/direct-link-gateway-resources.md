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
|`bgp_base_cidr`|String|Required|The BGP base CIDR of the gateway to be created. For example: `10.254.30.76/30` | Yes |
|`global`|Boolean|Required|Gateway with global routing as `true` can connect networks outside your associated region. | No |
|`metered`|Boolean|Required|Metered billing option. If set `true` gateway usage is billed per GB. Otherwise, flat rate is charged for the gateway.| No |
|`name`|String|Required|The unique user-defined name for the gateway. For example, `myGateway`.| No |
|`speed_mbps`|Integer|Required|The gateway speed in MBPS. For example, `10.254.30.78/30`.| No |
|`type`|String|Required|The gateway type, allowed values are `dedicated` and `connect`.| Yes |
|`bgp_cer_cidr`|String|Optional|The BGP customer edge router CIDR. Specify a value within bgp_base_cidr. If bgp_base_cidr is `169.254.0.0/16`, this parameter can be ommitted and a CIDR is selected automatically. For example, `10.254.30.78/30`.| Yes |
|`bgp_ibm_cidr`|String|Optional|The {{site.data.keyword.IBM_notm}} BGP ASN. Specify a value within bgp_base_cidr. If bgp_base_cidr is `169.254.0.0/16`, this parameter can be ommitted and a CIDR is selected automatically. For example: `10.254.30.77/30`.| Yes |
|`resource_group`|String|Optional|The resource group. If unspecified, the account's default resource group is used.| Yes |
|`carrier_name`|String|Required|The carrier name. Constraints are 1 ≤ length ≤ 128, Value must match regular expression ^[a-z][A-Z][0-9][ -_]$. For example, `myCarrierName`.| Yes |
|`cross_connect_router`|String|Required|The cross connect router. For example, `xcr01.dal03`.| Yes |
|`customer_name`|String|Required|The customer name is required for `dedicated` type. Constraints are 1 ≤ length ≤ 128, Value must match regular expression ^[a-z][A-Z][0-9][ -_]$. For example, `newCustomerName`.| Yes |
|`location_name`|String|Required|The gateway location is required for `dedicated` type. For example, `dal03`.| Yes |


### Output parameters
{: #dl-gwy-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique ID of the gateway.|
|`name`|String|The unique user-defined name for the gateway |
|`crn`|String|The CRN of the gateway.|
|`created_at`|String|The date and time resource created.|
|`location_display_name`|String|The gateway location long name.|
|`resource_group`|String|The resource group reference.|
|`bgp_asn`|String|The {{site.data.keyword.IBM.notm}} BGP ASN.|
|`bgp_status`|String|The gateway BGP status.|
|`completion_notice_reject_reason`|String|The reason for completion notice rejection.|
|`link_status`|String|The gateway link status. You can include only on `type=dedicated` gateways. For example, `down`, `up`.|
|`port`|String|The gateway port for `type=connect` gateways.|
|`vlan`|String|The VLAN allocated for the gateway. You can set only for `type=connect` gateways created directly through the {{site.data.keyword.IBM_notm}} portal.|
|`provider_api_managed`|String|Indicates whether gateway changes need to be made via a provider portal.|
|`operational_status`|String|The gateway operational status. For gateways pending LOA approval, patch operational_status to the appropriate value to approve or reject its LOA. For example, `loa_accepted`.|

Operational_status(gateway operational status) and loa_reject_reason(LOA reject reason) cannot be updated using Terraform as the status and reason keeps changing with the different workflow actions.
{: note}

### Import
{: #dl-gwy-import}

The `ibm_dl_gateway` can be imported using gateway id. 

**Example**

```
terraform import ibm_dl_gateway.example 5ffda12064634723b079acdb018ef308
```
{: pre}






