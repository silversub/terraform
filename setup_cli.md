---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-28"

keywords: install Terraform cli, set up Terraform cli, ibm cloud provider plugin, ibm cloud for Terraform

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

# Setting up the Terraform CLI and the IBM Cloud Provider plug-in
{: #setup_cli}

Before you can automate your {{site.data.keyword.Bluemix_notm}} resource provisioning, you must install the Terraform CLI and the {{site.data.keyword.Bluemix_notm}} Provider plug-in.
{: shortdesc}

## Installing the Terraform CLI and the IBM Cloud Provider plug-in
{: #install_cli}

To use Terraform to manage {{site.data.keyword.Bluemix_notm}} resources, you must install the Terraform CLI and the {{site.data.keyword.Bluemix_notm}} Provider plug-in for Terraform. 
{: shortdesc}

**What is the {{site.data.keyword.Bluemix_notm}} Provider plug-in and why do I need it?**</br>
To support a multi-cloud approach, Terraform works with cloud providers. A cloud provider is responsible for understanding the resources that you can provision, their API, and the methods to expose these resources in the cloud. To provision resources in {{site.data.keyword.Bluemix_notm}}, you must install the {{site.data.keyword.Bluemix_notm}} Provider plug-in for Terraform. The plug-in is aware of the {{site.data.keyword.Bluemix_notm}} resources that you can provision with Terraform and the syntax to describe them.  

1. Install Terraform on your local machine. 
   1. Create a folder on your local system that is called `terraform` and navigate into your folder. 
      ```
      mkdir terraform && cd terraform
      ```
      {: pre}

   2. [Download the Terraform CLI version 0.11.x to your local machine ![External link icon](../icons/launch-glyph.svg "External link icon")](https://releases.hashicorp.com/terraform/). 
   
      The {{site.data.keyword.Bluemix_notm}} Provider plug-in is not yet verified to work with Terraform version 0.12.x. To use the {{site.data.keyword.Bluemix_notm}} Provider plug-in, install Terraform version 0.11.x. 
      {: important}

   3. Extract the Terraform package and copy the binary file into your `terraform` directory. 
   4. Point the `$PATH` environment variable to your Terraform binary file.
      ```
      export PATH=$PATH:$HOME/terraform
      ```
      {: pre}
      
   5. Verify that the installation is successful by using a `terraform` command.
      ```
      terraform
      ```
      {: pre}
      
      Example output: 
      ```
      Usage: terraform [-version] [-help] <command> [args]

      The available commands for execution are listed below.
      The most common, useful commands are shown first, followed by less common or more advanced commands. If you're just getting started with Terraform, stick with the common commands. For the other commands, please read the help and docs before usage.

      Common commands:
          apply              Builds or changes infrastructure
          console            Interactive console for Terraform interpolations
          destroy            Destroy Terraform-managed infrastructure
          env                Workspace management
          fmt                Rewrites config files to canonical format
          get                Download and install modules for the configuration
          graph              Create a visual graph of Terraform resources
          import             Import existing infrastructure into Terraform
          init               Initialize a Terraform working directory
          output             Read an output from a state file
          plan               Generate and show an execution plan
          providers          Prints a tree of the providers used in the configuration
          push               Upload this Terraform module to Atlas to run
          refresh            Update local state file against real resources
          show               Inspect Terraform state or plan
          taint              Manually mark a resource for recreation
          untaint            Manually unmark a resource as tainted
          validate           Validates the Terraform files
          version            Prints the Terraform version
          workspace          Workspace management

      All other commands:
          debug              Debug output management (experimental)
          force-unlock       Manually unlock the terraform state
          state              Advanced state management
      ```
      {: screen}  

2. Install the {{site.data.keyword.Bluemix_notm}} Provider plug-in for Terraform. 
   1. [Download the latest version of the {{site.data.keyword.Bluemix_notm}} Provider binary file ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/IBM-Cloud/terraform-provider-ibm/releases). 
   2. Create a hidden folder for your plug-in. The {{site.data.keyword.Bluemix_notm}} Provider plug-in is used only by the Terraform CLI and is not meant to be accessed by the user.  
      ```
      mkdir $HOME/.terraform.d/plugins
      ```
      {: pre}
      
   3. Move the {{site.data.keyword.Bluemix_notm}} Provider plug-in into your hidden folder. 
      ```
      mv $HOME/Downloads/terraform-provider-ibm* $HOME/.terraform.d/plugins/
      ```
      {: pre}
      
   4. Navigate into your hidden directory and verify that the installation is complete. 
      ```
      cd $HOME/.terraform.d/plugins && ./terraform-provider-ibm_*
      ```
      {: pre}
      
      Example output: 
      ```
      2018/09/25 17:30:14 {{site.data.keyword.Bluemix_notm}} Provider version 0.11.3  fdc4aa0f0547177f3ea8b14c7a58a849e240f64a
      This binary is a plugin. These are not meant to be executed directly.
      Please execute the program that consumes these plugins, which will load any plugins automatically
      ```
      {: screen}

## Configuring the IBM Cloud Provider plug-in
{: #configure_provider}

Terraform uses the {{site.data.keyword.Bluemix_notm}} Provider plug-in to securely communicate with the {{site.data.keyword.Bluemix_notm}} REST API. To provision and work with {{site.data.keyword.Bluemix_notm}} resources, you must configure your {{site.data.keyword.Bluemix_notm}} Provider plug-in to use the credentials that are required to access your resource.

### Determining required credentials
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

### Preparing the credentials for Terraform
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
