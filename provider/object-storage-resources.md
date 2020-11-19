---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-19"

keywords: terraform provider plugin, terraform data source cos, terraform data source object storage, terraform get cloud object storage bucket, terraform get object storage resources

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

# Object Storage resources
{: #object-storage-resources}

Review the [{{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage) resources that you can create, modify, or delete. You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 
{: shortdesc}

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}

## `ibm_cos_bucket`
{: #cos-bucket}

Create or delete an {{site.data.keyword.cos_full_notm}} bucket. The bucket is used to store your data. For more information, about configuration options, see [Create some buckets to store your data](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage#gs-create-buckets). 

To create a bucket, you must provision an {{site.data.keyword.cos_full_notm}} instance first by using the [`ibm_resource_instance`](/docs/terraform?topic=terraform-resource-mgmt-resources#resource-instance) resource.
{: note}

### Sample Terraform code
{: #cos-bucket-sample}

The following example creates an instance of {{site.data.keyword.cos_full_notm}}, {{site.data.keyword.cloudaccesstrailfull_notm}}, and {{site.data.keyword.mon_full_notm}}. Then, multiple buckets are created and configured to send audit events and metrics to your service instances.  
{: shortdesc}

```
data "ibm_resource_group" "cos_group" {
  name = "cos-resource-group"
}

resource "ibm_resource_instance" "cos_instance" {
  name              = "cos-instance"
  resource_group_id = data.ibm_resource_group.cos_group.id
  service           = "cloud-object-storage"
  plan              = "standard"
  location          = "global"
}

resource "ibm_resource_instance" "activity_tracker" {
  name              = "activity_tracker"
  resource_group_id = data.ibm_resource_group.cos_group.id
  service           = "logdnaat"
  plan              = "lite"
  location          = "us-south"
}
resource "ibm_resource_instance" "metrics_monitor" {
  name              = "metrics_monitor"
  resource_group_id = data.ibm_resource_group.cos_group.id
  service           = "sysdig-monitor"
  plan              = "lite"
  location          = "us-south"
}

resource "ibm_cos_bucket" "standard-ams03" {
  bucket_name          = "a-standard-bucket-at-ams"
  resource_instance_id = ibm_resource_instance.cos_instance.id
  single_site_location = "ams03"
  storage_class        = "standard"
}

resource "ibm_cos_bucket" "flex-us-south" {
  bucket_name          = "a-flex-bucket-at-us-south"
  resource_instance_id = ibm_resource_instance.cos_instance.id
  region_location      = "us-south"
  storage_class        = "flex"
}

resource "ibm_cos_bucket" "cold-ap" {
  bucket_name           = "a-cold-bucket-at-ap"
  resource_instance_id  = ibm_resource_instance.cos_instance.id
  cross_region_location = "ap"
  storage_class         = "cold"
}

resource "ibm_cos_bucket" "standard-ams03-firewall" {
  bucket_name          = "a-standard-bucket-at-ams-firewall"
  resource_instance_id = ibm_resource_instance.cos_instance.id
  cross_region_location      = "us"
  storage_class        = "standard"
 activity_tracking {
    read_data_events     = true
    write_data_events    = true
    activity_tracker_crn = ibm_resource_instance.activity_tracker.id
  }
  metrics_monitoring {
    usage_metrics_enabled  = true
    metrics_monitoring_crn = ibm_resource_instance.metrics_monitor.id
  }
  allowed_ip =  ["223.196.168.27","223.196.161.38","192.168.0.1"]
}

resource "ibm_cos_bucket" "flex-us-south-firewall" {
  bucket_name          = "a-flex-bucket-at-us-south"
  resource_instance_id = ibm_resource_instance.cos_instance.id
  cross_region_location      = "us"
  storage_class        = "flex"
 activity_tracking {
    read_data_events     = true
    write_data_events    = true
    activity_tracker_crn = ibm_resource_instance.activity_tracker.id
  }
  metrics_monitoring {
    usage_metrics_enabled  = true
    metrics_monitoring_crn = ibm_resource_instance.metrics_monitor.id
  }
  allowed_ip =  ["223.196.168.27","223.196.161.38","192.168.0.1"]
}

resource "ibm_cos_bucket" "cold-ap-firewall" {
  bucket_name          = "a-cold-bucket-at-ap"
  resource_instance_id = ibm_resource_instance.cos_instance.id
  cross_region_location      = "us"
  storage_class        = "cold"
 activity_tracking {
    read_data_events     = true
    write_data_events    = true
    activity_tracker_crn = ibm_resource_instance.activity_tracker.id
  }
  metrics_monitoring {
    usage_metrics_enabled  = true
    metrics_monitoring_crn = ibm_resource_instance.metrics_monitor.id
  }
  allowed_ip =  ["223.196.168.27","223.196.161.38","192.168.0.1"]
}

```

### Input parameters
{: #hpvs-cos-bucket-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required / optional | Description |
| ------------- |-------------| ----- | -------------- |
| `activity_tracking`| Object to enable auditing with {{site.data.keyword.cloudaccesstrailfull_notm}} | Optional | Configure your {{site.data.keyword.cloudaccesstrailfull_notm}} service instance and the type of events that you want to send to your service to audit activity against your bucket. For a list of supported actions, see [Bucket actions](/docs/cloud-object-storage?topic=cloud-object-storage-at-events#at-actions-mngt-2).|
|`activity_tracking.read_data_events`| Boolean| Required | If set to **true**, all read events against a bucket are sent to your {{site.data.keyword.cloudaccesstrailfull_notm}} service instance.|
|`activity_tracking.write_data_events`| Boolean| Required| If set to **true**, all write events against a bucket are sent to your {{site.data.keyword.cloudaccesstrailfull_notm}} service instance.|
|`activity_tracking.activity_tracker_crn`| String| Required | The CRN of your {{site.data.keyword.cloudaccesstrailfull_notm}} service instance that you want to send your events to. This value is required only when you configure your instance for the first time.|
| `allowed_ip` | Array of strings | Optional | A list of IPv4 or IPv6 addresses in CIDR notation that you want to allow access to your {{site.data.keyword.cos_full_notm}} bucket.|
| `bucket_name` | String | Required | The name of the bucket. |
| `cross_region_location` | String | Optional | Specify the cross-regional bucket location. Supported values are `us`, `eu`, and `ap`. If you use this parameter, do not set `single_site_location` or `region_location` at the same time. |
|`endpoint_type`| String | Optional | The type of the endpoint either public or private to be used for buckets. Default value is `public`.|
| `key_protect` | String | Optional | The CRN of the {{site.data.keyword.keymanagementservicelong_notm}} root key that you want to use to encrypt data that is sent and stored in {{site.data.keyword.cos_full_notm}}. Before you can enable {{site.data.keyword.keymanagementservicelong_notm}} encryption, you must provision an instance of {{site.data.keyword.keymanagementservicelong_notm}} and authorize the service to access {{site.data.keyword.cos_full_notm}}. For more information, see [Server-Side Encryption with IBM Key Protect or Hyper Protect Crypto Services (SSE-KP)](/docs/cloud-object-storage?topic=cloud-object-storage-encryption). |
|`metrics_monitoring`| Object to enable metrics tracking with {{site.data.keyword.mon_full_notm}} | Optional| Set up your {{site.data.keyword.mon_full_notm}} service instance to receive metrics for your {{site.data.keyword.cos_full_notm}} bucket.|
|`metrics_monitoring.usage_metrics_enabled`|Boolean|Optional|If set to **true**, all metrics are sent to your {{site.data.keyword.mon_full_notm}} service instance.|
|`metrics_monitoring.metrics_monitoring_crn`|String|Required| The CRN of the {{site.data.keyword.mon_full_notm}} service instance that you want to send metrics to. This value is required only when you configure your instance for the first time.|
| `resource_instance_id` | String | Required | The ID of the {{site.data.keyword.cos_full_notm}} service instance for which you want to create a bucket. |
| `region_location` | String | Optional | The location of a regional bucket. Supported values are `au-syd`, `eu-de`, `eu-gb`, `jp-tok`, `us-east`, `us-south`. If you set this parameter, do not set `single_site_location` or `cross_region_location` at the same time.|
| `single_site_location` | String | Optional | The location for a single site bucket. Supported values are: `ams03`, `che01`, `hkg02`, `mel01`, `mex01`, `mil01`, `mon01`, `osl01`, `par01`, `sjc04`, `sao01`, `seo01`, `sng01`, and `tor01`. If you set this parameter, do not set `region_location` or `cross_region_location` at the same time.|
| `storage_class` | String | Required | The storage class that you want to use for the bucket. Supported values are `standard`, `vault`, `cold`, `flex`, and `smart`. For more information, about storage classes, see [Use storage classes](/docs/cloud-object-storage?topic=cloud-object-storage-classes).|

You need to set `cross_region_location`, `region_location`, or `single_site_location` to specify that location where you want to create the bucket. 
{: note}

### Output parameters
{: #hpvs-cos-bucket-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `crn` | String | The CRN of the bucket. |
| `cross_region_location` | String | The location if you created a cross-regional bucket. |
| `id` | String | The ID of the bucket. | 
| `key_protect` | String | The CRN of the {{site.data.keyword.keymanagementservicelong_notm}} instance that you use to encrypt your data in {{site.data.keyword.cos_full_notm}}. |
| `region_location` | String | The location if you created a regional bucket. |
| `resource_instance_id` | String | The ID of {site.data.keyword.cos_full_notm}} instance. | 
| `single_site_location` | String | The location if you created a single site bucket. |
| `storage_class` | String | The storage class of the bucket. |

### Import

The `ibm_cos_bucket` resource can be imported by using the `id`. The ID is formed from the `CRN` (Cloud Resource Name), the `bucket type` which must be `ssl` for single_site_location, `rl` for region_location or `crl` for cross_region_location, and the bucket location. The `CRN` and bucket location can be found on the portal.

id = `$CRN:meta:$buckettype:$bucketlocation`

```
$ terraform import ibm_cos_bucket.mybucket <crn>

$ terraform import ibm_cos_bucket.mybucket crn:v1:bluemix:public:cloud-object-storage:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3:bucket:mybucketname:meta:crl:eu:public
```
