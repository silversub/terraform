---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-10"

keywords: terraform provider plugin, terraform dns, terraform vpc dns, terraform private dns

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

# DNS services resources
{: #dns-resources}

Review the IBM Cloud DNS service resources that you can create, modify, or delete. You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 
{: shortdesc}

For more information, about IBM Cloud DNS service, see [About DNS services](/docs/dns-svcs?topic=dns-svcs-about-dns-services).

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}
 
## `ibm_dns_permitted_network`
{: #dns-permitted-network}

Create or delete a DNS permitted network. For more information, see [Managing permitted networks](/docs/dns-svcs?topic=dns-svcs-managing-permitted-networks).
{: shortdesc}

You can add a VPC as a permitted network to a DNS entry only. 
{: note}

### Sample Terraform code
{: #dns-permitted-network-sample}

```
resource "ibm_dns_permitted_network" "test-pdns-permitted-network-nw" {
    instance_id = ibm_resource_instance.test-pdns-instance.guid
    zone_id = ibm_dns_zone.test-pdns-zone.zone_id
    vpc_crn = ibm_is_vpc.test_pdns_vpc.crn
    type = "vpc"
}
```

### Input parameters
{: #dns-permitted-network-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required / optional|Description|
|----|-----------|-----------|---------------------|
|`instance_id`|String|Required|The ID of the IBM Cloud DNS service instance where you want to add a permitted network.|
|`zone_id`|String|Required|The ID of the private DNS zone where you want to add the permitted network.|
|`vpc_crn`|String|Required|The CRN of the VPC that you want to add as a permitted network.|
|`type`|String|Required|The type of permitted network that you want to add. Supported values are `vpc`.|

### Output parameters
{: #dns-permitted-network-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the DNS private network. The ID is composed of `<instance_ID>/<zone_ID>/<permitted_network_ID>`. |
|`created_on`|Timestamp|The time when the permitted network was added to the DNS.|
|`modified_on`|Timestamp|The time when the permitted network was modified.|

## `ibm_dns_resource_record`
{: #dns-record}

Create, update, or delete a DNS record. For more information, see [Managing DNS records](/docs/dns-svcs?topic=dns-svcs-managing-dns-records). 
{: shortdesc}

### Sample Terraform code
{: #dns-record-sample}

```
resource "ibm_dns_resource_record" "test-pdns-resource-record-a" {
  instance_id = ibm_resource_instance.test-pdns-instance.guid
  zone_id     = ibm_dns_zone.test-pdns-zone.zone_id
  type        = "A"
  name        = "testA"
  rdata       = "1.2.3.4"
  ttl         = 3600
}

resource "ibm_dns_resource_record" "test-pdns-resource-record-aaaa" {
  instance_id = ibm_resource_instance.test-pdns-instance.guid
  zone_id     = ibm_dns_zone.test-pdns-zone.zone_id
  type        = "AAAA"
  name        = "testAAAA"
  rdata       = "2001:0db8:0012:0001:3c5e:7354:0000:5db5"
}

resource "ibm_dns_resource_record" "test-pdns-resource-record-cname" {
  instance_id = ibm_resource_instance.test-pdns-instance.guid
  zone_id     = ibm_dns_zone.test-pdns-zone.zone_id
  type        = "CNAME"
  name        = "testCNAME"
  rdata       = "test.com"
}

resource "ibm_dns_resource_record" "test-pdns-resource-record-ptr" {
  instance_id = ibm_resource_instance.test-pdns-instance.guid
  zone_id     = ibm_dns_zone.test-pdns-zone.zone_id
  type        = "PTR"
  name        = "1.2.3.4"
  rdata       = "testA.test.com"
}

resource "ibm_dns_resource_record" "test-pdns-resource-record-mx" {
  instance_id = ibm_resource_instance.test-pdns-instance.guid
  zone_id     = ibm_dns_zone.test-pdns-zone.zone_id
  type        = "MX"
  name        = "testMX"
  rdata       = "mailserver.test.com"
  preference  = 10
}

resource "ibm_dns_resource_record" "test-pdns-resource-record-srv" {
  instance_id = ibm_resource_instance.test-pdns-instance.guid
  zone_id     = ibm_dns_zone.test-pdns-zone.zone_id
  type        = "SRV"
  name        = "testSRV"
  rdata       = "tester.com"
  priority    = 100
  weight      = 100
  port        = 8000
  service     = "_sip"
  protocol    = "udp"
}

resource "ibm_dns_resource_record" "test-pdns-resource-record-txt" {
  instance_id = ibm_resource_instance.test-pdns-instance.guid
  zone_id     = ibm_dns_zone.test-pdns-zone.zone_id
  type        = "TXT"
  name        = "testTXT"
  rdata       = "textinformation"
  ttl         = 900
}
```
{: codeblock}

### Input parameters
{: #dns-record-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required / optional|Description|
|----|-----------|-----------|---------------------|
|`instance_id`|String|Required|The ID of the IBM Cloud DNS service instance where you want to create the DNS record.|
|`zone_id`|String|Required|The ID of the DNS zone where you want to create a DNS record.|
|`type`|String|Required|The type of DNS record that you want to create. Supported values are `A`, `AAAA`, `CNAME`, `PTR`, `TXT`, `MX`, and `SRV`.|
|`name`|String|Required|The name of the DNS record.| 
|`rdata`|String|Required|The resource data of a DNS resource record.
|`ttl`|Integer|Optional|The time to live (TTL) in minutes that the resolved DNS record is cached before the associated IP address must be retrieved again. The minimum TTL must be 1 minute and can be 12 hours at a maximum. If no value is specified, 15 minutes is used by default. To find the default TTL values for each record type, see [Adding DNS records](/docs/dns-svcs?topic=dns-svcs-managing-dns-records#adding-dns-records).|
|`preference`|Integer|Required for `MX` records|If you create an `MX` record, enter the preference of the record.|
|`priority`|Integer|Required for `SRV` records|If you create an `SRV` record, enter the priority of the record.|
|`weight`|Integer|Required for `SRV` records|If you create an `SRV` record, enter the weight of the record. The weight is considered when multiple records with the same priority exist. A higher value is associated with a higher weight and a higher chance of being considered among records with the same priority.|
|`port`|Integer|Required for `SRV` records|If you create an `SRV` record, enter the TCP or UDP port of the target server. |
|`service`|String|Required for `SRV` records|If you create an `SRV` record, enter the name of the service that you want. The name must start with an underscore (`_`).|
|`protocol`|String|Required for `SRV` records|If you create an `SRV` record, enter the name of the protocol that you want. |

### Output parameters
{: #dns-record-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the DNS record. The ID is composed of `<instance_id>/<zone_id>/<dns_record_id>`.|
|`zone_id`|String|The ID of the zone where the DNS record was created.| 
|`resource_record_id`|String|The ID of the DNS record.| 
|`created_on`|Timestamp|The time when the DNS record was created.| 
|`modified_on`|Timestamp|The time when the DNS record was modified.|

### Import
{: #dns-record-import}

The resource can be imported by using the DNS record ID, zone ID and resource record ID. 

```
terraform import ibm_dns_resource_record.example <instance_id>/<zone_id>/<dns_record_id>
```
{: pre}





## `ibm_dns_zone`
{: #dns-zone}

Create, update, or delete a DNS zone. For more information, see [Managing DNS zones](/docs/dns-svcs?topic=dns-svcs-managing-dns-zones).
{: shortdesc}

### Sample Terraform code
{: #dns-zone-sample}

```
resource "ibm_dns_zone" "pdns-1-zone" {
    name = "test.com"
    instance_id = p-dns-instance-id
    description = "testdescription"
    label = "testlabel"
}
```
{: codeblock}

### Input parameters
{: #dns-zone-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required / optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required|The name of the DNS zone that you want to create.| 
|`instance_id`|String|Required|The ID of the IBM Cloud DNS service instance where you want to create a DNS zone.|
|`description`|String|Optional|The description of the DNS zone.|
|`label`|String|Optional|The label of the DNS zone.| 

### Output parameters
{: #dns-zone-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the DNS zone. The ID is composed of `<instance_id>/<zone_id>`.|
|`zone_id`|String|The ID of the zone that is associated with the DNS zone. |
|`created_on`|Timestamp|The time when the DNS zone was created.| 
|`modified_on`|Timestamp|The time when the DNS zone was updated.| 
|`state`|String|The state of the DNS zone.|




