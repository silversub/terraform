---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-31"

keywords: terraform provider plugin, terraform certificate manager, terraform cert manager, terraform certificate

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

# Certificate Manager resources
{: #cert-manager-resources}

Review the [Certificate Manager](/docs/services/certificate-manager?topic=certificate-manager-about-certificate-manager) resources that you can create, modify, or delete. You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 
{: shortdesc}

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 


## `ibm_certificate_manager_import`
{: #cert-manager}

Upload or delete a certificate in Certificate Manager.
{: shortdesc}

### Sample Terraform code
{: #cert-manager-sample}

```
provider "ibm"
{
}
resource "ibm_certificate_manager_import" "cert" {
  certificate_manager_instance_id = ibm_resource_instance.cm.id
  name                            = "test"
  description="string"
  data = {
    content = file(var.certfile_path)
    priv_key = ""
    intermediate = ""
  }
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

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`certificate_manager_instance_id`|String|Required|The CRN of your Certificate Manager instance.|
|`name`|String|Required|The name for the certificate that you want to order.|
|`description`|String|Optional|The description that you want to add to the certificate that you order.|
|`domains`|List of strings|Required|An list of valid domains for the issued certificate. The first domain is the primary domain. Additional domains are secondary domains.|
|`rotate_keys`|Boolean|Optional|Default value: False|
|`domain_validation_method`|String|Optional|The domain validation method that you want to use for your domain. The validation method is applied to analyze DNS parameters for your domain and determine the domain health and quality standards that your domain meets. Supported parameters are `dns-01`. |
|`dns_provider_instance_crn`|String|Optional|The CRN-based instance ID of the IBM Cloud Internet Services instance that manages the domains. If not present, Certificate Manager assumes that a `v4` or above Callback URL notifications channel with domain validation exists.|

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

### Timeouts
{: #certmanager-order-timeout}

The following timeouts are defined for this resource. 
{: shortdesc}

- **Create**: The ordering of the certificate is considered failed if no response is received for 10 minutes.
- **Update**: The renewal or update of the certificate is considered failed if no response is received for 10 minutes.
