---

copyright:
  years: 2017, 2020
lastupdated: "2020-09-07"

keywords: terraform identity and access, terraform iam, terraform permissions, terraform iam policy

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


# Terraform `provider` block configuration
{: #provider-reference}

Review what credentials and information you need to provide to work with {{site.data.keyword.cloud_notm}} resources and data sources with the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform.
{: shortdesc}

## Required input parameters for each resource category
{: #required-parameters}

Review what information you must provide in the `provider` block to work with a specific resource category. The values in this table are required values. To retrieve the values or view additional parameters that you can specify, see the [Supported input parameters](#provider-parameter-ov). 
{: shortdesc}

|Resource|Required input parameters|
|-------------|---------------------|
|Classic infrastructure|<ul><li><code>iaas_classic_username</code>: The user name to access classic {{site.data.keyword.cloud_notm}} infrastructure.</li><li><code>iaas_classic_api_key</code>: The API key to access classic {{site.data.keyword.cloud_notm}} infrastructure.</li><li><code>region</code>: The {{site.data.keyword.cloud_notm}} region where you want to create classic infrastructure resources.</li><li><code>ibmcloud_api_key</code>: The {{site.data.keyword.cloud_notm}} API key to authenticate with the {{site.data.keyword.cloud_notm}} platform.</li></ul>|
|Cloud Foundry and all other IAM-enabled services|<ul><li><code>region</code>: The {{site.data.keyword.cloud_notm}} region where you want to create Cloud Foundry or IAM services.</li><li><code>ibmcloud_api_key</code>: The {{site.data.keyword.cloud_notm}} API key to authenticate with the {{site.data.keyword.cloud_notm}} platform.</li></ul>|
|Functions|<ul><li><code>function_namespace</code>: The namespace in {{site.data.keyword.openwhisk}} where you want to create your resources.</li><li><code>ibmcloud_api_key</code>: The {{site.data.keyword.cloud_notm}} API key to authenticate with the {{site.data.keyword.cloud_notm}} platform.</li></ul>
|Kubernetes Service|<ul><li><code>ibmcloud_api_key</code>: The {{site.data.keyword.cloud_notm}} API key to authenticate with the {{site.data.keyword.cloud_notm}} platform.</li><li><code>generation</code>: If you want to create a VPC cluster, specify the generation of {{site.data.keyword.cloud_notm}} VPC infrastructure.</li></ul>|
|Power Systems|<ul><li><code>zone</code>: The {{site.data.keyword.cloud_notm}} zone where you want to create Power System resources. This value is required only when you want to work with a resource in a multizone-capable region.</li><li><code>region</code>: The {{site.data.keyword.cloud_notm}} region where you want to create Power System resources.</li><li><code>ibmcloud_api_key</code>: The {{site.data.keyword.cloud_notm}} API key to authenticate with the {{site.data.keyword.cloud_notm}} platform.</li></ul>|
|VPC infrastructure for Gen 1 and Gen 2 compute|<ul><li><code>generation</code>: The generation of {{site.data.keyword.cloud_notm}} VPC infrastructure.</li><li><code>region</code>: The {{site.data.keyword.cloud_notm}} region where you want to create VPC resources.</li><li><code>ibmcloud_api_key</code>: The {{site.data.keyword.cloud_notm}} API key to authenticate with the {{site.data.keyword.cloud_notm}} platform.</li></ul>|


## Supported input parameters
{: #provider-parameter-ov}

Review what parameters you can set in the `provider` block of your Terraform configuration file.
{: shortdesc}

|Input parameter|Required/ optional|Description|
|-------------|--------|-----------------------|
|`iaas_classic_api_key`|Required for classic infrastructure|The API key to access classic {{site.data.keyword.cloud_notm}} infrastructure. For more information about how to retrieve your API key, see [Managing classic infrastructure API keys](/docs/account?topic=account-classic_keys). This value is required when you want to work with classic infrastructure resources. You can specify the API key in the `provider` block or retrieve the value from the `IAAS_CLASSIC_API_KEY` environment variable.|
|`iaas_classic_username`|Required for classic infrastructure|The user name to access classic {{site.data.keyword.cloud_notm}} infrastructure. For more information about how to retrieve your user name, see [Managing classic infrastructure API keys](/docs/account?topic=account-classic_keys). This value is required when you want to work with classic infrastructure resources. You can specify the user name in the `provider` block or retrieve the value from the `IAAS_CLASSIC_USERNAME` environment variable.|
|`iaas_classic_endpoint_url`|Optional|The API endpoint that you want to use to access {{site.data.keyword.cloud_notm}} classic infrastructure. If this value is not specified, `https://api.softlayer.com/rest/v3` is used by default. You can specify the URL in the `provider` block or retrieve the value from the `IAAS_CLASSIC_ENDPOINT_URL`. |
|`iaas_classic_timeout`|Optional|The number of seconds to wait until the classic infrastructure API is considered unavailable. The default values is `60`. You can specify this information in the `provider` block or retrieve it from the `IAAS_CLASSIC_TIMEOUT` environment variable.|
|`ibmcloud_api_key`|Required|The {{site.data.keyword.cloud_notm}} API key to authenticate with the {{site.data.keyword.cloud_notm}} platform. For more information about how to create an API key, see [Creating an API key](/docs/account?topic=account-userapikey#create_user_key). You can specify the API key in the `provider` block or retrieve the value from the `IC_API_KEY` or `IBMCLOUD_API_KEY` environment variables. If both environment variables are defined, `IC_API_KEY` takes precedence.|
|`ibmcloud_timeout`|Optional|The number of seconds that you want to wait until the {{site.data.keyword.cloud_notm}} API is considered unavailable. The default value is `60`. You can specify the timeout in the `provider` block or retrieve the value from the `IC_TIMEOUT` or `IBMCLOUD_TIMEOUT` environment variables. If both variables are specified, `IC_TIMEOUT` takes precedence.|
|`function_namespace`|Required for Functions|The {{site.data.keyword.openwhisk}} namespace that you want to use. The namespace is composed from your Cloud Foundry organization and space in the format `<org>_<space>`. You can specify the namespace in your `provider` block or retrieve the value from the `FUNCTION_NAMESPACE` environment variable. 
|`generation`|Required for VPC infrastructure|The generation of Virtual Private Cloud infrastructure that you want to use. Supported values are `1` for VPC generation 1, and `2` for VPC generation 2 infrastructure. If this value is not specified, `2` is used by default. You can specify the generation in your `provider` block or retrieve the value from the `IC_GENERATION` or `IBMCLOUD_GENERATION` environment variables. If both environment variables are defined, `IC_GENERATION` takes precedence.|
|`max_retries`|Optional|The maximum number of times that a request to the {{site.data.keyword.cloud_notm}} infrastructure API is sent before the request is considered failed. The default value is `10`. Use this parameter when you receive network-related timeouts or rate limit exceeded error codes. You can specify the number in your `provider` block or retrieve the value from the `MAX_RETRIES` environment variable.|
|`region`|Optional|The {{site.data.keyword.cloud_notm}} region where you want to create your resources. If this value is not specified, `us-south` is used by default. You can specify the region in the `provider` block or retrieve the value from the `IBMCLOUD_REGION` or `IC_REGION` environment variables. If both environment variables are specified, `IC_REGION` takes precedence.|
|`resource_group`|Optional|The ID of the resource group that you want to use for your {{site.data.keyword.cloud_notm}} resources. To retrieve the ID, run `ibmcloud resource groups`. You can specify the resource group in the `provider` block or retrieve the value from the `IC_RESOURCE_GROUP` or `IBMCLOUD_RESOURCE_GROUP` environment variables. If both environment variables are defined, `IC_RESOURCE_GROUP` takes precedence. |
|`zone`|Required for Power Systems|The zone of an {{site.data.keyword.cloud_notm}} region where you want to create Power System resources. This value is required if you want to work with resources in a multizone-capable region. For example, if you want to work in the `eu-de` region, you must enter `eu-de-1` or `eu-de-2`. You can specify the zone in the `provider` block or retrieve the value from the `IC_ZONE` or `IBMCLOUD_ZONE` environment variables. If both environment variables are specified, `IC_ZONE` takes precedence.| 


## Example usage
{: #provider-example}

You can choose if you want to provide the input parameters as static values in the `provider` block or if you want to retrieve the values from Terraform variables or environment variables that you set. 
{: shortdesc}

### Static values
{: #static}

You can provide the values for your provider input parameters as static values in the `provider` block of your Terraform configuration file. 
{: shortdesc}

```
provider "ibm" {
    ibmcloud_api_key = "<api_key>"
    iaas_classic_username = "<classic_username>"
    iaas_classic_api_key = "<classic_api_key>"
}
```

### Terraform variables file
{: #tf-variables}

You can retrieve the values for the provider input parameters from a Terraform variables file (`terraform.tfvars`) that you created on your local machine.
{: shortdesc}

1. Create the Terraform variables file `terraform.tfvars` on your local machine. 
   ```
   ibmcloud_api_key = "<ibmcloud_api_key>"
   iaas_classic_username = "<classic_infrastructure_username>"
   iaas_classic_api_key = "<classic_infrasturcture_apikey>"
   ```
   {: codeblock}
   
2. Use Terraform interpolation syntax to reference the variables in the `provider` block of your Terraform configuration file. 
   ```
   provider "ibm" {
     ibmcloud_api_key    = var.ibmcloud_api_key
     iaas_classic_username = var.iaas_classic_username
     iaas_classic_api_key  = var.iaas_classic_api_key
   }
   ```
   {: codeblocK}

### Environment variables
{: #env-vars}

You can retrieve the values for the provider input parameters from environment variables that you set on your local machine. Make sure that you use the [correct name for an environment variable](#provider-parameter-ov) so that Terraform can automatically read these when you run a `terraform apply`, `plan`, or `destroy` action. For example, to specify a classic infrastructure user name, use `IAAS_CLASSIC_USERNAME`. 

1. Add an empty `provider` block to your Terraform configuration file.
   ```
   provider "ibm" {}
   ```
   {: codeblock}

2. Set the environment variables on your local machine. 
   ```
   export IC_API_KEY="<ibmcloud_api_key>"
   export IAAS_CLASSIC_USERNAME="<classic_username>"
   export IAAS_CLASSIC_API_KEY="<classic_api_key>"
   ```
   {: codeblock}
   
3. Create a Terraform execution plan. 
   ```
   terraform plan
   ```
   {: pre} 
   

## Creating multiple `provider` configurations
{: #multiple-providers}

You can add multiple `provider` configurations within the same Terraform configuration file to create your {{site.data.keyword.cloud_notm}} resources with different provider parameters. 
{: shortdesc}

Creating multiple `provider` configurations is useful when you want to use different input parameters, such as different regions, zones, infrastructure generations, or accounts to create the {{site.data.keyword.cloud_notm}} resources in your Terraform configuration file. For more information, see [Multiple Provider Instances](https://www.terraform.io/docs/configuration/providers.html#alias-multiple-provider-instances){: external}. 

1. In your Terraform configuration or `provider.tf` file, create multiple provider blocks with the same provider name. The provider configuration without an alias is considered the default provider configuration and is used for every resource where you do not specify a specific provider configuration. Any additional provider configurations must include an alias so that you can reference this provider from your resource definition.
   ```
   provider "ibm" {
     ibmcloud_api_key    = var.ibmcloud_api_key
     region = "us-south"
   }
   
   provider "ibm" {
     alias = "east"
     ibmcloud_api_key    = var.ibmcloud_api_key
     region = "us-east"
   }
   ```
   {: codeblock}
   
2. In your resource definition, specify the provider configuration that you want to use. If you do not specify a provider, the default provider configuration is used.
   ```
   resource "ibm_container_cluster" "cluster" {
     provider = ibm.east
   ...
   }
   ```
   {: codeblock}
   
   
