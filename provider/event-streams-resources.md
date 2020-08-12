---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-12"

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


## ibm_event_streams_topic
{: #event-streams}

Create and update the event streams.
{: shortdesc}

### Example 1: Create an event streams service instance and topic
{: #event-stream-sample}

Create an event streams service instance and topic.
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

### Example 2: Create a topic on an existing event streams instance
{: #event-stream-sample2}

Create topic on an existing event streams instance.
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

### Example 3: Create a Kafka consumer application connecting to an event streams instance and its topics
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
{: #event-streams-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required / optional|Description|
|----|-----------|-----------|---------------------|
|`resource_instance_id`|String|Required|The ID/CRN of the event streams service instance.|
|`name`|String|Required|The name of the topic.|
|`partitions`|Integer|Optional|The number of partitions of the topic. Default value is 1.|
|`config`|Map|Optional|The configuration parameters of the topic. Supported configurations are: `cleanup.policy`, `retention.ms`, `retention.bytes`, `segment.bytes`, `segment.ms`, `segment.index.bytes`.|
{: caption="Table. Available input parameters" caption-side="top"}

### Output parameters
{: #event-streams-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the topic in CRN format. For example, `crn:v1:bluemix:public:messagehub:us-south:a/6db1b0d0b5c54ee5c201552547febcd8:cb5a0252-8b8d-4390-b017-80b743d32839:topic:my-es-topic`|
|`kafka_http_url`|String|The API endpoint for interacting with event streams REST API.|
|`kafka_brokers_sasl`|Array of Strings|Kafka brokers uses for interacting with Kafka native API.|
{: caption="Table 1. Available output parameters" caption-side="top"}


### Import
{: #event-streams-import}

This resource can be imported by using `ID`. The ID is the 'CRN', the `resource type` is `topic`, the `resource` is the name of the topic.
{: shortdesc}

**Syntax**: 
```
terraform import ibm_event_streams_topic.es_topic <crn>

```
**Example**:
```
terraform import ibm_event_streams_topic.es_topic crn:v1:bluemix:public:messagehub:us-south:a/6db1b0d0b5c54ee5c201552547febcd8:cb5a0252-8b8d-4390-b017-80b743d32839:topic:my-es-topic
```
