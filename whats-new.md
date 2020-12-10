---

copyright:
  years: 2019, 2020
lastupdated: "2020-12-10"

keywords: terraform resources, terraform modules, terraform provider, terraform autodeploy, 

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

# What's new in Terraform?
{: #new-in-terraform}

Learn about the latest changes to the Terraform service that are grouped by month.
<staging>

## December 2020
{: #december-2020}

<table>
    <thead>
    <th style="width:80px">New resources</th>
    <th style="width:80px">New data sources</th>
    <th style="width:500px">Enhancements</th>
    </thead>
  <tbody>
    <tr>
 <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px">[ibm_cis_routing](/docs/terraform?topic=terraform-cis-resources#cis-routing)</li><li style="margin:0px; padding:0px">[ibm_cis_cache_settings](/docs/terraform?topic=terraform-cis-resources#cis-cache)<li style="margin:0px; padding:0px">[ibm_cis_custom_page](/docs/terraform?topic=terraform-cis-resources#cis-custom)<li style="margin:0px; padding:0px">[ibm_dns_glb](/docs/terraform?topic=terraform-dns-resources#dns-glb)<li style="margin:0px; padding:0px">[ibm_dns_glb_monitor](/docs/terraform?topic=terraform-dns-resources#dns-glb-monitor)<li style="margin:0px; padding:0px">[ibm_dns_glb_pool](/docs/terraform?topic=terraform-dns-resources#dns-glb-pool)<li style="margin:0px; padding:0px">[ibm_is_vpc_routing_table](/docs/terraform?topic=terraform-vpc-gen2-resources#vpc-routing-table)<li style="margin:0px; padding:0px">[ibm_is_vpc_routing_table_route](/docs/terraform?topic=terraform-vpc-gen2-resources#vpc-routing-table-route)</li></ul></td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px">[ibm_cis_global_load_balancers](/docs/terraform?topic=terraform-cis_data#cis-global-lb-ds)</li><li style="margin:0px; padding:0px">[ibm_cis_custom_pages](/docs/terraform?topic=terraform-cis-resources#cis-custom)</li><li style="margin:0px; padding:0px">[ibm_dns_glbs](/docs/terraform?topic=terraform-dns-resources#dns-glbs-ds)</li><li style="margin:0px; padding:0px">[ibm_dns_glb_monitors](/docs/terraform?topic=terraform-dns-resources#dns-glb-monitors-ds)</li><li style="margin:0px; padding:0px">[ibm_dns_glb_pools](/docs/terraform?topic=terraform-dns-resources#dns-glb-pools-ds)</li><li style="margin:0px; padding:0px">[ibm_is_vpc_default_routing_table](/docs/terraform?topic=terraform-vpc-gen2-data-sources#vpc-default-routing-tableds)</li><li style="margin:0px; padding:0px">[ibm_is_vpc_routing_tables](/docs/terraform?topic=terraform-vpc-gen2-data-sources#vpc-routing-tableds)</li><li style="margin:0px; padding:0px">[ibm_is_vpc_routing_table_routes](/docs/terraform?topic=terraform-vpc-gen2-data-sources#vpc-routing-tablesds)</li></ul></td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px">public_ip attribute for [power network resource](/docs/terraform?topic=terraform-power-vsi#power-network)</li><li style="margin:0px; padding:0px">Policies attribute for [ibm_kms_key resource](/docs/terraform?topic=terraform-kms-resources#kms-key)</li><li style="margin:0px; padding:0px">Number of invited users and invited users attribute for [ibm_iam_user_invite resource](/docs/terraform?topic=terraform-iam-resources#iam-user-invite#iam-user-invite-output)</li><li style="margin:0px; padding:0px">HTTPS and Enum attribute for [ibm_is_lb_pool vpc generation2 resource](/docs/terraform?topic=terraform-vpc-gen2-resources#lb-pool)</li><li style="margin:0px; padding:0px">Encrypted data key and encryption key argument for [image data source](/docs/terraform?topic=terraform-vpc-gen2-data-sources#image)</li><li style="margin:0px; padding:0px"> Archive rule argument for [COS bucket resource](https://cloud.ibm.com/docs/terraform?topic=terraform-object-storage-resources#cos-bucket)</li><li style="margin:0px; padding:0px">Policies for [ibm_kms_key and ibm_kms_keys data source](https://cloud.ibm.com/docs/terraform?topic=terraform-kms-data-sources)</li><li style="margin:0px; padding:0px">List bounded services argument for [ibm_container_cluster data source](/docs/terraform?topic=terraform-container-data-sources#container-cluster)</li><li style="margin:0px; padding:0px">Access rule and au rule argument for [internet service firewall resource and datasource](/docs/terraform?topic=terraform-cis_data#cis-firewall-dsoutput)</li><li style="margin:0px; padding:0px">Allow ip spoofing argument for [ibm_is_instance](/docs/terraform?topic=terraform-vpc-gen2-resources#instance-input)</li><li style="margin:0px; padding:0px">Routing table and ip version argument for [ibm_is_subnet](/docs/terraform?topic=terraform-vpc-gen2-resources#subnet-input)</li><li style="margin:0px; padding:0px">Import for [floating IP resources](/docs/terraform?topic=terraform-vpc-gen2-resources#provider-floating-ip#floating-ip-import)</li><li style="margin:0px; padding:0px">Import for [Ike policy resources](/docs/terraform?topic=terraform-vpc-gen2-resources#provider-ike-policy#provider-ike-policy-import)</li><li style="margin:0px; padding:0px">Import for [images resources](/docs/terraform?topic=terraform-vpc-gen2-resources#image#image-import)</li><li style="margin:0px; padding:0px">Import for [volume resources](/docs/terraform?topic=terraform-vpc-gen2-resources#volume#volume-import)</li></ul></td>
    </tr>
  </tbody>
  </table> 