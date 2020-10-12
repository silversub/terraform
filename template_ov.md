---

copyright:
  years: 2017, 2020
lastupdated: "2020-10-12"

keywords: terraform templates, schematics template

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



# Sample Terraform templates and deploy to {{site.data.keyword.bplong_notm}}
{: #sample_terraformtemplates}

All Terraform templates use the Terraform version 0.12 format.
{: note}

The sample Terraform templates and deploy to {{site.data.keyword.bplong_notm}} link is an efficient way to access the templates and experience the auto deploy of the {{site.data.keyword.bplong_notm}} to create workspace.
{: shortdesc}

When you click the `Deploy to IBM Cloud Schematics` following actions occur:

  1. If the user does not have an active {{site.data.keyword.cloud_notm}} account, you must create a trial account or a real account.

  2. The user can select a region, resource group (available in the Dallas, Washington, London, Frankfurt, and Tokyo regions) or organization and space (available in the Dallas, London, and Frankfurt regions).

  3. The auto deploy link creates a workspace in the {{site.data.keyword.bplong_notm}}.

  4. To execute `generate plan` successfully, you need to configure the required variables.



## API Gateway templates
{: #api-gwy-template}

<table>
   <thead>
    <th style="width:60px">Name</th>
    <th style="width:250px">Description and resources</th>
    <th style="width:180px">Access</th>
  </thead>
  <tbody>
    <tr>
      <td><code>ibm-api-gateway</code></td>
      <td>Create an [IBM Cloud API Gateway](/docs/api-gateway?topic=api-gateway-whatis_apigw) service instance to set up an API for an IBM Cloud service of your choice. You can specify the API endpoint that you want to use to access your service, and define subscription keys so that developers can securely consume your API.<br><br>**Resources**<br><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_resource_instance</code></li><li style="margin:0px; padding:0px"><code>ibm_api_gateway_endpoint</code></li><li style="margin:0px; padding:0px"><code>ibm_api_gateway</code></li><li style="margin:0px; padding:0px"><code>ibm_api_gateway_endpoint_subscription</code></li></ul></td>
      <td>[View GitHub repo](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-api-gateway)<br> <br>[Deploy to {{site.data.keyword.bplong_notm}}](https://cloud.ibm.com/schematics/workspaces/create?repository=https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-api-gateway&terraform_version=terraform_v0.12)</td>
 </tr>
  </tbody>
  </table>
 
## Certificate Manager templates
{: #cert-mgr-template}

  <table>
   <thead>
    <th style="width:60px">Name</th>
    <th style="width:250px">Description and resources</th>
    <th style="width:180px">Access</th>
  </thead>
  <tbody>
 <tr>
   <td><code>ibm-certificate-manager-import</code></td>
      <td> Generate a TLS certificate and import this certificate into [IBM Cloud Certificate Manager](/docs/certificate-manager?topic=certificate-manager-about-certificate-manager).<br><br>**Resources**<br><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_resource_instance</code></li><li style="margin:0px; padding:0px"><code>ibm_certificate_manager_import</code></li><li style="margin:0px; padding:0px"><code>null_resource</code></li></ul></td>
      <td>[View GitHub repo](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-certificate-manager/ibm-certificate-manager-import) <br> <br>[Deploy to {{site.data.keyword.bplong_notm}}](https://cloud.ibm.com/schematics/workspaces/create?repository=https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-certificate-manager/ibm-certificate-manager-import&terraform_version=terraform_v0.12)</td>
  </tr>
 <tr>
   <td><code>ibm-certificate-manager-order</code></td>
      <td> Create an IBM Cloud Internet Services instance with a domain, and use [IBM Cloud Certificate Manager](/docs/certificate-manager?topic=certificate-manager-about-certificate-manager) to generate a TLS certificate for this domain.<br><br>**Resources**<br><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_resource_instance</code></li><li style="margin:0px; padding:0px"><code>ibm_cis</code></li><li style="margin:0px; padding:0px"><code>ibm_cis_domain</code></li><li style="margin:0px; padding:0px"><code>ibm_certificate_manager_order</code></li></ul></td>
     <td>[View GitHub repo](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-certificate-manager/ibm-certificate-manager-order) <br> <br>[Deploy to {{site.data.keyword.bplong_notm}}](https://cloud.ibm.com/schematics/workspaces/create?repository=https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-certificate-manager/ibm-certificate-manager-order&terraform_version=terraform_v0.12)</td>
	  </tr>
	</tbody>
	</table>


## Direct Link templates
{: #dl-template}

   <table>
   <thead>
    <th style="width:60px">Name</th>
    <th style="width:250px">Description and resources</th>
    <th style="width:180px">Access</th>
  </thead>
  <tbody>
  <tr>
    <td><code>ibm-dl-gateway</code></td>
      <td>Create a speed and reliable direct link gateways, virtual connections, offering information, routers, and ports by using the resources.<br><br>**Resources**<br><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_dl_gateway</code></li><li style="margin:0px; padding:0px"><code>ibm_dl_virtual_connection</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpc</code></li></ul></td>
      <td>[View GitHub repo](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-direct-link)<br> <br>[Deploy to {{site.data.keyword.bplong_notm}}](https://cloud.ibm.com/schematics/workspaces/create?repository=https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-direct-link&terraform_version=terraform_v0.12)</td>
  </tr>
  </tbody>
  </table>


## Identity & Access (IAM) templates
{: #iam-template}

  <table>
   <thead>
    <th style="width:60px">Name</th>
    <th style="width:250px">Description and resources</th>
    <th style="width:180px">Access</th>
  </thead>
  <tbody>
  <tr>
    <td><code>ibm-iam-custom-role</code></td>
      <td>Create a custom role in IBM Cloud Identity and Access Management (IAM) for IBM Cloud Key Protect.<br><br>**Resources**<br><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_iam_custom_role</code></li></ul></td>
	  <td>[View GitHub repo](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-iam-custom-role)<br> <br>[Deploy to {{site.data.keyword.bplong_notm}}](https://cloud.ibm.com/schematics/workspaces/create?repository=https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-iam-custom-role&terraform_version=terraform_v0.12)</td>
  </tr>
  <tr>
    <td><code>ibm-iam-policy</code></td>
      <td>Create an access policy in IBM Cloud Identity and Access Management (IAM) to grant permissions for a resource group to a user.<br><br>**Resources**<br><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_iam_user_policy</code></li></ul></td>
	  <td>[View GitHub repo](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-iam-policy) <br> <br>[Deploy to {{site.data.keyword.bplong_notm}}](https://cloud.ibm.com/schematics/workspaces/create?repository=https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-iam-policy&terraform_version=terraform_v0.12)</td>
  </tr>
  </tbody>
  </table>

## Key Management Service templates
{: #kms-template}

   <table>
   <thead>
    <th style="width:60px">Name</th>
    <th style="width:250px">Description and resources</th>
    <th style="width:180px">Access</th>
  </thead>
  <tbody>
  <tr>
    <td><code>ibm-key-management-service</code></td>
      <td>Create an {{site.data.keyword.cos_full_notm}} service instance with a bucket to store your data and provide a key management service resource for Hyper Protect Crypto Services and Key Protect service instance with a root key. This allow access between these services with an IBM Cloud Identity and Access Management policy.<br><br>**Resources**<br><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_kms_key</code></li><li style="margin:0px; padding:0px"><code>ibm_kp_key</code></li></ul></td>
      <td>[View GitHub repo](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-kms)<br> <br>[Deploy to {{site.data.keyword.bplong_notm}}](https://cloud.ibm.com/schematics/workspaces/create?repository=https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-kms&terraform_version=terraform_v0.12)</td>
  </tr>
  </tbody>
  </table>


