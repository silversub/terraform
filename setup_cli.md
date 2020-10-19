---

copyright:
  years: 2017, 2020
lastupdated: "2020-10-19"

keywords: install Terraform cli, set up Terraform cli, ibm cloud provider plugin, ibm cloud for Terraform

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

   2. [Download the Terraform CLI version 0.12.x to your local machine ![External link icon](../icons/launch-glyph.svg "External link icon")](https://releases.hashicorp.com/terraform/). 
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
      The most common, useful commands are shown first, followed by less common or more advanced commands. If you're just getting started with Terraform, stick with the common commands. For the other commands, please read the help and Docs  before usage.

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
          force-unlock       Manually unlock the Terraform state
          state              Advanced state management
      ```
      {: screen}  

2. Install the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. 
   1. [Download the latest version of the {{site.data.keyword.cloud_notm}} Provider binary package ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/IBM-Cloud/terraform-provider-ibm/releases). 
   2. Extract the {{site.data.keyword.cloud_notm}} Provider package to retrieve the binary file.
   3. Create a hidden folder for your plug-in. The {{site.data.keyword.cloud_notm}} Provider plug-in is used only by the Terraform CLI and is not meant to be accessed by the user.  
      ```
      mkdir -p $HOME/.terraform.d/plugins
      ```
      {: pre}
      
   4. Move the {{site.data.keyword.cloud_notm}} Provider plug-in into your hidden folder. 
      ```
      mv $HOME/Downloads/terraform-provider-ibm* $HOME/.terraform.d/plugins

       ```
      {: pre}
      
   5. Navigate into your hidden directory and verify that the installation is complete. 
      ```
      cd $HOME/.terraform.d/plugins && ./terraform-provider-ibm_*
      ```
      {: pre}
      
      Example output: 
      ```
      2018/09/25 17:30:14 {{site.data.keyword.cloud_notm}} Provider version 0.11.3  fdc4aa0f0547177f3ea8b14c7a58a849e240f64a
      This binary is a plugin. These are not meant to be executed directly.
      Please execute the program that consumes these plugins, which loads any plugins automatically
      ```
      {: screen}
      
3. [Configure the provider plug-in](#configure_provider).
      
## Migrating your Terraform configuration files from version 0.11 to version 0.12
{: #tf-0.12-migration}
  
Update your Terraform configuration files from version 0.11 to version 0.12 so that you can run your Terraform code with the Terraform version 0.12 

With the release of Terraform version 0.12, the syntax for configuration files changed. If you want to run your infrastructure code by using Terraform version 0.12, you must first update your configuration files to apply the new syntax. 
{: important}

1. Follow the [instructions](#install_cli) to install Terraform version 0.12 and the latest release of the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. 
2. Copy your Terraform version 0.11 configuration files into your Terraform working directory. 
   ```
   mv <tf_config_file_path> $HOME/terraform
   ```
   {: pre}
   
3. Use the Terraform version 0.12 CLI to automatically apply the new syntax to your Terraform configuration files. 
   ```
   terraform 0.12 upgrade
   ```
   {: pre}
   
   Example output: 
   ```
   This command rewrites the configuration files in the given directory so
   that they use the new syntax features from Terraform v0.12, and identify
   any constructs that may need to be adjusted for correct operation with
   Terraform v0.12.

   We recommend to use this command in a clean version control work tree, so that
   you can easily see the proposed changes as a diff against the latest commit.
   If you have uncommited changes already present, we recommend aborting this
   command and dealing with them before running this command again.

   Would you like to upgrade the module in the current directory?
     Only 'yes' is accepted to confirm.

     Enter a value: yes

   -----------------------------------------------------------------------------

   Upgrade complete!

   The configuration files were upgraded successfully. Use your version control
   system to review the proposed changes, make any necessary adjustments, and
   then commit.
   ```
   {: screen}
   
4. Open your Terraform configuration file to verify the changes. 


## Configuring the provider plug-in
{: #configure_provider}

Because Terraform supports multiple cloud providers, you must specify IBM as your cloud provider and configure the plug-in with all required parameters for the resource or data source category that you want to use. 
{: shortdesc}

1. Review the required [input parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) for the resource or data source category that you want to use and retrieve these parameters by using the [Supported input parameters](/docs/terraform?topic=terraform-provider-reference#provider-parameter-ov) documentation.

2. Optional. Create a directory on your local machine for your Terraform project and navigate into the directory. This directory is used to store all Terraform configuration files, the provider configuration, and variable definitions. If you have an existing directory that you want to use, navigate into this directory. 
   ```
   mkdir myproject && cd myproject
   ```
   {: pre}  
   
3. Create a local Terraform variables file that is named `terraform.tfvars` to store the credentials and other input parameters that you retrieved earlier. When you initialize the Terraform CLI, all variables that are defined in this file are automatically loaded by Terraform and you can reference them in every Terraform configuration file in the same directory. 

   Because the `terraform.tfvars` file contains confidential information, do not push this file to your version control system where you store the Terraform configuration files of the resources that you want to provision. The `terraform.tfvars` file is meant to be on your local system only. 
   {: important}
   
   Example `terraform.tfvars` file:
   ```
   ibmcloud_api_key = "<ibmcloud_api_key>"
   iaas_classic_username = "<classic_infrastructure_username>"
   iaas_classic_api_key = "<classic_infrasturcture_apikey>"
   region = "<region>"
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
   <td><code>iaas_classic_username</code></td>
   <td>Enter your {{site.data.keyword.cloud_notm}} classic infrastructure user name.  </td>
   </tr>
   <tr>
   <td><code>iaas_classic_api_key</code></td>
   <td>Enter your {{site.data.keyword.cloud_notm}} classic infrastructure API key. </td>
   </tr>
   <tr>
   <td><code>region</code></td>
   <td>Enter the {{site.data.keyword.cloud_notm}} region where you want to provision your resources. </td>
   </tr>
   </tbody>
   </table>

4. In the same directory, create a Terraform provider configuration file that is named `provider.tf`. Use this file to specify IBM as your cloud provider and to reference the credentials from your `terraform.tfvars` file. To reference a variable, declare the variable first, and then retrieve the value of the variable by using Terraform interpolation syntax. You can also specify more variables in this file that you did not include in your `terraform.tfvars` file. 
   ```
   variable "ibmcloud_api_key" {}
   variable "iaas_classic_username" {}
   variable "iaas_classic_api_key" {}
   variable "region" {}
   
   provider "ibm" {
   ibmcloud_api_key = var.ibmcloud_api_key
   generation = 1
   region = var.region
   iaas_classic_username = var.iaas_classic_username
   iaas_classic_api_key  = var.iaas_classic_api_key
   }
   ```
   {: codeblock}
   
5. After you configured the provider with all required input parameters, you can now start [provisioning {{site.data.keyword.cloud_notm}} resources](/docs/terraform?topic=terraform-manage_resources#provision_resources). 
   

