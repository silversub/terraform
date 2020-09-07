---

copyright:
  years: 2017, 2020
lastupdated: "2020-09-07"

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


# Key Management Service resources
{: #kms-resources}

Create, modify, or delete [{{site.data.keyword.cloud_notm}} Key Protect](/docs/key-protect?topic=key-protect-about) resources. This resource can be used for management of keys in both Key Protect and Hyper Protect Crypto Service (HPCS).
{: shortdesc}

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## `ibm_kms_key`
{: #kms-key}

Provide a key management service resource for Hyper Protect Crypto Services and Key Protect services. This allows standard and root keys to be created and deleted. The region parameter in the `provider.tf` file must be set. If region parameter is not specified, `us-south` is used as default. If the region in the `provider.tf` file is different from the Key Protect instance, the instance cannot be retrieved by Terraform and the Terraform action fails. 
{: shortdesc}

After creating an  Hyper Protect Crypto Service instance you need to initialize the instance properly with the crypto units, in order to create, or manage Hyper Protect Crypto Service keys. For more information about how to initialize the Hyper Protect Crypto Service instance, see [Initialize Hyper Protect Crypto](/docs/hs-crypto?topic=hs-crypto-initialize-hsm). 
{: note}


### Sample Terraform code
{: #kms-key-sample}

```
resource "ibm_resource_instance" "kms_instance" {
  name     = "instance-name"
  service  = "kms"
  plan     = "tiered-pricing"
  location = "us-south"
}
resource "ibm_kms_key" "test" {
  instance_id  = ibm_resource_instance.kms_instance.guid
  key_name     = "key-name"
  standard_key = false
  force_delete =true
}
resource "ibm_cos_bucket" "flex-us-south" {
  bucket_name          = "atest-bucket"
  resource_instance_id = "cos-instance-id"
  region_location      = "us-south"
  storage_class        = "flex"
  key_protect          = ibm_kms_key.test.id
}
```

### Input parameters
{: #kms-key-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description| Forces new resource |
|----|-----------|-----------|---------------------| ------- |
|`instance_id`|String|Required|The HPCS or key-protect instance GUID.| Yes |
|`key_name`|String|Required|The name of the key.| Yes |
|`standard_key`|Boolean|Optional|Set flag `true` for standard key, and `false` for root key. Default value is **false**.| Yes |
|`endpoint_type`|String|Optional|The type of the public or private endpoint to be used for creating keys. | Yes |
|`payload`|String|Optional| The base64 encoded key that you want to store and manage in the service. To import an existing key, provide a 256-bit key. To generate a new key, omit this parameter.| Yes |
|`encrypted_nonce`|String|Optional|The encrypted nonce value that verifies your request to import a key to Key Protect. This value must be encrypted by using the key that you want to import to the service. To retrieve a nonce, use the `ibmcloud kp import-token get` command. Then, encrypt the value by running `ibmcloud kp import-token encrypt-nonce`. Only for imported root key.| Yes |
|`force_delete`|Boolean|Optional|If set to **true**, Key Protect forces the deletion of a root or standard key, even if this key is still in use, such as to protect an {{site.data.keyword.cos_full_notm}} bucket. Note that the key cannot be deleted if the protected cloud resource is set up with a retention policy. Successful deletion includes the removal of any registrations that are associated with the key. Default value: **false**.| No |
|`iv_value`|String|Optional| Used with import tokens. The initialization vector (IV) that is generated when you encrypt a nonce. The IV value is required to decrypt the encrypted nonce value that you provide when you make a key import request to the service. To generate an IV, encrypt the nonce by running `ibmcloud kp import-token encrypt-nonce`. Only for imported root key.|  Yes |

You need to set `terraform destroy` force_delete flag after the provisioning keys is initiated. Later, a `terraform apply` is used prior to terraform destroy for force_delete flag to take effect.
{: note}

### Output parameters
{: #kms-key-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The CRN of the key.|
|`crn`|String|The CRN of the key.|
|`status`|String|The status of the key.|
|`key_id`|String|The ID of the key.|
|`type`|String|The type of the key KMS or HPCS.|

### Import
{: #kms-key-import}

`ibm_kms_key` can be imported by using the `id` and `crn`.

```
terraform import ibm_kms_key.crn crn:v1:bluemix:public:kms:us-south:a/faf6addbf6bf4768hhhhe342a5bdd702:05f5bf91-ec66-462f-80eb-8yyui138a315:key:52448f62-9272-4d29-a515-15019e3e5asd
```
{: pre}

## `ibm_kp_key`
{: #kp-key}

Create, or delete a Key Protect standard or root key.  
{: shortdesc}

To use the `ibm_kp_key` resource, the region parameter in the `provider.tf` file must be set to the same region that your Key Protect service instance. If region parameter is not specified, `us-south` is used as default. If the region in the `provider.tf` file is different from the Key Protect instance, the instance cannot be retrieved by Terraform and the Terraform action fails. 
{: note}

`ibm_kp_key` resource will be deprecated shortly, as a replacement, you can use `ibm_kms_key` resource.
{: important}

### Sample Terraform code
{: #kp-key-sample}

```
resource "ibm_resource_instance" "kp_instance" {
  name     = "instance-name"
  service  = "kms"
  plan     = "tiered-pricing"
  location = "us-south"
}
resource "ibm_kp_key" "test" {
  key_protect_id  = ibm_resource_instance.kp_instance.guid
  key_name     = "key-name"
  standard_key = false
}
resource "ibm_cos_bucket" "flex-us-south" {
  bucket_name          = "atest-bucket"
  resource_instance_id = "cos-instance-id"
  region_location      = "us-south"
  storage_class        = "flex"
  key_protect          = ibm_kp_key.test.id
}
```

### Input parameters
{: #kp-key-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description| Forces new resource |
|----|-----------|-----------|---------------------| ------- |
|`key_protect_id`|String|Required|The Key Protect service instance ID.| Yes |
|`key_name`|String|Required|The name of the key.| Yes |
|`standard_key`|Boolean|Optional|Set flag `true` for standard key, and `false` for root key. Default value is **false**.| Yes |
|`payload`|String|Optional| The base64 encoded key that you want to store and manage in the service. To import an existing key, provide a 256-bit key. To generate a new key, omit this parameter.| Yes |
|`encrypted_nonce`|String|Optional|The encrypted nonce value that verifies your request to import a key to Key Protect. This value must be encrypted by using the key that you want to import to the service. To retrieve a nonce, use the `ibmcloud kp import-token get` command. Then, encrypt the value by running `ibmcloud kp import-token encrypt-nonce`. Only for imported root key.| Yes |
|`force_delete`|Boolean|Optional|If set to **true**, Key Protect forces the deletion of a root or standard key, even if this key is still in use, such as to protect an {{site.data.keyword.cos_full_notm}} bucket. Note, the key cannot be deleted if the protected cloud resource is set up with a retention policy. Successful deletion includes the removal of any registrations that are associated with the key. Default value is **false**.| No |
|`iv_value`|String|Optional| Used with import tokens. The Initialization Vector (IV) that is generated when you encrypt a nonce. The IV value is required to decrypt the encrypted nonce value that you provide when you make a key import request to the service. To generate an IV, encrypt the nonce by running `ibmcloud kp import-token encrypt-nonce`. Only for imported root key.|  Yes |

### Output parameters
{: #kp-key-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The CRN of the key.|
|`crn`|String|The CRN of the key.|
|`status`|String|The status of the key.|
|`key_id`|String|The ID of the key.|

### Import
{: #kp-key-import}

`ibm_kp_key` can be imported using the `id` and `crn`.

```
terraform import ibm_kp_key.crn crn:v1:bluemix:public:kms:us-south:a/faf6addbf6bf4768hhhhe342a5bdd702:05f5bf91-ec66-462f-80eb-8yyui138a315:key:52448f62-9272-4d29-a515-15019e3e5asd
```
{: pre}

