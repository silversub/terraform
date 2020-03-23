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

# Key Protect resources
{: #kp-resources}

Create, modify, or delete [{{site.data.keyword.cloud_notm}} Key Protect](/docs/services/key-protect?topic=key-protect-about) resources. 
{: shortdesc}

## `ibm_kp_key`
{: #kp-key}

Create, or delete a Key Protect standard or root key.  
{: shortdesc}

To use the `ibm_kp_key` resource, the region parameter in the `provider.tf` file must be set to the same region that your Key Protect service instance is in. If no region parameter is specified, `us-south` is used by default. If the region in the `provider.tf` file is different from the Key Protect instance, the instance cannot be retrieved by Terraform and the Terraform action fails. 
{: note}

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

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`key_protect_id`|String|Required|The Key Protect service instance ID.|
|`key_name`|String|Required|The name of the key.|
|`standard_key`|Boolean|Optional|Set to **true** to create a standard key, to create a root key set this flag to **false**. Default is **false**.|
|`payload`|String|Optional| The base64 encoded key that you want to store and manage in the service. To import an existing key, provide a 256-bit key. To generate a new key, omit this parameter.|
|`encrypted_nonce`|String|Optional|The encrypted nonce value that verifies your request to import a key to Key Protect. This value must be encrypted by using the key that you want to import to the service. To retrieve a nonce, use the `ibmcloud kp import-token get` command. Then, encrypt the value by running `ibmcloud kp import-token encrypt-nonce`. Only for imported root key.|
|`iv_value`|String|Optional| Used with import tokens. The initialization vector (IV) that is generated when you encrypt a nonce. The IV value is required to decrypt the encrypted nonce value that you provide when you make a key import request to the service. To generate an IV, encrypt the nonce by running `ibmcloud kp import-token encrypt-nonce`. Only for imported root key.|

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

`ibm_kp_key` can be imported using the ID and CRN.

```
terraform import ibm_kp_key.crn crn:v1:bluemix:public:kms:us-south:a/faf6addbf6bf4768hhhhe342a5bdd702:05f5bf91-ec66-462f-80eb-8yyui138a315:key:52448f62-9272-4d29-a515-15019e3e5asd
```
{: pre}

