---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-30"

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

# Setting up the CLI and preparing your environment
{: #setup_cli}

Before you can automate your {{site.data.keyword.cloud_notm}} resource provisioning, you must install the Terraform CLI and the {{site.data.keyword.cloud_notm}} Provider plug-in.
{: shortdesc}

## Installing the Terraform CLI and the IBM Cloud Provider plug-in
{: #install_cli}

To use Terraform to manage {{site.data.keyword.cloud_notm}} resources, you must install the Terraform CLI and the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. 
{: shortdesc}

**What is the {{site.data.keyword.cloud_notm}} Provider plug-in and why do I need it?**</br>
To support a multi-cloud approach, Terraform works with cloud providers. A cloud provider is responsible for understanding the resources that you can provision, their API, and the methods to expose these resources in the cloud. To provision resources in {{site.data.keyword.cloud_notm}}, you must install the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. The plug-in is aware of the {{site.data.keyword.cloud_notm}} resources that you can provision with Terraform and the syntax to describe them.  

1. Install Terraform on your local machine. 
   1. Create a folder on your local system that is called `terraform` and navigate into your folder. 
      ```
      mkdir terraform && cd terraform
      ```
      {: pre}

   2. [Download the Terraform CLI version 0.11.x to your local machine ![External link icon](../icons/launch-glyph.svg "External link icon")](https://releases.hashicorp.com/terraform/). 
   
      The {{site.data.keyword.cloud_notm}} Provider plug-in is not yet verified to work with Terraform version 0.12.x. To use the {{site.data.keyword.cloud_notm}} Provider plug-in, install Terraform version 0.11.x. 
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

2. Install the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. 
   1. [Download the latest version of the {{site.data.keyword.cloud_notm}} Provider binary file ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/IBM-Cloud/terraform-provider-ibm/releases). 
   2. Create a hidden folder for your plug-in. The {{site.data.keyword.cloud_notm}} Provider plug-in is used only by the Terraform CLI and is not meant to be accessed by the user.  
      ```
      mkdir $HOME/.terraform.d/plugins
      ```
      {: pre}
      
   3. Move the {{site.data.keyword.cloud_notm}} Provider plug-in into your hidden folder. 
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
      2018/09/25 17:30:14 {{site.data.keyword.cloud_notm}} Provider version 0.11.3  fdc4aa0f0547177f3ea8b14c7a58a849e240f64a
      This binary is a plugin. These are not meant to be executed directly.
      Please execute the program that consumes these plugins, which will load any plugins automatically
      ```
      {: screen}

## Retrieving required credentials for your resources
{: #retrieve_credentials}

Terraform uses the {{site.data.keyword.cloud_notm}} Provider plug-in to securely communicate with the {{site.data.keyword.cloud_notm}} REST API. To provision and work with {{site.data.keyword.cloud_notm}} resources, you must configure your {{site.data.keyword.cloud_notm}} Provider plug-in to use the credentials that are required to access your resource.
{: shortdesc}

The credentials that you need depend on the type of {{site.data.keyword.cloud_notm}} resource that you want to provision. You can decide to provision {{site.data.keyword.cloud_notm}} platform services, {{site.data.keyword.cloud_notm}} classic infrastructure and {{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC) infrastructure resources, and {{site.data.keyword.containerlong_notm}} and {{site.data.keyword.cloud_notm}} Functions components. 
  
To retrieve the credentials: 

1. Review the following table to determine the required credentials for your resource. To retrieve the category that your resource belongs to, see the [{{site.data.keyword.cloud_notm}} Provider plug-in reference](https://ibm-cloud.github.io/tf-ibm-docs/) for more information. 

   <table>
   <thead>
     <th>Provider plug-in resource category</th>
     <th>Description</th>
     <th>Required credentials</th>
  </thead>
  <tbody>
    <tr>
      <td>Container data sources and resources</td>
      <td>Retrieve information or create, update, or delete a Kubernetes cluster and worker nodes in {{site.data.keyword.containerlong_notm}}.</td>
      <td><ul><li>{{site.data.keyword.cloud_notm}} classic infrastructure user name</li><li>{{site.data.keyword.cloud_notm}} classic infrastructure API key</li><li>{{site.data.keyword.cloud_notm}} API key</li></ul></td>
    </tr>
    <tr>
      <td>Infrastructure data sources and resources</td>
      <td>Retrieve information, or create, update, or delete {{site.data.keyword.cloud_notm}} classic infrastructure resources.</td>
      <td><ul><li>{{site.data.keyword.cloud_notm}} classic infrastructure user name</li><li>{{site.data.keyword.cloud_notm}} classic infrastructure API key</li></ul></td>
    </tr>
    <tr>
      <td>VPC Services data sources and resources</td>
      <td>Retrieve information, or create, update, or delete a Virtual Private Cloud (VPC) instance and infrastructure resources in your VPC.</td>
      <td><ul><li>{{site.data.keyword.cloud_notm}} API key</li><li>SSH key (for VPC virtual servers)</li></ul></td>
    </tr>
    <tr>
      <td>Identity and Access data sources and resources</td>
      <td>Retrieve information, or create, update, or delete {{site.data.keyword.cloud_notm}} account settings and service IDs. </td>
      <td>{{site.data.keyword.cloud_notm}} API key</td>
    </tr>
    <tr>
      <td>Cloud Foundry data sources and resources</td>
      <td>Retrieve information or create, update, or delete Cloud Foundry services, organizations, and spaces.</td>
      <td>{{site.data.keyword.cloud_notm}} API key</td>
    </tr>
    <tr>
      <td>Functions data sources and resources</td>
      <td>Retrieve information, or create, update, or delete {{site.data.keyword.cloud_notm}} Functions resources.</td>
      <td>{{site.data.keyword.cloud_notm}} API key</td>
    </tr>
  </tbody>
  </table>
   
2. To provision VPC infrastructure resources and {{site.data.keyword.cloud_notm}} platform services, [create an {{site.data.keyword.cloud_notm}} API key](/docs/iam?topic=iam-userapikey#create_user_key).

3. To connect to a VPC infrastructure virtual server via SSH, create an SSH key. 
   1. [Create the SSH key](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-ssh-keys). 
   2. To upload the SSH key to {{site.data.keyword.cloud_notm}} account, open the [{{site.data.keyword.cloud_notm}} VPC console](https://cloud.ibm.com/vpc/compute/sshKeys). 
   3. From the navigation, click **Compute** > **SSH key**. 
   4. Click **Add SSH key**. 
   5. Enter a name for your SSH key, and select the resource group and the region, for which you want to use the SSH key. 
   6. Copy the public SSH key value in the **Public key** field. 
   7. Click **Add SSH key** to save your SSH key. 
   
4. To provision {{site.data.keyword.cloud_notm}} classic infrastructure resources, retrieve the classic infrastructure user name and API key. For more information, see [Managing classic infrastructure API keys](/docs/iam?topic=iam-classic_keys)

## Storing your credentials in a local Terraform variables file
{: #store_credentials}

To separate your {{site.data.keyword.cloud_notm}} credentials from your Terraform configuration files so that you can publish your configuration files in a version control system, you can use a local Terraform variables file to store your sensitive information. 
{: shortdesc}

The local Terraform variables file is named `terraform.tfvars` and must be present in the same directory as your Terraform configuration files. When you initialize the Terraform CLI, all variables that are defined in this file are automatically loaded by Terraform and you can reference them in every Terraform configuration file in the same directory. 

Because the `terraform.tfvars` file contains confidential information, do not push this file to your version control system where you store the Terraform configuration files of the resources that you want to provision. The `terraform.tfvars` file is meant to be on your local system only. 
{: important}

1. Optional. Create a directory on your local machine for your Terraform project and navigate into the directory. This directory is used to store all configuration files and variable definitions. If you have an existing directory that you want to use, navigate into this directory. 
   ```
   mkdir myproject && cd myproject
   ```
   {: pre}  

2. Create a Terraform configuration file that is named `terraform.tfvars` to store the credentials that you retrieved. Make sure that you include the credentials that are required to provision your resource. To determine what credentials you need, see [Retrieving required credentials for your resources](#retrieve_credentials).
   ```
   ibmcloud_api_key = "<ibmcloud_api_key>"
   ssh_key = "<ssh_key_name>"
   softlayer_username = "<classic_infrastructure_username>"
   softlayer_api_key = "<classic_infrasturcture_apikey>"
   ```
   {: codeblock}
   
   <table>
   <caption>Understanding the configuration file components</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the configuration file components</th>
   </thead>
   <tbody>
   <tr>
   <td><code>ibmcloud_api_key</code></td>
   <td>Enter your {{site.data.keyword.cloud_notm}} API key. </td>
   </tr>
   <tr>
   <td><code>ssh_key</code></td>
   <td>Enter the name of the SSH key that you uploaded to your {{site.data.keyword.cloud_notm}} account. </td>
   </tr>
   <tr>
   <td><code>softlayer_username</code></td>
   <td>Enter your {{site.data.keyword.cloud_notm}} classic infrastructure user name.  </td>
   </tr>
   <tr>
   <td><code>softlayer_api_key</code></td>
   <td>Enter your {{site.data.keyword.cloud_notm}} classic infrastructure API key. </td>
   </tr>
   </tbody>
   </table>

## Configuring the provider plug-in
{: #configure_provider}

Because Terraform supports multiple cloud providers, you must specify the cloud provider that you want to use to provision your resources. 
{: shortdesc}

The cloud provider configuration file is named `provider.tf`. Terraform automatically loads the cloud provider configuration when you want to provision resources to determines what cloud provider plug-in to call. To provision {{site.data.keyword.cloud_notm}} resources, you must specify `IBM` as your cloud provider and provide the credentials that the {{site.data.keyword.cloud_notm}} Provider plug-in needs to successfully provision your resources. 

1. Review the following table to determine the parameters that you need to configure the {{site.data.keyword.cloud_notm}} Provider plug-in for your resource. 

   <table>
   <thead>
   <th>Resource</th>
     <th>Required input parameters</th>
  </thead>
  <tbody>
    <tr>
      <td>VPC infrastructure resources</td>
      <td><ul><li>{{site.data.keyword.cloud_notm}} API key</li><li>Generation of {{site.data.keyword.cloud_notm}} VPC infrastructure</li><li>{{site.data.keyword.cloud_notm}} region</li></ul></td>
    </tr>
    <tr> 
      <td>Classic infrastructure resources</li>
    <td><ul><li>{{site.data.keyword.cloud_notm}} classic infrastructure user name</li><li>{{site.data.keyword.cloud_notm}} classic infrastructure API key</li><li>{{site.data.keyword.cloud_notm}} region</li></ul></td>
    </tr>
    <tr>
  <td>{{site.data.keyword.containerlong_notm}} resources</td>
  <td><ul><li>{{site.data.keyword.cloud_notm}} API key</li><li>{{site.data.keyword.cloud_notm}} classic infrastructure user name</li><li>{{site.data.keyword.cloud_notm}} classic infrastructure API key</li><li>{{site.data.keyword.cloud_notm}} region</li></ul></td>
  </tr>
  <tr>
  <td>All other resources</td>
  <td><ul><li>{{site.data.keyword.cloud_notm}} API key</li><li>{{site.data.keyword.cloud_notm}} region</li></ul></td>
  </tr>
  </tbody>
  </table>
   
2. Create a Terraform provider configuration file that is named `provider.tf`. Use this file to specify IBM as your cloud provider and reference the credentials from your `terraform.tfvars` file. To reference a variable, declare the variable first, and then retrieve the value of the variable by using the `${var.<variable_name>}` syntax. You can specify additional variables in this file that are required by the {{site.data.keyword.cloud_notm}} Provider plug-in to successfully provision your resources. Make sure that you store this file in the same folder as your `terraform.tfvars` file. 
   ```
   variable "ibmcloud_api_key" {}
   variable "softlayer_username" {}
   variable "softlayer_api_key" {}
   
   provider "ibm" {
   ibmcloud_api_key    = "${var.ibmcloud_api_key}"
   generation = 1
   region = "us-south"
   softlayer_username = "${var.softlayer_username}"
   softlayer_api_key  = "${var.softlayer_api_key}"
   }
   ```
   {: codeblock}
   
   <table>
   <caption>Understanding the configuration file components</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the configuration file components</th>
   </thead>
   <tbody>
   <tr>
   <td><code>ibmcloud_api_key</code></td>
   <td>Reference the {{site.data.keyword.cloud_notm}} API key that you stored in your <code>terraform.tfvars</code> file. This API key is required to provision {{site.data.keyword.cloud_notm}} platform and VPC infrastructure resources. You can remove this credential if you want to provision classic infrastructure resources only.  </td>
   </tr>
   <tr>
   <td><code>generation</code></td>
   <td>Enter <strong>1</strong> to configure the {{site.data.keyword.cloud_notm}} provider plug-in to provision your VPC resources on {{site.data.keyword.cloud_notm}} classic infrastructure (VPC on Classic). You can remove this parameter if you want to provision only classic infrastructure resources that are not in a VPC. </td>
   </tr>
   <tr>
   <td><code>region</code></td>
   <td>Specify the {{site.data.keyword.cloud_notm}} region where you want to provision your resources. Run <code>ibmcloud is regions</code> to find supported regions.  </td>
   </tr>
   <tr>
   <td><code>softlayer_username</code></td>
   <td>Reference the {{site.data.keyword.cloud_notm}} classic infrastructure user name that you stored in your <code>terraform.tfvars</code> file. This user name is required to provision {{site.data.keyword.cloud_notm}} classic infrastructure resources. You can remove this credential if you want to provision {{site.data.keyword.cloud_notm}} platform or VPC infrastructure resources only. </td>
   </tr>
   <tr>
   <td><code>softlayer_api_key</code></td>
   <td>Reference the {{site.data.keyword.cloud_notm}} classic infrastructure API key that you stored in your <code>terraform.tfvars</code> file. This API key is required to provision {{site.data.keyword.cloud_notm}} classic infrastructure resources. You can remove this credential if you want to provision {{site.data.keyword.cloud_notm}} platform or VPC infrastructure resources only.   </td>
   </tr>
   </tbody>
   </table>
