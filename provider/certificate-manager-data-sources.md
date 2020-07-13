---

copyright:
  years: 2017, 2020
lastupdated: "2020-07-08"

keywords: terraform provider plugin, terraform api gateway

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


# Certificate Manager data sources
{: #cert-manager-data-sources}

Review the data sources that you can use to retrieve information about the certificates that your manage in Certificate Manager. All data sources are imported as read-only information. You can reference the output parameters for each data source by using Terraform interpolation syntax.

Before you start working with your data source, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}



## `ibm_certificate_manager_certificates`
{: #cert-manager-certificates}

Retrieve the details of one or all certificates that are managed by your Certificate Manager service instance. 
{: shortdesc}

### Sample Terraform code
{: #cert-manager-certificates-sample}

```
data "ibm_resource_instance" "cm" {
    name     = "testname"
    location = "us-south"
    service  = "cloudcerts"
}
data "ibm_certificate_manager_certificates" "certs"{
    certificate_manager_instance_id=data.ibm_resource_instance.cm.id
}
```
{: codeblock}

### Input parameters
{: #cert-manager-certificates-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
|`certificate_manager_instance_id`|String|Required|The CRN of the Certificate Manager service instance. |

### Output parameters
{: #cert-manager-certificates-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
|`id`|String|The ID of the certificate that is managed in Certificate Manager. The ID is composed of `<certificate_manager_instance_ID>:<certificate_ID>`. |
|`name`|String|The name of the certificate. | 
|`domains`|Array|A list of domains that the certificate is associated with. The first domain is referred to as the primary domain. Any additional domains are referred to as secondary domains.|
|`issuer`|String|The issuer of the certificate.|
|`begins_on`|Timestamp|The timestamp when the certificate was created in Unix epoch time format.| 
|`expires_on`|Date|The date when the certificate expires in Unix epoch time format.|
|`imported`|Boolean|If set to **true**, the certificate is imported. |
|`status`|String|The status of the certificate.|
|`has_previous`|Boolean|If set to **true**, the certificate has a previous version.| 
|`key_algorithm`|String|The key algorithm of the certificate. |
|`algorithm`|String|The algorithm that is used for the certificate.| 
|`serial_number`|String|The serial number of the certificate.|
|`issuance_info`|List of objects|The issuance information of the certificate.| 
|`issuance_info.status`|String|The status of the certificate.|
|`issuance_info.ordered_on`|Date|The date when the certificate was ordered.|
|`issuance_info.code`|String|The code of the certificate.|
|`issuance_info.additional_info`|String|Any additional information for the certificate.| 
