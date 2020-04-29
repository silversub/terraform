---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-29"

keywords: terraform provider plugin, terraform api gateway

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



certificate_manager_instance_id - (Required,string) The CRN-based service instance ID.

### Output parameters
{: #cert-manager-certificates-output}


id - The Id of the Certificate. It is a combination of <certificate_manager_instance_id>:<CertificateID>
name - The display name for the certificate.
domains - An array of valid domains for the issued certificate. The first domain is the primary domain. Additional domains are secondary domains.
issuer - The issuer of the certificate.
begins_on - The creation date of the certificate in Unix epoch time.
expires_on - The expiration date of the certificate in Unix epoch time.
imported - Indicates whether a certificate has imported or not.
status - The status of certificate.
has_previous - Indicates whether a certificate has a previous version.
key_algorithm - Key Algorithm of a certificate.
algorithm - Algorithm of a certificate.
serial_number - The certificate serial number
issuance_info - Issuance Info of Certificate.
status - The status of certificate.
ordered_on - The date the certificate was ordered.
code - Code of Certificate.
additional_info - The Additional Info of certificate.
