---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-07"

keywords: terraform provider plugin, terraform event streams, terraform event stream service, terraform event

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


# Event Streams resources
{: #event-streams-resources}


Review the [Event Streams](/docs/EventStreams?topic=EventStreams-about) resource that you can connect, administer, develope with event streams and integrate with the other services. You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration/resources.html){: external}.
{: shortdesc}

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## `ibm_event_streams
{: #event-streams}

Create and update the event streams topic.
{: shortdesc}

### Sample1 Terraform code
{: #event-stream-sample}

Create event streams service instance and topic.
{: shortdesc}

```
resource "ibm_resource_instance" "es_instance_1" {
  name              = "terraform-integration-1"
  service           = "messagehub"
  plan              = "standard" # "lite", "enterprise-3nodes-2tb"
  location          = "us-south" # "us-east", "eu-gb", "eu-de", "jp-tok", "au-syd"
  resource_group_id = data.ibm_resource_group.group.id
}

resource "ibm_event_streams_topic" "es_topic_1" {
  resource_instance_id = ibm_resource_instance.es_instance_1.id
  name                 = "my-es-topic"
  partitions           = 1
  config = {
    "cleanup.policy"  = "compact,delete"
    "retention.ms"    = "86400000"
    "retention.bytes" = "1073741824"
    "segment.bytes"   = "536870912"
  }
}

```

### Sample2 Terraform code
{: #event-stream-sample2}

Create topic on an existing Event Streams instance.
{: shortdesc}

```
data "ibm_resource_instance" "es_instance_2" {
  name              = "terraform-integration-2"
  resource_group_id = data.ibm_resource_group.group.id
}

resource "ibm_event_streams_topic" "es_topic_2" {
  resource_instance_id = data.ibm_resource_instance.es_instance_2.id
  name                 = "my-es-topic"
  partitions           = 1
  config = {
    "cleanup.policy"  = "compact,delete"
    "retention.ms"    = "86400000"
    "retention.bytes" = "1073741824"
    "segment.bytes"   = "536870912"
  }
}

```

### Sample3 Terraform code
{: #event-stream-sample3}

Create a kafka consumer application connecting to an existing event streams instance and its topics.
{: shortdesc}

```
data "ibm_resource_instance" "es_instance_3" {
  name              = "terraform-integration-3"
  resource_group_id = data.ibm_resource_group.group.id
}

data "ibm_event_streams_topic" "es_topic_3" {
  resource_instance_id = data.ibm_resource_instance.es_instance_3.id
  name                 = "my-es-topic"
}

resource "kafka_consumer_app" "es_kafka_app" {
  bootstrap_server = lookup(data.ibm_resource_instance.es_instance_3.extensions, "kafka_brokers_sasl", [])
  topics           = [data.ibm_event_streams_topic.es_topic_3.name]
  apikey           = var.es_reader_api_key
}

```


### Input parameters
{: #cert-manager-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`certificate_manager_instance_id`|String|Required|The CRN-based service instance ID.|
|`name`|String|Required|The display name for the imported certificate.|
|`data`|Map|Required|The certificate data.|
|`data.content`|String|Required|The content of certificate data, escaped.|
|`data.priv_key`|String|Optional|The private key data, escaped.|
|`data.intermediate`|String|Optional|The intermediate certificate data, escaped.|
|`description`|String|Optional|The description of the certificate.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #cert-manager-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the certificate.|
|`issuer`|String|The issuer of the certificate.|
|`begins_on`|String|The creation date of the certificate in Unix epoch time.|
|`expires_on`|String|The expiration date of the certificate in Unix epoch time.|
|`imported`|Boolean|Indicates whether a certificate was imported or not.|
|`status`|String|The status of certificate. Possible values are `active`, `inactive`, `expired`, `revoked`, `valid`, `pending`, and `failed`.|
|`has_previous`|Boolean|Indicates whether a certificate has a previous version.|
|`key_algorithm`|String|The key algorithm. Valid values are `rsaEncryption 2048 bit` or `rsaEncryption 4096 bit`. Default value: `rsaEncryption 2048 bit`.|
|`algorithm`|String|The encryption algorithm. Valid values are `sha256WithRSAEncryption`. |
{: caption="Table 1. Available output parameters" caption-side="top"}

## `ibm_certificate_manager_order`
{: #certmanager-order}

Order, renew, update, or delete a certificate in Certificate Manager. For more information, see [Ordering certificates](/docs/services/certificate-manager?topic=certificate-manager-ordering-certificates).
{: shortdesc}

### Sample Terraform code
{: #certmanager-order-sample}

```
resource "ibm_certificate_manager_order" "cert" {
  certificate_manager_instance_id = ibm_resource_instance.cm.id
  name                            = "test"
  description                     = "test description"
  domains                         = ["example.com"]
  rotate_keys                     = false
  domain_validation_method        = "dns-01"
  dns_provider_instance_crn       = ibm_cis.instance.id
}
```

### Input parameters
{: #certmanager-order-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description| Forces new resource |
|----|-----------|-----------|---------------------| ------ |
|`certificate_manager_instance_id`|String|Required|The CRN of your Certificate Manager instance.| Yes |
|`name`|String|Required|The name for the certificate that you want to order.| No |
|`description`|String|Optional|The description that you want to add to the certificate that you order.| No |
|`domains`|List of strings|Required|An list of valid domains for the issued certificate. The first domain is the primary domain. Additional domains are secondary domains.| Yes |
|`rotate_keys`|Boolean|Optional|Default value: False| No |
|`domain_validation_method`|String|Optional|The domain validation method that you want to use for your domain. The validation method is applied to analyze DNS parameters for your domain and determine the domain health and quality standards that your domain meets. Supported parameters are `dns-01`. | No |
|`key_algorithm`|String|Optional|The encryption algorithm key that you want to use for your certificate. Supported values are `rsaEncryption 2048 bit`, and `rsaEncryption 4096 bit`. If you do not provide an algorithm, `rsaEncryption 2048 bit` is used by default.| No |
|`dns_provider_instance_crn`|String|Optional|The CRN-based instance ID of the IBM Cloud Internet Services instance that manages the domains. If not present, Certificate Manager assumes that a `v4` or above Callback URL notifications channel with domain validation exists.| No |
|`auto_renew_enabled`|Boolean|Optional|Determines the certificate is auto renewed. Default is **false**. <br> With `auto_renew_enabled` as true, certificates are automatically renewed for 31 days. If the certificate expires before 31 days. You can renew by updating `rotate_keys` to renewed the certificates automatically.{: note} | No |

### Output parameters
{: #certmanager-order-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the certificate.|
|`issuer`|String|The issuer of the certificate.|
|`begins_on`|String|The creation date of the certificate in Unix epoch time.|
|`expires_on`|String|The expiration date of the certificate in Unix epoch time.|
|`imported`|Boolean|Indicates whether a certificate was imported or not.|
|`status`|String|The status of certificate. Possible values are `active`, `inactive`, `expired`, `revoked`, `valid`, `pending`, and `failed`.|
|`has_previous`|Boolean|Indicates whether a certificate has a previous version.|
|`key_algorithm`|String|The key algorithm. Valid values are `rsaEncryption 2048 bit` or `rsaEncryption 4096 bit`. Default value: `rsaEncryption 2048 bit`.|
|`algorithm`|String|The encryption algorithm. Valid values are `sha256WithRSAEncryption`. |
{: caption="Table 1. Available output parameters" caption-side="top"}

### Import
{: #certmanager-import}

This resource can be imported by using Crn ID of the certificate. The ID is available in the UI as `Certificate CRN` in the certificate details section.
{: shortdesc}

**Syntax**: 
```
terraform import ibm_certificate_manager_order.cert <id>

```
**Example**:
```
terraform import ibm_certificate_manager_order.cert crn:v1:bluemix:public:cloudcerts:us-south:a/4448261269a14562b839e0a3019ed980:8e80c112-5e48-43f8-8ab9-e198520f62e4:certificate:f543e1907a0020cfe0e883936916b336
```

### Timeouts
{: #certmanager-order-timeout}

The following timeouts are defined for this resource. 
{: shortdesc}

- **Create**: The ordering of the certificate is considered failed if no response is received for 10 minutes.
- **Update**: The renewal or update of the certificate is considered failed if no response is received for 10 minutes.
