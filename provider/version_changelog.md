---

copyright:
  years: 2014, 2020
lastupdated: "2020-02-25"

keywords: terraform, terraform provider release, terraform provider versions
subcollection: containers

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
{:external: target="_blank" .external}

# Version changelog
{: #provider-changelog}

View information for updates to the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform.
{:shortdesc}

## Changelog for 1.2.2, released 26 February 2020

The following table shows the changes that are included in version 1.2.1 of the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform.
{: shortdesc}

| Previous | Current | Description |
| -------- | ------- | ----------- |
| 1.2.1 | 1.2.2 | <ul><li><code>[ibm_container_cluster_config](/docs/terraform?topic=terraform-container-data-sources#container-cluster-config)</code>: Added new examples to download TLS certificates and permission files for the cluster administrator for classic and VPC {{site.data.keyword.containerlong_notm}} clusters, and classic {{site.data.keyword.openshiftlong_notm}} clusters. </li><li><code>[ibm_container_cluster](/docs/terraform?topic=terraform-container-resources#container-cluster)</code>: Deprecated `billing` and `is_trusted` input parameter, and added the `gateway_enabled` input parameter. </li><li><code>ibm_is_network_acl</code> for [Gen 1](/docs/terraform?topic=terraform-vpc-gen1-resources#network-acl) and [Gen 2](/docs/terraform?topic=terraform-vpc-gen2-resources#network-acl): Updated the `rules` input parameter description. </li><li><code>ibm_is_floating_ip</code> for [Gen 1](/docs/terraform?topic=terraform-vpc-gen1-resources#provider-floating-ip) and [Gen 2](/docs/terraform?topic=terraform-vpc-gen2-resources#provider-floating-ip): Added the `tags` input parameter.</li><li><code>ibm_is_public_gateway</code> for [Gen 1](/docs/terraform?topic=terraform-vpc-gen1-resources#provider-public-gateway) and [Gen 2](/docs/terraform?topic=terraform-vpc-gen2-resources#provider-public-gateway): Added the `tags` and `resource_group` input parameters. </li><li><code>[ibm_resource_key](/docs/terraform?topic=terraform-resource-mgmt-resources#resource-key)</code>: Added an example to create credentials for an IAM-enabled service by using a service ID. </li></ul>|
{: caption="Terraform provider 0.12.2" caption-side="top"}


## Changelog for 1.2.1, released 13 February 2020
{: #121}

The following table shows the changes that are included in version 1.2.1 of the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform.
{: shortdesc}

| Previous | Current | Description |
| -------- | ------- | ----------- |
| 1.2.0 | 1.2.1 | [See the release notes for the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform version 1.2.1](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v1.2.1){: external}.|
{: caption="Terraform provider 0.12.1" caption-side="top"}
