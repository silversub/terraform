---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-23"

keywords: terraform provider plugin, terraform key protect, terraform kp, terraform root key 

subcollection: terraform

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}

# Key Protect data sources
{: #kp-data-sources}

You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 


## `ibm_kp_key`
{: #kp-key}

Retrieve information about an existing Key Protect standard or root key. 
{: shortdesc}

To use the `ibm_kp_key` data resource, the region parameter in the `provider.tf` file must be set to the same region that your Key Protect service instance is in. If no region parameter is specified, `us-south` is used by default. If the region in the `provider.tf` file is different from the Key Protect instance, the instance cannot be retrieved by Terraform and the Terraform action fails. 
{: note}

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
| `keys.standard_key` |Boolean|If set to **true**, the key is a standard key. If set to **false**, the key is a root key. |
