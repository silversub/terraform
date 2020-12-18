---

copyright:
  years: 2017, 2020
lastupdated: "2020-12-18"

keywords: terraform faqs, softlayer, iaas

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

# FAQs
{: #faqs}

## How do I find the flavor and parameters to configure a virtual service instance in {{site.data.keyword.Bluemix_notm}}? 
{: #vsi_config}
{: faq}

The Terraform on {{site.data.keyword.cloud_notm}} `ibm_compute_vm_instance` resource includes optional and mandatory configuration parameters. To find an overview of how you can configure your virtual server, use the {{site.data.keyword.Bluemix_notm}} CLI.  

1. Install the [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli). 

2. List supported configuration options for virtual servers in {{site.data.keyword.Bluemix_notm}}. The listed options include available data centers, machine flavors, cpu, memory, operating systems, local disk and SAN disk sizes, and network interface controllers (nic). {{site.data.keyword.Bluemix_notm}} offers multiple virtual server offerings that each come with a specific configuration. The configuration of an offering is optimized for a specific workload need, such as high performance, or real-time analytics. For more information, see [Public Virtual Servers](/docs/virtual-servers?topic=virtual-servers-about-public-virtual-servers). 
   ```
   ibmcloud sl vs options
   ```
   {: pre}


## How long does it take for my resources to provision and delete?
{: #provisioning_times}
{: faq}

Most {{site.data.keyword.Bluemix_notm}} platform resources provision within a few seconds. Infrastructure resources, including Bare Metal servers, virtual servers, and {{site.data.keyword.Bluemix_notm}} Load Balancers can take longer. When you run the `terraform apply` or `terraform destroy` command, the command might take a few minutes to complete and you are not able to enter a different command during that time. The `terraform apply` command returns when your resources are fully provisioned, whereas the `terraform destroy` command might return before your resources are deleted from your {{site.data.keyword.Bluemix_notm}} platform or infrastructure portfolio. 

Use the `terraform apply` and `terraform destroy` times in the following table as a reference for when you can expect your commands to complete. 

If the Terraform on {{site.data.keyword.cloud_notm}} operation does not complete due to a timeout, wait for the resource state change to complete and retry the operation. 
{: tip}

<table>
<caption>Overview of `terraform apply` and `terraform destroy` command completion times</caption>
<thead>
<th>Resource</th>
<th><code>terraform apply</code> return time</th>
<th><code>terraform destroy</code> return time</th>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} platform resources</td>
<td>A few seconds</td>
<td>A few seconds</td>
</tr>
<tr>
<td>Virtual servers</td>
<td>A few minutes</td>
<td>A few seconds</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Load Balancers</td>
<td>A few minutes</td>
<td>Up to 30 minutes</td>
</tr>
<tr>
<td>Bare Metal servers</td>
<td>Up to a few hours</td>
<td>Up to a few hours</td>
</tr>
</tbody>
</table>

## How do I setup IBM Terraform on {{site.data.keyword.cloud_notm}} provider for Terraform on {{site.data.keyword.cloud_notm}} version 0.13.0?
{: #ibm-terraform-provider-v13}
{: faq}

Complete the following steps can be used in IBM Terraform on {{site.data.keyword.cloud_notm}} provider to support Terraform on {{site.data.keyword.cloud_notm}} version 0.13.0:

1. From the HashiCorp site, [download version 0.13.x](https://releases.hashicorp.com/terraform/){: external}
2. Find the [{{site.data.keyword.cloud_notm}} Terraform on {{site.data.keyword.cloud_notm}} provider](https://github.com/IBM-Cloud/terraform-provider-ibm/releases) version.
2. Include the shared Terraform on {{site.data.keyword.cloud_notm}} block in the `providers.tf` file.

  **Syntax**

  ```
  terraform {
    required_providers {
      ibm = {
        source = "IBM-Cloud/ibm"
        version = "<provider version>"
       }
     }
   }
  ```

  **Example**

  ```
  terraform {
    required_providers {
      ibm = {
        source = "IBM-Cloud/ibm"
        version = "1.13.0"
       }
     }
   }
  ```
3. If you are using Terraform on {{site.data.keyword.cloud_notm}} modules, the shared Terraform on {{site.data.keyword.cloud_notm}} block to be used in all the module folders that is been used.
    Detailed steps in the IBM Terraform on {{site.data.keyword.cloud_notm}} documentation will be published shortly.
    {: note}

## Why I am getting an issue when trying to provision an `ibm_container_alb_cert`?
{: #provision-ibm-container-alb-cert}
{: faq}

If you are using the resource `ibm_container_alb_cert` to create a secret and fails with a following error

**Error**

```
stderr : 
Error: Error waiting for create resource alb cert (buvlsclf0qcur3hjcrng/ingress-tls-cert) : The resource alb cert buvlsclf0qcur3hjcrng/ingress-tls-cert does not exist anymore: Request failed with status code: 404, ServerErrorResponse: {"incidentID":"5f82fa1696ce299a-IAD","code":"E0024","description":"The specified Ingress secret name is not found for this cluster.","type":"ALBSecret","recoveryCLI":"To list the Ingress secrets for a cluster, run 'ibmcloud ks ingress secret ls -c \u003ccluster_name_or_ID\u003e'."}
```
{: pre}

**Solution**

You need to update the {{site.data.keyword.cloud_notm}} Terraform on {{site.data.keyword.cloud_notm}} provider to use `version 1.16.1` and above.

