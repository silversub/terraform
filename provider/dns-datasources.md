---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-10"

keywords: terraform provider plugin, terraform dns service, terraform dns, terraform private dns

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

# DNS services data sources 
{: #dns-data-sources}

You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 

Before you start working with your data source, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}

## `ibm_dns_permitted_networks`
{: #dns-permitted-network}

Retrieve details about permitted networks for a zone that is associated with the private DNS service instance. 
{: shortdesc}

### Sample Terraform code
{: #dns-permitted-network-sample}

```
data "ibm_resource_group" "rg" {
  name = "default"
}

resource "ibm_is_vpc" "test_pdns_vpc" {
  name           = "test-pdns-vpc"
  resource_group = data.ibm_resource_group.rg.id
}

resource "ibm_resource_instance" "test-pdns-instance" {
  name              = "test-pdns"
  resource_group_id = data.ibm_resource_group.rg.id
  location          = "global"
  service           = "dns-svcs"
  plan              = "standard-dns"
}

resource "ibm_dns_zone" "test-pdns-zone" {
  name        = "test.com"
  instance_id = ibm_resource_instance.test-pdns-instance.guid
  description = "testdescription"
  label       = "testlabel-updated"
}

resource "ibm_dns_permitted_network" "test-pdns-permitted-network-nw" {
  instance_id = ibm_resource_instance.test-pdns-instance.guid
  zone_id     = ibm_dns_zone.test-pdns-zone.zone_id
  vpc_crn     = ibm_is_vpc.test_pdns_vpc.crn
}

data "ibm_dns_permitted_networks" "test" {
  instance_id = ibm_dns_permitted_network.test-pdns-permitted-network-nw.instance_id
  zone_id     = ibm_dns_permitted_network.test-pdns-permitted-network-nw.zone_id
}
```
{: codeblock}

### Input parameters
{: #dns-permitted-network-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
|`instance_id`|String|Required|The ID of the private DNS service instance where you created permitted networks.|
|`zone_id`|String|Required|The ID of the zone where you added the permitted networks.|


### Output parameters
{: #dns-permitted-network-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`permitted_networks`|List of permitted networks|A list of all permitted networks that were created for a zone in your private DNS instance. |
|`permitted_networks.created_on`|Timestamp|The date and time when the permitted network was created.|
|`permitted_networks.instance_id`|String|The ID of the private DNS service instance where you created permitted networks.
|`permitted_networks.modified_on`|Timestamp|The date and time when the permitted network was updated.|
|`permitted_networks.permitted_network`|List of VPCs|A list of VPC CRNs that are associated with the permitted network.| 
|`permitted_networks.permitted_network.vpc_crn`|String|The CRN of the VPC that the permitted network belongs to. | 
|`permitted_networks.permitted_network_id`|String|The ID of the permitted network.|
|`permitted_networks.state`|String|The state of the permitted network.| 
|`permitted_networks.type`|String|The type of the permitted network.|
|`permitted_networks.zone_id`|String|The ID of the zone where you added the permitted network.|

## `ibm_dns_resource_records`
{: #dns-record}

Retrieve details about existing IBM Cloud private domain name service records. 
{: shortdesc}

### Sample Terraform code
{: #dns-record-template}

```
data "ibm_dns_resource_records" "ds_pdns_resource_records" {
  instance_id = "resource_instance_guid"
  zone_id = "resource_dns_resource_records_zone_id"
}
```
{: codeblock}

### Input parameters
{: #dns-record-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
|`instance_id`|String|Required|The ID of the private DNS service instance.|
|`zone_id`|String|Required|The ID of the zone that you added to the private DNS service instance.|

### Output parameters
{: #dns-record-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`dns_resource_records`|List of DNS records|A list of all private domain name service resource records. |
|`dns_resource_records.id`|String|The unique identifier of the private DNS resource record.|
|`dns_resource_records.name`|String|The name of a private DNS resource record.|
|`dns_resource_records.type`|String|The type of the private DNS resource record. Supported values are `A`, `AAAA`, `CNAME`, `PTR`, `TXT`, `MX`, and `SRV`.|
|`dns_resource_records.rdata`|String|The resource data of a private DNS resource record.
|`dns_resource_records.ttl`|Integer|The time-to-live value of the DNS resource record.|

## `ibm_dns_zones`
{: #dns-zones}

Retrieve details about a zone that you added to your private DNS service instance.
{: shortdesc}

### Sample Terraform code
{: #dns-zones-sample}

```
data "ibm_dns_zones" "ds_pdnszones" {
  instance_id = "resource_instance_guid"
}
```
{: codeblock}

### Input parameters
{: #dns-zones-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
|`instance_id`|String|Required|The ID of the private DNS service instance.|

### Output parameters
{: #dns-zones_output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`dns_zones`|List of zones|A List of zones that you added to your private DNS service instance.| 
|`dns_zones.zone_id`|String|The ID of the zone.|
|`dns_zones.instance_id`|String|The ID of the private DNS service instance where you added the zone.|
|`dns_zones.description`|String|The description of the zone.|
|`dns_zones.name`|String|The name of the zone.|
|`dns_zones.label`|String|The label of the zone.|
|`dns_zones.created_on`|Timestamp|The date and time when the zone was added to the private DNS service instance.|
|`dns_zones.modified_on`|Timestamp|The date and time when the zone was updated.|
|`dns_zones.state`|String|The state of the zone.|


