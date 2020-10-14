---

copyright:
  years: 2017, 2020
lastupdated: "2020-10-14"

keywords: terraform provider deployment, automation, schematics workspace, ibm cloud terraform provider deployment, schematics workspace creation, autodeploy 

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


# Automating the deployment for {{site.data.keyword.cloud_notm}} Terraform provider template in Schematics
{: #autodeploy_schematics_workspace}

To automate the deployment of {{site.data.keyword.cloud_notm}} Terraform v0.12 provider template example in {{site.data.keyword.bplong_notm}}, you need to follow the steps:

1. Create a template example using {{site.data.keyword.cloud_notm}} Terraform provider and publish in the public Git repository. To create example, refer [Sample template example](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples){: external}.
2. Copy the public Git repository URL, for example, `https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-api-gateway`.
3. Use this syntax to autodeploy the Schematics workspace creation in the {{site.data.keyword.cloud_notm}}.

  **Syntax**

  ```
  https://cloud.ibm.com/schematics/workspaces/create?repository=<template public Git repository example url>&terraform_version=<terraform_v0.xx>
  ```
  {: pre}

  **Example**

  ```
  https://cloud.ibm.com/schematics/workspaces/create?repository=https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-api-gateway&terraform_version=terraform_v0.12
  ```
  {: pre}

  The URL contains 2 parameters, first parameter has the name of the workspace and second parameter has the Terraform version. If you do not provide any parameters of ignore one parameter, the `Deploy to {{site.data.keyword.cloud_notm}}` link defaults to the repository's master branch. You can provide the terraform version parameter as per the Terraform version that you are using.
  {: important}

4. You can copy, and paste the example URL in the browser to view the {{site.data.keyword.cloud_notm}} Schematics workspace UI with the create button is display.
5. Cross check the parameters in the workspace UI and click `Create` button.

## Adding an image on deploy to {{site.data.keyword.cloud_notm}} hyperlink
{: #add_an_image}

You can add an image on `Deploy to {{site.data.keyword.cloud_notm}} Schematics` text by using the following syntax and example.

**Syntax**
```
<a href="https://cloud.ibm.com/schematics/workspaces/create?repository=<public Git repository example URL>/<workspace name>&terraform_version=terraform_xx">Deploy to {{site.data.keyword.bplong_notm}} <img src=<image location>></a>
```
{: pre}

**Sample example**

```
<a href="https://test.cloud.ibm.com/schematics/workspaces/create?repository=https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-api-gateway&terraform_version=terraform_v0.12">Deploy to {{site.data.keyword.cloud_notm}}<img src="/images/autodeploy.png"></a>
```
{: pre}

**Sample output**

<img src="/images/deploytoschematics.png" alt="Deploy to {{site.data.keyword.cloud_notm}}" width="200" style="width: 200px; border-style: none"/>

To view about the sample Terraform template examples, refer [Sample Terraform templates and deploy to {{site.data.keyword.bplong_notm}}](/docs/terraform?topic=terraform-sample_terraformtemplates#api-gwy-template).

