---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-28"

keywords: Terraform, ibm cloud Terraform, ibm cloud provider plugin for Terraform, softlayer, iaas

subcollection: terraform

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Configuring the IBM Cloud Provider plug-in
{: #configure_provider-old}

Terraform uses the {{site.data.keyword.Bluemix_notm}} Provider plug-in to securely communicate with the {{site.data.keyword.Bluemix_notm}} REST API. To provision and work with {{site.data.keyword.Bluemix_notm}} resources, you must configure your {{site.data.keyword.Bluemix_notm}} Provider plug-in to use the credentials that are required to access your resource.

## Determining required credentials
{: #determine_credentials}

The credentials that you need depend on the type of {{site.data.keyword.Bluemix_notm}} resource that you want to provision. You can decide to provision {{site.data.keyword.Bluemix_notm}} Platform services, {{site.data.keyword.Bluemix_notm}} classic infrastructure, {{site.data.keyword.Bluemix_notm}} Virtual Private Cloud (VPC), and {{site.data.keyword.containerlong_notm}} and {{site.data.keyword.Bluemix_notm}} Functions resources. Review the following table to see what credentials are required for each of the available resources. 

Looking for a full list of {{site.data.keyword.Bluemix_notm}} resources that you can provision with the {{site.data.keyword.Bluemix_notm}} Provider plug-in? See the [{{site.data.keyword.Bluemix_notm}} Provider reference](https://ibm-cloud.github.io/tf-ibm-docs/) for more information. 
{: tip}
  
|Resource|Description|Required credentials|
|---|----|---------|
|Container data sources and resources|Retrieve information or create, update, or delete a Kubernetes cluster and worker nodes in {{site.data.keyword.containerlong_notm}}.|<ul><li>{{site.data.keyword.Bluemix_notm}} classic infrastructure user name</li><li>{{site.data.keyword.Bluemix_notm}} classic infrastructure API key</li><li>{{site.data.keyword.Bluemix_notm}} API key</li></ul>|
|Infrastructure data sources and resources|Retrieve information, or create, update, or delete {{site.data.keyword.Bluemix_notm}} classic infrastructure instances. |<ul><li>{{site.data.keyword.Bluemix_notm}} classic infrastructure user name</li><li>{{site.data.keyword.Bluemix_notm}} classic infrastructure API key</li></ul>|
|VPC Services data sources and resources|Retrieve information, or create, update, or delete a Virtual Private Cloud (VPC) and classic infrastructure instances that are provisioned in the VPC.|{{site.data.keyword.Bluemix_notm}} API key|
|Identity and Access data sources and resources|Retrieve information, or create, update, or delete {{site.data.keyword.Bluemix_notm}} account settings and service IDs. |{{site.data.keyword.Bluemix_notm}} API key|
|Cloud Foundry data sources and resources|Retrieve information or create, update, or delete Cloud Foundry services, organizations, and spaces.|{{site.data.keyword.Bluemix_notm}} API key|
|Functions data sources and resources|Retrieve information, or create, update, or delete {{site.data.keyword.Bluemix_notm}} Functions resources.|{{site.data.keyword.Bluemix_notm}} API key|

## Preparing the credentials for Terraform
{: prepare_credentials}

Choose an option for how to provide your {{site.data.keyword.Bluemix_notm}} credentials to Terraform. 
{: shortdesc} 

Want to set up a cloud environment that includes multiple cloud providers? Include the required credentials for each cloud provider. 
{: tip}

To prepare your credentials: 

1. Retrieve the credentials that you need to provision your resource. To determine the credentials that you need, see [Determining required credentials](#determine_credentials). 
   
   <table>
   <thead>
     <th>Type of credential</th>
     <th>Documentation link</th>
   </thead>
   <tbody>
    <tr>
      <td>{{site.data.keyword.Bluemix_notm}} API key</td>
      <td>[Creating an API key](/docs/iam?topic=iam-userapikey#create_user_key)</td>
    </tr>
    <tr>
      <td>{{site.data.keyword.Bluemix_notm}}  lassic infrastructure user name and API key</td>
      <td>[Managing classic infrastructure API keys](/docs/iam?topic=iam-classic_keys)</td>
    </tr>
   </tbody>
   </table>

2. Create a folder on your local machine for your Terraform project and navigate into the folder. This folder is used to store all configuration files and variable definitions.
   ```
   mkdir myproject && cd myproject
   ```
   {: pre}

3. Create a Terraform configuration file that is named `terraform.tfvars` to store the credentials that you retrieved. Variables that are defined in the `terraform.tfvars` file are automatically loaded by Terraform when the Terraform CLI is initialized. 
   ```
   softlayer_username = "<infrastructure_username>"
   softlayer_api_key = "<infrasturcture_apikey>"
   ibmcloud_api_key = "<platform_api_key>"
   ```
   {: codeblock}
   
   <table>
   <caption>Understanding the configuration file components</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the configuration file components</th>
   </thead>
   <tbody>
   <tr>
   <td><code>softlayer_username</code></td>
   <td>Enter the {{site.data.keyword.Bluemix_notm}} classic infrastructure user name that you retrieved earlier.  </td>
   </tr>
   <tr>
   <td><code>softlayer_api_key</code></td>
   <td>Enter the {{site.data.keyword.Bluemix_notm}} classic infrastructure API key that you retrieved earlier. </td>
   </tr>
   <tr>
   <td><code>ibmcloud_api_key</code></td>
   <td>Enter the {{site.data.keyword.Bluemix_notm}} platform API key that you retrieved earlier. </td>
   </tr>
   </tbody>
   </table>
   
4. Create a Terraform provider configuration file that is named `provider.tf`. Use this file to specify the {{site.data.keyword.Bluemix_notm}} provider and reference the credentials from your `terraform.tfvars` file that you want to provide to Terraform.
   ```
   variable "softlayer_username" {}
   variable "softlayer_api_key" {}
   variable "ibmcloud_api_key" {}
   
   provider "ibm" {
   ibmcloud_api_key    = "${var.ibmcloud_api_key}"
   softlayer_username = "${var.softlayer_username}"
   softlayer_api_key  = "${var.softlayer_api_key}"
   }
   ```
   {: codeblock}
  
Now that you are all set, you can go ahead and start [specifying and provisioning your {{site.data.keyword.Bluemix_notm}} resources](/docs/terraform?topic=terraform-manage_resources#manage_resources) with Terraform. 

