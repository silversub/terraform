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


# Direct Link Gateway data sources
{: #dl-gateway-ds}

Use {{site.data.keyword.cloud_notm}} [Direct Link](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) to seamlessly connect your on-premises resources to your cloud resources. You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}.
{: shordesc}

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## ibm_dl_gateway
{: #dl_gateway_ds}

Import the details of an existing {{site.data.keyword.cloud_notm}} infrastructure direct link gateway and its virtual connections.
{: shortdesc}

### Sample Terraform code
{: #dl-gw-dssample}

```
      data "ibm_dl_gateway" "test_dl_gateway_vc" {
			name = "mygateway"
		 }"
     
```
{: codeblock}

### Input parameters
{: #api-dl-gw-dsinput}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
|`name`|String|Required|The unique user-defined name for the gateway. |


### Output parameters
{: #dl-gw-dsoutput}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`bgp_asn`|String|Customer BGP ASN.|
|`created_at`|String|The date and time resource is created.|
|`crn`|String|The CRN of the gateway.|
|`global`|Boolean|Gateway with global routing as `true` can connect networks outside your associated region.|
|`id`|String|The unique identifier of the gateway.|
|`location_display_name`|String|Long name of the gateway location.|
|`location_name`|String|The location name of the gateway.|
|`metered`|String|Metered billing option. If set `true` gateway usage is billed per GB. Otherwise, flat rate is charged for the gateway.|
|`operational_status`|String|The gateway operational status|
|`resource_group`|String|The resource group identifier.|
|`speed_mbps`|String|The gateway speed in MBPS.|
|`type`|String|The gateway type.|
|`bgp_base_cidr`|String|The BGP base CIDR.|
|`bgp_cer_cidr`|String|The BGP customer edge router CIDR.|
|`bgp_ibm_asn`|String|The {{site.data.keyword.IBM_notm}} BGP ASN.|
|`bgp_ibm_cidr`|String|The {{site.data.keyword.IBM_notm}} BGP  CIDR.|
|`bgp_status`|String|The gateway BGP status.|
|`completion_notice_reject_reason`|String| The reason for completion notice rejection. Only included on a dedicated gateways type with a rejected completion notice.|
|`cross_connect_router`|String| The cross connect router. Only included on a dedicated gateways type. |
|`link_status` |String| The gateway link status. Only included on a dedicated gateways type.
|`port`|Integer|The port identifier.|
|`provider_api_managed`|Boolean|Indicates the gateway is created through a provider portal. If set `true`, gateway can only be changed. If set `false`, gateway is deleted through the corresponding provider portal.|
|`vlan`|String| The VLAN allocated for the gateway. Only set for connect gateways type created directly through the {{site.data.keyword.IBM_notm}} portal.|
|`virtual_connections`|String|List of the specified gateway's virtual connections.|
|`virtual_connections.created_at`|String|The creation date and time resource.|
|`virtual_connections.id`|String|The unique identifier of the virtual connection. For Example, `ef4dcbtyu1a-fee4-41c7-9e11-9cd99e65c1f4`|
|`virtual_connections.name`|String|The unique user-defined name of the only virtual connection in the gateway.| 
|`virtual_connections.status`|String| The status of the virtual connection. Possible values are `pending`,`attached`,`approval_pending`,`rejected`,`expired`,`deleting`,`detached_by_network_pending`,`detached_by_network`.|
|`virtual_connections.type`|String|The virtual connection type. Possible values are `classic`,`vpc`.|
|`virtual_connections.network_account`|String|For virtual connections across two different {{site.data.keyword.cloud_notm}} accounts. Network_account indicates the account you own the target network. For Example: `00aa14a2e0fb102c8995ebefhhhf8655556`
|`virtual_connections.network_id`|String| The unique identifier of the target network. For type `vpc`, virtual connections is the CRN of the target VPC. This field do not apply for type `classic` connections. For Example, `crn:v1:bluemix:public:is:us-east:a/28e4d90ac7504be69447111122223333::vpc:aaa81ac8-5e96-42a0-a4b7-6c2e2dbb`|

## ibm_dl_gateways
{: #dl_gateways_ds}

Import the details of an existing {{site.data.keyword.cloud_notm}} infrastructure direct link gateway and its virtual connections.
{: shortdesc}

### Sample Terraform code
{: #dl-gws-dssample}

```
data "ibm_dl_gateways" "ds_dlgateways" {
}
     
```
{: codeblock}

### Input parameters
{: #api-dl-gws-dsinput}

There is no input parameters that you need to specify for the data source. 
{: shortdesc}

### Output parameters
{: #dl-gws-dsoutput}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`gateways`|String|List of all the direct link gateways in the the {{site.data.keyword.cloud_notm}} infrastructure.|
|`gateways.bgp_asn`|String|Customer BGP ASN.|
|`gateways.created_at`|String|The date and time resource is created.|
|`gateways.crn`|String|The CRN of the gateway.|
|`gateways.global`|Boolean|Gateway with global routing as `true` can connect networks outside your associated region.|
|`gateways.id`|String|The unique identifier of the gateway.|
|`gateways.location_display_name`|String|Long name of the gateway location.|
|`gateways.location_name`|String|The location name of the gateway.|
|`gateways.metered`|String|Metered billing option. If set `true` gateway usage is billed per GB. Otherwise, flat rate is charged for the gateway.|
|`gateways.name`|String| The uniqure user defined name of the gateway.|
|`gateways.operational_status`|String|The gateway operational status|
|`gateways.resource_group`|String|The resource group identifier.|
|`gateways.speed_mbps`|String|The gateway speed in MBPS.|
|`gateways.type`|String|The gateway type.|
|`gateways.bgp_base_cidr`|String|The BGP base CIDR.|
|`gateways.bgp_cer_cidr`|String|The BGP customer edge router CIDR.|
|`gateways.bgp_ibm_asn`|String|The {{site.data.keyword.IBM_notm}} BGP ASN.|
|`gateways.bgp_ibm_cidr`|String|The {{site.data.keyword.IBM_notm}} BGP  CIDR.|
|`gateways.bgp_status`|String|The gateway BGP status.|
|`gateways.completion_notice_reject_reason`|String| The reason for completion notice rejection. Only included on a dedicated gateways type with a rejected completion notice.|
|`gateways.cross_connect_router`|String| The cross connect router. Only included on a dedicated gateways type. |
|`gateways.link_status` |String| The gateway link status. Only included on a dedicated gateways type.
|`gateways.port`|Integer|The port identifier.|
|`gateways.provider_api_managed`|Boolean|Indicates the gateway is created through a provider portal. If set `true`, gateway can only be changed. If set `false`, gateway is deleted through the corresponding provider portal.|
|`gateways.vlan`|String| The VLAN allocated for the gateway. Only set for connect gateways type created directly through the {{site.data.keyword.IBM_notm}} portal.|
