---

copyright:
  years: 2017, 2020
lastupdated: "2020-10-09"

keywords: terraform provider deployment, automation, schematics workspace, ibm cloud terraform provider deployment, schematics workspace creation

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


# Automating the deployment for IBM cloud terraform provider in Schematics

## Auto deploy example
{: #auto-deploy-example}

Click [View GitHub repo](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples){: external} hyperlink to access the public Git repository examples of an {{site.data.keyword.cloud_notm}} Terraform provider.

Click [Deploy to IBM Cloud Schematics](https://cloud.ibm.com/schematics/workspaces/create?repository=https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-api-gateway&terraform_version=terraform_v0.12){: external} hyperlink to deploy an example in the Schematics to create a workspace automatically.

To automate the deployment into IBM Cloud Schematics, you can copy, paste the following snippet template URL in the browser URL.

```
https://cloud.ibm.com/schematics/workspaces/create?repository=https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-api-gateway&terraform_version=terraform_v0.12
```
{: pre}

The URL must contains 2 parameters, first parameter has the name of the workspace and second parameter has the Terraform version. If you do not provide the two parameters, the `Deploy to IBM Cloud Schematics` link defaults to the repository's master branch.
{: tip}