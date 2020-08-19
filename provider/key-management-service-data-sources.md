---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-19"

keywords: terraform provider plugin, terraform key management service, terraform key management, terraform kms, kms, terraform key protect, terraform kp, terraform root key, hyper protect crypto service, HPCS

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


# Key Management Service data sources
{: #kms-data-sources}

You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 

Before you start working with your data source, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## `ibm_kms_key`
{: #kms-key-ds}

Retrieves the list of keys from the Hyper Protect Crypto Services (HPCS) and Key Protect services for the given key name. The region parameter in the `provider.tf` file must be set. If region parameter is not specified, `us-south` is used by default. If the region in the `provider.tf` file is different from the Key Protect instance, the instance cannot be retrieved by Terraform and the Terraform action fails. 
{: shortdesc}

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


### Sample Terraform code
{: #kms-key-ds-sample}

```
data "ibm_kms_key" "test" {
  instance_id = "guid-of-keyprotect-or hs-crypto-instance"
  key_name = "name-of-key"
}
resource "ibm_cos_bucket" "flex-us-south" {
  bucket_name          = "atest-bucket"
  resource_instance_id = "cos-instance-id"
  region_location      = "us-south"
  storage_class        = "flex"
  key_protect          = data.ibm_kms_key.test.key.0.crn
}
```

### Input parameters
{: #kms-key-ds-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`instance_id`|String|Required|The key-protect instance GUID.|
|`key_name`|String|Required|The name of the key. Only matching name of the keys are etrieved |
|`endpoint_type`|String|Optional|The type of the public or private endpoint to be used for fetching keys. |

### Output parameters
{: #kms-key-ds-output}

Review the output parameters that are exported.
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`keys`|String|Lists the Keys of HPCS or Key-protect instance. |
|`keys.name`|String|The name for the key. |
|`keys.id`|String|The unique ID for the key. |
|`keys.crn`|String|The CRN of the key. |
|`keys.standard_key`|String|Set the flag `true` in case of standard key, and `false` for root key. Default value is **false**.|

## `ibm_kp_key`
{: #kp-key}

Retrieve information about an existing Key Protect standard or root key. 
{: shortdesc}

To use the `ibm_kp_key` data source, the region parameter in the `provider.tf` file must be set to the same region that your Key Protect service instance. If region parameter is not specified, `us-south` is used by default. If the region in the `provider.tf` file is different from the Key Protect instance, the instance cannot be retrieved by Terraform and the Terraform action fails. 
{: note}

`ibm_kp_key` resource will be deprecated shortly, as a replacement, you can use `ibm_kms_key` data source.
{: important}

### Sample Terraform code
{: #kp-key-sample}

The following example creates a read-only copy of the `mydatabase` instance in `us-east`.  
{: shortdesc}

```
data "ibm_kp_key" "test" {
  key_protect_id = "my-kp-id"
}
```

### Input parameters
{: #kp-key-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required/optional|Description|
|----|-----------|------|--------|
| `key_protect_id`|String|Required|The ID of the Key Protect service instance.|
| `key_name`| String|Optional| The name of the key. Only the keys with matching name will be retrieved.|
{: caption="Table. Available input parameters" caption-side="top"}


### Output parameters
{: #kp-key-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
| `keys` | List of objects | A list of all keys in your Key Protect service instance.|
| `keys.name`|String| The name of the key.|
| `keys.id`|String| The unique identifier of the key.|
| `keys.crn`|String| The CRN of the key.|
| `keys.standard_key` |Boolean|Set the flag `true` in case of standard key, and `false` for root key. Default value is **false**. |
