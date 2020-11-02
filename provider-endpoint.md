---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-02"

keywords: terraform provider, terraform provider private endpoint, private endpoint

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

# Configure the provider to use the service endpoint
{: #config-provider}

The steps involved in configuring your Terraform runtime to use the private Cloud Service Endpoint (CSE) of an {{site.data.keyword.cloud_notm}} service within  public CSE in [Production environment](https://cloud.ibm.com).

You can configure the {{site.data.keyword.cloud_notm}} provider for Terraform to communicate with an {{site.data.keyword.cloud_notm}} service by using the service's private service endpoint.
{: shortdesc}

1. Setup the Terraform engine and an {{site.data.keyword.cloud_notm}} provider, in {{site.data.keyword.cloud_notm}} virtual machine by using private VLAN. And provision the enabled Virtual Routing and Forwarding (VRF) account. For information about the {{site.data.keyword.bplong_notm}} private end points, see [{{site.data.keyword.bplong_notm}} endpoint prerequisites](/docs/schematics?topic=schematics-private-endpoints#private-network-prereqs).
2. Export the following environment variables on your local machine. For more information, about supported private service endpoints for each {{site.data.keyword.cloud_notm}} service to support in production, see [Use service endpoints](/docs/account?topic=account-vrf-service-endpoint).
3. Initialize the Terraform CLI to load the environment variables that you set.

```
terraform init
```
{: pre}


  
|Service|Environment variable key|Public service endpoint for staging|
|-------------|--------|-----------------------|
|Account management|`IBMCLOUD_ACCOUNT_MANAGEMENT_API_ENDPOINT`|`https://accountmanagement.stage1.ng.bluemix.net`|
|Certificate manager|`IBMCLOUD_CERTIFICATE_MANAGER_API_ENDPOINT`|`https://us-south.certificate-manager.test.cloud.ibm.com`|
|Cloud Foundry|`IBMCLOUD_MCCP_API_ENDPOINT`|`https://mccp.us-south.cf.test.cloud.ibm.com`|
|Cloud functions|`IBMCLOUD_NAMESPACE_API_ENDPOINT`|`https://us-south.functions.cloud.ibm.com/api/v1`|
|Containers|`IBMCLOUD_CS_API_ENDPOINT`|`https://containers.test.cloud.ibm.com/global`|
|CIS|`IBMCLOUD_CIS_API_ENDPOINT`|`https://api.cis.test.cloud.ibm.com`|
|Direct Link|`IBMCLOUD_DL_API_ENDPOINT`|`https://directlink.test.cloud.ibm.com/v1`|
|GHoST / Tagging|`IBMCLOUD_GT_API_ENDPOINT`|`https://tags.global-search-tagging.test.cloud.ibm.com`|
|HPCS|`IBMCLOUD_HPCS_API_ENDPOINT`| `https://zcryptobroker.stage1.mybluemix.net/crypto_v2/` |
|IAM|`IBMCLOUD_IAM_API_ENDPOINT`|`https://iam.test.cloud.ibm.com`|
|IAMPAP|`IBMCLOUD_IAMPAP_API_ENDPOINT`|`https://iam.test.cloud.ibm.com`|
|ICD|`IBMCLOUD_ICD_API_ENDPOINT`|`https://api.us-south.databases.test.cloud.ibm.com`|
|Key protect|`IBMCLOUD_KP_API_ENDPOINT`|`https://qa.us-south.kms.test.cloud.ibm.com`||
|Private DNS|`IBMCLOUD_PRIVATE_DNS_API_ENDPOINT`| `https://api.dns-svcs.cloud.ibm.com/v1`|
|Resource management|`IBMCLOUD_RESOURCE_MANAGEMENT_API_ENDPOINT`|`https://resource-controller.test.cloud.ibm.com`|
|Resource controller|`IBMCLOUD_RESOURCE_CONTROLLER_API_ENDPOINT`|`https://resource-controller.test.cloud.ibm.com`|
|Resource catalog|`IBMCLOUD_RESOURCE_CATALOG_API_ENDPOINT`|`https://globalcatalog.test.cloud.ibm.com`|
|Schematics|`IBMCLOUD_SCHEMATICS_API_ENDPOINT`|`https://schematics.test.cloud.ibm.com`|
|Transit Gateway|`IBMCLOUD_TG_API_ENDPOINT`| `https://transit.test.cloud.ibm.com/v1`|
|UAA|`IBMCLOUD_UAA_ENDPOINT`|`https://login.stage1.ng.bluemix.net/UAALoginServerWAR`|
|User management|`IBMCLOUD_USER_MANAGEMENT_ENDPOINT`|`https://user-management.test.cloud.ibm.co`|
|VPC Gen1|`IBMCLOUD_IS_API_ENDPOINT`|`https://us-south-stage01.iaasdev.cloud.ibm.com`|
|VPC Gen2|`IBMCLOUD_IS_NG_API_ENDPOINT`|`https://us-south-stage01.iaasdev.cloud.ibm.com`|

