---

copyright:
  years: 2017, 2020
lastupdated: "2020-10-20"

keywords: Terraform, ansible, wordpress, automate, automation, iaas, highly available, multizone, cross-region

subcollection: terraform

content-type: tutorial
services: terraform, virtual-servers
account-plan: 
completion-time: 1h

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

 
# Tutorial: Deploying Wordpress in a highly available, cross-region web site architecture with Terraform and Ansible  
{: #multi_region}
{: toc-content-type="tutorial"}
{: toc-services="terraform, virtual-servers"}
{: toc-completion-time="1h"}

Use this tutorial to provision a highly available web site architecture across multiple regions on {{site.data.keyword.cloud_notm}} classic infrastructure with Terraform. Then, deploy a single instance of Wordpress and replicate Wordpress across the two regions to explore the security and resiliency features of {{site.data.keyword.cloud_notm}} Security Groups, DNS, Web Application Firewall (WAF), and global and local load balancers. With this setup, you can create a resilient, secure, and scalable web environment with highly available web sites that use custom DNS domain names. Terraform and Ansible are loosely integrated through the sharing of inventory information.
{: shortdesc}
 
## Solution overview
{: #overview}

The following image shows the classic infrastructure and software components of the highly available web site architecture that you provision as part of this tutorial. 
The basic web site architecture that is used in [Tutorial: Deploying Wordpress on {{site.data.keyword.cloud_notm}} classic infrastructure with Terraform and Ansible](/docs/terraform/tutorials?topic=terraform-deploy_wordpress#deploy_wordpress) is deployed into each {{site.data.keyword.cloud_notm}} region. To balance the workload between the two regions, an {{site.data.keyword.cloud_notm}} Internet Services Load Balancer with a user-specified domain is deployed into your {{site.data.keyword.cloud_notm}} account and configured with a health check. The health check performs tests for each region to determine whether a region is available to receive network traffic. If one region becomes unavailable due to a network, app, or infrastructure failure, the health check determines this event and stops sending network traffic to the unavailable region.

{{site.data.keyword.cloud_notm}} Security Groups extend across regions and are used to secure all Apache web servers and the two MariaDB instances. The security groups provide secure outbound access to the open source repositories for Apache, MariaDB, and Wordpress. At the same time, the security groups deny all inbound internet traffic except via the {{site.data.keyword.cloud_notm}} Load Balancers. To secure your environment, the communication between the Wordpress instances and the MariaDB databases is limited to the private network and is allowed on the MariaDB ports only.

The Ansible playbooks install multiple Wordpress instances on Apache web servers and set up a replicated MariaDB database on {{site.data.keyword.cloud_notm}} classic virtual servers. To keep both MariaDB database instances in sync, a master-master replication between the two regions is set up on the private network. 

<img src="../images/multi_region_wordpress.png" alt="Infrastructure and app components to deploy Wordpress on {{site.data.keyword.cloud_notm}} across regions with Terraform and Ansible" width="800" style="width: 800px; border-style: none"/>

The following {{site.data.keyword.cloud_notm}} resources are provisioned for you as part of this tutorial. 

<table>
<caption>Web app infrastructure and software components</caption>
<thead>
<th>Tool</th>
<th>Resources</th>
</thead>
<tbody>
<tr>
<td>Terraform</td>
<td><ul>
  <li>6 {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Servers that run CentOS 7.x; 4 Virtual Servers are used for the Wordpress Apache web servers, 2 are used for the Wordpress MariaDB database</li><li>2 {{site.data.keyword.cloud_notm}} classic infrastructure Security Groups to secure your Wordpress Apache web servers and the MariaDB instances</li><li>2 {{site.data.keyword.cloud_notm}} CloudInit templates</li><li>2 {{site.data.keyword.cloud_notm}} classic infrastructure Load Balancers, one load balancer for each data center to balance workload between the Wordpress Apache web servers</li><li>1 {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer to connect the two regions</li><li>2 {{site.data.keyword.cloud_notm}} Internet Services origin pools</li><li>1 {{site.data.keyword.cloud_notm}} Internet Services health check to check whether the regions are available to receive workloads</li><li>{{site.data.keyword.cloud_notm}} DNS Registration and domain name servers for your custom domain</li><li>1 {{site.data.keyword.cloud_notm}} Internet Services DNS record</li><li>1 {{site.data.keyword.cloud_notm}} Internet Services Web Application Firewall with DDoS protection</li></ul></td>
</tr>
<tr>
<td>Ansible</td>
<td><ul>
  <li>4 Apache (HTTPD) app servers for your Wordpress instances</li>
  <li>4 Wordpress installations</li>
  <li>2 MariaDB database servers for your Wordpress database and the database replication</li>
  <li>1 MariaDB master-master database replication to keep the databases in both data centers in sync</li>
  <li>1 Wordpress database instance</li>
  <li>Automated Wordpress configuration via CLI</li>
</ul></td>
</tr>
</tbody>
</table>

This tutorial intends to demonstrate the capability of building secure, resilient, highly available, and scalable websites on {{site.data.keyword.cloud_notm}} classic infrastructure with cross-region networking. However, the tutorial does not intend to provide a fully operational Wordpress deployment. To run this tutorial, infrastructure costs incur for the virtual servers, load balancers, and the custom DNS domain name. The cost for the DNS domain name is fixed and depends on the domain name that you register and the duration that you request. The costs for your classic infrastructure resources depend on the number of hours or days that the classic infrastructure resources are provisioned for you. To cancel the billing for your resources, you must remove your classic infrastructure resources. 
{: important}

## Objectives
{: #objectives_multi_region}

In this tutorial, you use Terraform to deploy a highly available {{site.data.keyword.cloud_notm}} classic infrastructure setup that you use to deploy a Wordpress sample app across {{site.data.keyword.cloud_notm}} regions by using Ansible. In particular, you will:

- Set up your environment and all the software that you need for the highly available Wordpress app, including Terraform, the  {{site.data.keyword.cloud_notm}} Provider plug-in, and Ansible.
- Provision {{site.data.keyword.cloud_notm}} classic infrastructure components for your Wordpress sample app across regions by using Terraform.
- Import infrastructure resource information from Terraform to Ansible. 
- Deploy a sample Wordpress app across {{site.data.keyword.cloud_notm}} regions on your {{site.data.keyword.cloud_notm}} classic infrastructure with Ansible. 
- Use Ansible to finalize the setup of your Wordpress app across both regions. 
- Explore the high availability capabilities of the multi-region Wordpress architecture.  

## Audience
{: #audience_multi_region}

This tutorial is intended for network administrators, software developers, and architects who want to become familiar with {{site.data.keyword.cloud_notm}} classic infrastructure networking and compute components, learn how to use Terraform and Ansible to automate network configuration, and to deploy web infrastructure and apps on classic IaaS. 

## Prerequisites
{: #prerequisites_multi_region}
- If you do not have one, create an {{site.data.keyword.cloud_notm}} [Pay-As-You-Go or Subscription {{site.data.keyword.cloud_notm}} account ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/classic/services/domains). 
- [Set up a VPN connection and SSH authentication](/docs/terraform/ansible?topic=terraform-ansible#setup_vpn) to access {{site.data.keyword.cloud_notm}} classic infrastructure resources over the private network. 
- If you do not have an existing DNS domain that is registered with IBM Cloud, register one with the {{site.data.keyword.cloud_notm}} Domain Registration service. For more information, about how to register a new domain, see [Register a New Domain](/docs/dns?topic=dns-register-a-new-domain). To transfer an existing domain to {{site.data.keyword.cloud_notm}}, see [Transfer an Existing Domain to {{site.data.keyword.cloud_notm}}](/docs/dns?topic=dns-transfer-domains). 
- If you already completed the [Tutorial: Deploying Wordpress on {{site.data.keyword.cloud_notm}} classic infrastructure with Terraform and Ansible](/docs/terraform/tutorials?topic=terraform-deploy_wordpress#deploy_wordpress), you can reuse the Terraform and Ansible installations. 
  1. Remove the old Terraform `tf` configuration files from your Terraform project directory and follow step 1 and 6 in [Lesson 1](#setup_terraform) to copy the new Terraform configuration files into your project directory.
  2. Follow [Lesson 3](#provision_terraform_infrastructure) to provision the {{site.data.keyword.cloud_notm}} classic infrastructure. 
  3. After the classic infrastructure components are deployed, follow step 5 in [Lesson 4](#create_ansible_inventory) to update your Ansible classic infrastructure inventory. 
  4. To deploy your Wordpress app, continue with [Lesson 5](#install_configure_wordpress) in this tutorial. 

## Lesson 1: Setting up Terraform 
{: #setup_terraform}

To use Terraform to provision {{site.data.keyword.cloud_notm}} classic infrastructure resources, you must install Terraform and the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. The {{site.data.keyword.cloud_notm}} Provider plug-in is aware of all the {{site.data.keyword.cloud_notm}} resources that you can provision with Terraform, including the API and the methods to expose these resources in the cloud.
{: shortdesc}

1. Download the {{site.data.keyword.cloud_notm}} Terraform Provider project. This project contains all Terraform configuration files that you need in this tutorial.
   ```
   git clone https://github.com/IBM-Cloud/terraform-provider-ibm.git
   ```
   {: pre}

2. Install Terraform on your local machine. 
   1. Create a folder on your local system that is called `terraform` and navigate into your folder. 
      ```
      mkdir terraform && cd terraform
      ```
      {: pre}
      
   2. [Download the Terraform CLI version 0.11.x to your local machine ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://releases.hashicorp.com/terraform/). 
   
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
      The available commands for execution are listed.
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
          force-unlock       Manually unlock the terraform state
          state              Advanced state management
      ```
      {: screen}  
      
3. Install the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. 
   1. [Download the latest version of the {{site.data.keyword.cloud_notm}} Provider binary file ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/IBM-Cloud/terraform-provider-ibm/releases). 
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
      
4. Create an {{site.data.keyword.cloud_notm}} API key. Note the **API Key** that is created for you. The API key has the same permissions and {{site.data.keyword.cloud_notm}} IAM roles as your user ID. 
   ```
   ibmcloud iam api-key-create <apikey_name>
   ```
   {: pre}
   
   Example output: 
   ```
   Creating API key mykey as user@company.com...
   OK
   API key mykey was created
   Please preserve the API key! It cannot be retrieved after it's created.
                 
   Name          mykey   
   Description      
   Created At    2018-09-27T20:22+0000   
   API Key       a1BcdEfghIJkLmNopqrSTUV45W-ABC12DefGH3ij2klm   
   Locked        false   
   UUID          ApiKey-1a2v3c45-ab1c-1a2b-1234-a123b456712
   ```
   {: screen}
   
   Your API key is displayed in the **API Key** section of your CLI output. 
      
5. [Retrieve your {{site.data.keyword.cloud_notm}} infrastructure user name and API key](/docs/account?topic=account-classic_keys). 
   
6. Copy the Terraform configuration files to create your Wordpress infrastructure from the {{site.data.keyword.cloud_notm}} Terraform Provider package to your local Terraform project directory. 
   ```
   mv /terraform-provider-ibm/examples/ibm-website-multi-region/* <terraform_project_path>
   ```
   {: pre}
  
7. Store your credentials in the `terraform.tfvars` file.
   1. Navigate into your Terraform project directory and open the `terraform.tfvars` file. 
      ```
      cd <terraform_project_path> && nano terraform.tfvars
      ```
      {: pre}
      
   2. Add your classic infrastructure credentials and the {{site.data.keyword.cloud_notm}} API key. 
      ```
      softlayer_username = "<classic_infrastructure_username>"
      softlayer_api_key = "<classic_infrastructure_api_key>"
      ibmcloud_api_key = "<ibmcloud_api_key>"
      ```
      {: codeblock}
      
With your Terraform project directory set-up, you can continue to set up your Ansible work environment in Lesson 2. 

## Lesson 2: Setting up Ansible
{: #setup_ansible_multi_region}
Set up your Ansible project directory and install Ansible on your local machine to automate the deployment of Wordpress on your Terraform-deployed infrastructure. 
{: shortdesc}
1. On the same level as your Terraform project directory, create an Ansible project directory and navigate into the directory. 
   ```
   mkdir ansible && cd ansible
   ```
   {: pre}
   
2. Copy the Ansible playbooks and configuration files to create your Wordpress app from the `examples` folder of the {{site.data.keyword.cloud_notm}} Terraform Provider package to your Ansible project directory. 
   ```
   mv terraform-provider-ibm/examples/ansible/ibm_ansible_wordpress/* <ansible_project_path>
   ```
   {: pre}
   
3. Install Ansible on your local machine. 
   1. Install [Python version 3.6 or later ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.python.org/downloads/). 
   2. Install the Python package manager `pip`. Use the same version that you used when you installed Python. The following example assumes that you downloaded Python version 3.6 and want to install pip version 3.6. 
      ```
      python3.6 -m pip install 3.6
      ```
      {: pre}
   3. Install Ansible on your local machine. 
      ```
      sudo pip install ansible
      ```
      {: pre}
   
   4. Verify that Ansible is installed on your machine. 
      ```
      ansible --version
      ```
      {: pre}
   
      Example output: 
      ```
      ansible 2.7.0
      config file = None
      configured module search path = ['/Users/user/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
      ansible python module location = /Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages/ansible
      executable location = /Library/Frameworks/Python.framework/Versions/3.6/bin/ansible
      python version = 3.6.6 (v3.6.6:4cf1f54eb7, Jun 26 2018, 19:50:54) [GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.57)]
      ```
      {: screen}
4. If you run macOS on your local machine, securely store your local machine password with Ansible Vault in a `vault.yml` file. The Wordpress sample playbook package requires `sudo` permissions to execute updates to the host file and to install modules to monitor the state of the app. Your local machine password is stored as the encrypted variable `su_password` by Ansible Vault in the `vault.yml` file in the `group_vars/control` folder of your Ansible project directory. If you use a Linux distribution, this step is not required. 
   1. Create your `vault.yml` file. 
      ```
      ansible-vault create <ansible_project_path>/group_vars/control/vault.yml
      ```
      {: pre}
   2. Enter a password to protect your `vault.yml` file. Ansible Vault requires you to protect this file before you can work with your file. After you enter a password, Ansible Vault opens the editor that is stored in the `$EDITOR` environment variable. If this environment variable is not set, the `vi` editor is opened by default so that you can edit the file. 
   3. Add the `su_password` variable to your file and set the variable to your local machine password. 
      ```
      su_password: <password>
      ```
      {: pre}
 
   4. Save your changes and exit the editor. 
   5. To allow Ansible to decrypt the `vault.yml` file, store the password that you used to protect your `vault.yml` file in a `vault_pass.txt` file in your user home directory. The user home directory is defined in the `ansible.cfg` file as the default location to find the password. 
      ```
      echo "<vault_file_password>" > ~/vault_pass.txt
      ```
   
Great! Now that you completed the setup of Terraform and Ansible, you can start provisioning the Wordpress classic infrastructure in {{site.data.keyword.cloud_notm}} by using Terraform in Lesson 3. 
   
## Lesson 3: Provisioning the Wordpress classic infrastructure with Terraform
{: #provision_terraform_infrastructure}
In this lesson, you deploy the classic infrastructure virtual server instances, the {{site.data.keyword.cloud_notm}} classic infrastructure load balancers, an {{site.data.keyword.cloud_notm}} Internet Services instance, a global load balancer, and origin pools that you need for your Wordpress app. 
{: shortdesc}
1. In your Terraform project directory, update the `variables.tf` file. 
   1. Navigate into your Terraform project directory and edit the `variables.tf` file.
      ```
      cd <terraform_project_path> && nano variables.tf
      ```
      {: pre}
  2. Update the variable `domain` with the DNS domain that you registered earlier.    
      ```
      domain = "<example.com>"
      ```
      {: codeblock}   
  3. Specify the data centers where you want to deploy your resources. Make sure that these data centers belong to different {{site.data.keyword.cloud_notm}} regions. 
      ```
      variable datacenter1 {
        default = "<datacenter1>"
      }
      variable datacenter2 {
        default = "<datacenter2>"
      }
      ```
      {: codeblock} 
  4.  Enter an SSH label and the public SSH key that you created as part of the prerequisites in this tutorial.
      ```
      variable ssh_label {
        description = "ssh label"
        default     = "CloudSSHKey"
      }

      variable ssh_key {
        description = "ssh public key"
        default     = "<public_ssh_key>"
      }
      ```
      {: codeblock}
      
2. Review the content of the Terraform `tf` files. The resources that are deployed in this tutorial are spread across a number of files depending on the function that they perform. You can find the {{site.data.keyword.cloud_notm}} Internet Services resources in the `dns.tf` file. Information about {{site.data.keyword.cloud_notm}} classic infrastructure Security Groups is found in the `network.tf` file and the {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server and CloudInit resources are included in the `main.tf`file. For more information, about each resource and the resource configuration, see the [{{site.data.keyword.cloud_notm}} Provider plug-in documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://ibm-cloud.github.io/tf-ibm-docs/).

3. Deploy the {{site.data.keyword.cloud_notm}} resources. 
   1. Instruct Terraform to deploy the resources. Terraform parses the configuration files, creates an execution plan, and lists a summary of the resources that must be created. 
      ```
      terraform apply
      ```
      {: pre} 
   
      Example output: 
      ```
      data.template_cloudinit_config.db_userdata: Refreshing state...
      data.template_cloudinit_config.app_userdata: Refreshing state...
      An execution plan has been generated and is shown.
      Resource actions are indicated with the following symbols:
        + create
      Terraform will perform the following actions:
        + ibm_compute_ssh_key.ssh_key
 
        + ibm_compute_vm_instance.app1[0]
      ....
      Plan: 34 to add, 0 to change, 0 to destroy.
      Do you want to perform these actions?
        Terraform will perform the actions described above.
        Only 'yes' will be accepted to approve.
        Enter a value: 
      ```
      {: screen}
      
   2. Confirm the creation of the resources by entering **yes**. The creation of all resources might take a few minutes to complete. 
   
   3. Wait for the resources to be fully provisioned. 
     
      Example output: 
      ```
      Apply complete! Resources: 34 added, 0 changed, 0 destroyed.
      web_dns_name = wcpclouduk.com
      ```
      {: screen}
      
   4. In your CLI output, find and note the **web_dns_name** that is assigned to the {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer.
         
4. Verify that you can access one of the {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instances behind the {{site.data.keyword.cloud_notm}} Load Balancers that are deployed in each region. To test access via the load balancers, Terraform deploys a sample Apache web server on the {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instances by using CloudInit. This Apache web server is accessible by using the **web_dns_name** that is assigned to the {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer in the previous step.
   ```
   curl <web_dns_name>
   ```
   {: pre}
   
   Example output: 
   ```
   <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd"><html><head>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <title>Apache HTTP Server Test Page powered by CentOS</title>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
   ```
   {: screen}
   
Now that you provisioned your resources, you must make this information available to Ansible by creating an Ansible classic infrastructure inventory in Lesson 4. 
   
## Lesson 4: Creating an Ansible classic infrastructure inventory
{: #create_ansible_inventory}

Use the {{site.data.keyword.cloud_notm}} Terraform inventory script to import classic infrastructure information from your Terraform `terraform.tfstate` file into Ansible. 
{: shortdesc}
1. In your existing Ansible project directory, create a directory that is named `inventory`.
   ```
   mkdir inventory
   ```
   {: pre}
   
2. Copy the `terraform_inv.py` and the `terraform_inv.ini` files from the {{site.data.keyword.cloud_notm}} Terraform provider `examples/ansible/ibm_ansible_dyn_inv` folder into the `inventory` directory that you created. 
   ```
   mv terraform-provider-ibm/examples/ansible/ibm_ansible_dyn_inv/terraform_inv.* <ansible_project_path>/inventory/
   ```
   {: pre}
3. Create an empty `hosts` file in your `inventory` directory. 
   ```
   touch hosts
   ```
   {: pre}
      
4. Specify the location of the `terraform.tfstate` file that was created after you provisioned the {{site.data.keyword.cloud_notm}} resources with Terraform in Lesson 3. The `terraform.tfstate` file is the source for information about the deployed classic infrastructure resources that you want to target with Ansible. By specifying the location of the file in a `terraform_inv.ini` file and storing it in the same directory as the {{site.data.keyword.cloud_notm}} Terraform inventory script, Ansible can import the Terraform classic infrastructure information directly.
   1. In your Ansible `inventory` directory, open the `terraform_inv.ini` file. 
      ```
      nano terraform_inv.ini
      ```
      {: pre}
      
   2. Add the fully qualified path to the `terraform.tfstate` file that holds your Terraform classic infrastructure information.   
      ```
      [TFSTATE] 
      TFSTATE_FILE = <terraform_project_path>/terraform.tfstate
      ```
      {: codeblock}
   
5. Verify that the {{site.data.keyword.cloud_notm}} Terraform inventory script can import the Terraform classic infrastructure resources that are defined in the `terraform.tfstate` file. 
   1. Navigate to your Ansible project parent directory.  
   2. Import the Terraform classic infrastructure information from the `terraform.tfstate` file to create the Ansible classic infrastructure inventory. 
      ```
      ansible-inventory -i ./inventory --list
      ```
      {: pre}
      
You successfully imported the {{site.data.keyword.cloud_notm}} classic infrastructure information into Ansible. You can now go ahead and use Ansible to install and configure your Wordpress app in Lesson 5. 

## Lesson 5: Installing and configuring Wordpress with Ansible
{: #install_configure_wordpress}

Set up Wordpress on the Terraform-provided {{site.data.keyword.cloud_notm}} classic infrastructure by using Ansible. 
{: shortdesc}

1. Install Wordpress. The installation of Wordpress can take up to 10 minutes. 
   ``` 
   ansible-playbook -i inventory site.yml
   ```
   {: pre}
   
   Example output: 
   ```
   TASK [MySQL Change Master Status] **********************************
   changed: [db01]
   changed: [db02]
   
   PLAY RECAP *********************************************************
   app101     : ok=26   changed=15  unreachable=0   failed=0
   app102     : ok=26   changed=15  unreachable=0   failed=0
   app201     : ok=26   changed=15  unreachable=0   failed=0
   app202     : ok=26   changed=15  unreachable=0   failed=0
   control    : ok=5    changed=1   unreachable=0   failed=0
   db101      : ok=30   changed=16  unreachable=0   failed=0
   db102      : ok=30   changed=16  unreachable=0   failed=0
   ```
   {: screen}
   
   If errors occur during the Wordpress installation, you can correct the errors that are reported by Ansible and rerun the Ansible playbook again. Ansible playbooks are idempotent and can be executed multiple times. When you execute a playbook multiple times, only changes that bring the environment to the required state are executed.
   {: tip}
   
2. Open Wordpress. After the initial installation, Wordpress is not accessible via the {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer. When you try to access Wordpress after the initial installation, a `503 Service unavailable` HTTP response code is returned from the {{site.data.keyword.cloud_notm}} Load Balancers that are deployed in each region. This behavior is expected. After the installation, Wordpress force the user who administers Wordpress to set up the app by redirecting the user to the Wordpress configuration dialog with a `302 Temporary redirect` HTTP response code. The {{site.data.keyword.cloud_notm}} Load Balancers in each region do not allow to customize the list of valid HTTP response codes and do not recognize a `302` HTTP response code as a healthy response code. As a consequence, the {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer returns the `503` HTTP response code to the user.
   ```
   curl http://app101 -vS
   ```
   {: pre}
   
   Example output: 
   ```
   * Rebuild URL to: app101/
   *   Trying 10.72.58.86
   * TCP_NODELAY set
   * Connected to app101 (10.72.58.86) port 80 (#0)
   > GET / HTTP/1.1
   > Host: app101
   > User-Agent: curl/7.54.0
   > Accept: */*
   >
   < HTTP/1.1 302 Found
   < Date: Fri, 02 Nov 2018 15:53:02 GMT
   < Server: Apache
   < X-Powered-By: PHP/5.4.16
   < Expires: Wed, 11 Jan 2019 05:00:00 GMT
   < Cache-Control: no-cache, must-revalidate, max-age=0
   < Location: http://app101/wp-admin/install.php
   < Content-Length: 0
   < Content-Type: test/html; charset=UTF-8
   <
   * Connection #0 to host app101 is intact
   ```
   {: screen}
    
3. Complete the Wordpress setup dialog by using Ansible and the Wordpress CLI. During the setup, you provide the user details of the Wordpress admin that you want to use and configure the Wordpress database. 

   If you prefer to manually set up Wordpress, you can access the Wordpress site via the {{site.data.keyword.cloud_notm}} internal address `http://app101` from a web browser and complete the initial setup dialog. After you complete the setup, log in as the Wordpress admin user. Go to **Settings** and enter the **web_dns_name** of the {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer in the **Wordpress Address (URL)** and **Site Address (URL)** fields in the format `http://<web_dns_name>`. Then, click **Save**. 
   {: tip}
   
   1. Open the `wp_site_setup.yaml` file. 
      ```
      nano wp_site_setup.yaml
      ```
      {: pre}
   
   2. Enter the user name, password, and email address of the Wordpress admin user that you want to use. You can also use the default values that are already provided in the file.   
      ```
      ...
        site_admin: "<username>"
        site_email: "<email>"
        site_password: "<password>"
      ...
      ```
      {: codeblock}
      
   3. Run the `wp_site_setup.yaml` Ansible playbook to complete the initial setup dialog by using the Wordpress CLI. During the setup, Ansible automatically retrieves the **web_dns_name** of the {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer by using the Terraform inventory integration and uses the domain name to configure the Wordpress site. 
      ```
      ansible-playbook -i inventory wp_site_setup.yml
      ```
      {: pre}
      
      Example output: 
      ```
      TASK [debug] ****************************************************************
      ok: [app102] => {
           "msg": [
               "Wordpress URL is wcpclouduk.com",
               "Wordpress admin user is: admin and password is: strong@password
             ]
          }
      PLAY RECAP ******************************************************************
      app102    : ok=5  changed=2   unreachable=0   failed=0
      ```
      {: screen}
   
4. Access your Wordpress site from your preferred browser via the **web_dns_address** of the {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer. The **web_dns_address** is returned as the `Wordpress URL` in the CLI output of the previous step. 
   ```
   http://<web-dns-address>
   ```
   {: codeblock}
   
5. Check the status of your website. 
   ```
   ansible-playbook -i inventory stack_status.yml
   ```
   {: pre}
6. Optional: Restart your website. 
   ```
   ansible-playbook -i inventory stack_restart.yml
   ```
   {: pre}   

With your Wordpress app up and running, explore the failover capabilities of your highly available Wordpress architecture in {{site.data.keyword.cloud_notm}}. 

## Lesson 6: Explore the high availability features of {{site.data.keyword.cloud_notm}} 
{: #verify_resilience}

With your Wordpress app up and running, you can now experiment with what happens if one or more {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instances become unavailable. 
{: shortdesc}

1. Shut down one {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instance where Wordpress is deployed and observe the status change of the  {{site.data.keyword.cloud_notm}} Load Balancers and the availability of the web site. 
   1. Open the [classic infrastructure console](https://cloud.ibm.com/classic). 
   2. Select **Devices** > **Device List**. A list of your {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instances is shown. 
   3. Find the `app101` {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instance and select **Power On/Off** from the **Actions** menu.
    
2. Check the health of your {{site.data.keyword.cloud_notm}} Load Balancers.
   1. Open the [status page](https://cloud.ibm.com/classic/network/loadbalancing/cloud) of your {{site.data.keyword.cloud_notm}} Load Balancers and verify that both load balancers are listed as `online`. 
   2. Select the {{site.data.keyword.cloud_notm}} Load Balancer that is deployed in the same region as the `app101` {{site.data.keyword.cloud_notm}} Virtual Server that you shut down. The **Server Status** shows `1/2 Healthy` in the **Health Status**, which reflects that one of your {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instances is available, and the other one is not.  
    
3. Check the health of your {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer.   
   1. From the [{{site.data.keyword.cloud_notm}} resource list](https://cloud.ibm.com/resources), select your **Internet Services** instance. 
   2. Select **Reliability** > **Global Load Balancers** to navigate to the status page for your {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer. 
   3. Verify that your {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer shows a `Healthy` health status. 
   
4. Access Wordpress with your preferred web browser and verify that your web site is available and shows the splash screen. 
   ```
   http://<web-dns-address>
   ```
   {: codeblock}
   
5. Shut down a second {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instance where Wordpress is deployed and observe the status change of the {{site.data.keyword.cloud_notm}} Load Balancers and the availability of the web site.
   1. From the [classic infrastructure console](https://cloud.ibm.com/classic), select **Devices** > **Device List**. A list of your {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instances is shown. 
   3. Find the `app102` {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instance and select **Power On/Off** from the **Actions** menu.

6. Check the health of your {{site.data.keyword.cloud_notm}} Load Balancers.
   1. Open the [status page](https://cloud.ibm.com/classic/network/loadbalancing/cloud) of your {{site.data.keyword.cloud_notm}} Load Balancers and select the {{site.data.keyword.cloud_notm}} Load Balancer that is deployed in the same region as the `app101` and `app102` {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Servers that you shut down. 
   2. Verify that the **Server Status** shows `0/2 Healthy` in the **Health Status**, which reflects that all of your {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Servers are unavailable. 
      
7. Check the health of your {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer.   
   1. From the [{{site.data.keyword.cloud_notm}} resource list](https://cloud.ibm.com/resources), select your **Internet Services** instance. 
   2. Select **Reliability** > **Global Load Balancers** to navigate to the status page for your {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer. 
   3. Verify that your {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer shows a `Degraded` health status. The `Degraded` status is displayed because only the origin pool in the second data center is still in a healthy state. 
   
8. Access Wordpress with your preferred web browser and verify that your web site is still available and shows the splash screen. 
   ```
   http://<web-dns-address>
   ```
   {: codeblock}  

9. Shut down the {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instance in the second data center where MariaDB is deployed and observe the status change of the {{site.data.keyword.cloud_notm}} Load Balancers and the availability of the web site. 
   1. From the [classic infrastructure console](https://cloud.ibm.com/classic), select **Devices** > **Device List**. A list of your {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instances is shown. 
   3. Find the `db201` {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instance and select **Power On/Off** from the **Actions** menu.

10. Check the health of your {{site.data.keyword.cloud_notm}} Load Balancers.
    1. Open the [status page](https://cloud.ibm.com/classic/network/loadbalancing/cloud) of your {{site.data.keyword.cloud_notm}} Load Balancers and open one of the {{site.data.keyword.cloud_notm}} Load Balancers. 
    2. Verify that the **Server Status** shows `0/2 Healthy` in the **Health Status**, which reflects that all of your {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Servers are unavailable. 
    3. Open the other {{site.data.keyword.cloud_notm}} Load Balancers that is deployed in the other data center. 
    4. Verify that the **Server Status** also shows `0/2 Healthy` in the **Health Status**. When the MariaDB database becomes unavailable, all Wordpress app instances become unavailable immediately. 
      
11. Check the health of your {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer.   
    1. From the [{{site.data.keyword.cloud_notm}} resource list](https://cloud.ibm.com/resources), select your **Internet Services** instance. 
    2. Select **Reliability** > **Global Load Balancers** to navigate to the status page for your {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer. 
    3. Verify that your {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer shows a `Critical` health status. The `Critical` status is displayed because all Wordpress app instances are unavailable.  

12. Access Wordpress with your preferred web browser and verify that your web site is not accessible anymore. Your web browser displays a `503 Service Unavailable` HTTP response because both Wordpress web servers in one data center are down and the MariaDB database in the other data center is down.   
    ```
    http://<web-dns-address>
    ```
    {: codeblock}  
    
13. Restart your {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instances. 
    1. From the [classic infrastructure console](https://cloud.ibm.com/classic), select **Devices** > **Device List**. A list of your {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instances is shown. 
   3. Find the `db201`, `app101`, and `app102` {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Server instances and select **Power On/Off** from the **Actions** menu to power on your instances again. You can choose any order to power on your instances. 
   
14. Check the health of your {{site.data.keyword.cloud_notm}} Load Balancers.
    1. Open the [status page](https://cloud.ibm.com/classic/network/loadbalancing/cloud) of your {{site.data.keyword.cloud_notm}} Load Balancers and open one of the {{site.data.keyword.cloud_notm}} Load Balancers. 
    2. Verify that the **Server Status** shows `2/2 Healthy` in the **Health Status**, which reflects that all of your {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Servers in this data center are available. 
    3. Open the other {{site.data.keyword.cloud_notm}} Load Balancers that is deployed in the other data center. 
    4. Verify that the **Server Status** also shows `2/2 Healthy` in the **Health Status**, which reflects that all of your {{site.data.keyword.cloud_notm}} classic infrastructure Virtual Servers in the second data center are available. 
    
15. Check the health of your {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer.   
    1. From the [{{site.data.keyword.cloud_notm}} resource list](https://cloud.ibm.com/resources), select your **Internet Services** instance. 
    2. Select **Reliability** > **Global Load Balancers** to navigate to the status page for your {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer. 
    3. Verify that your {{site.data.keyword.cloud_notm}} Internet Services Global Load Balancer shows a `Healthy` health status. It might take a few minutes to show a `Healthy` status because of the policies that are defined in the health check. 
    
16. Access Wordpress with your preferred web browser and verify that your web site is available again and shows the splash screen. 
    ```
    http://<web-dns-address>
    ```
    {: codeblock}  

    
Great! You successfully installed and configured a highly available instance of Wordpress by using Ansible and Terraform. Now you can start designing and deploying resilient and scalable web sites with {{site.data.keyword.cloud_notm}}. Continue to explore Wordpress by reviewing the [First steps with Wordpress tutorial ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://codex.wordpress.org/First_Steps_With_WordPress). You can also research how to deploy any of the other widely available open source web site solutions on {{site.data.keyword.cloud_notm}}.
