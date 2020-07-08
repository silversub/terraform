
 <tr>
    <td><code>ibm-network-vlan</code></td>
      <td>Create a classic virtual server instance with a public and a private VLAN that you can access by using an SSH key. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_network_vlan</code></li><li style="margin:0px; padding:0px"><code>ibm_compute_ssh_key</code></li><li style="margin:0px; padding:0px"><code>ibm_compute_vm_instance</code></li></ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-network-vlan)</td>
  </tr>
<tr>
    <td><code>ibm-openshift-job</code></td>
      <td>Create and execute a secured shell script in a {{site.data.keyword.openshiftlong_notm}} cluster by using a Terraform template. This template creates a Kubernetes config map that includes a reference to a shell script. Then, you create a pod that mounts the config map as a volume and executes the shell script. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>kubernetes_secret</code></li><li style="margin:0px; padding:0px"><code>kubernetes_config_map</code></li><li style="margin:0px; padding:0px"><code>kubernetes_job" "demo412</code></li></ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-openshift-job)</td>
  </tr>
  
 <tr>
    <td><code>ibm-power</code></td>
      <td>Create a {{site.data.keyword.powersys_notm}} instance with a public and a private network that mounts the system volumes. You can also create an SSH key to access the instance. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_pi_key</code></li><li style="margin:0px; padding:0px"><code>ibm_pi_network</code></li><li style="margin:0px; padding:0px"><code>ibm_pi_volume</code></li>
	  <li style="margin:0px; padding:0px"><code>ibm_pi_instance</code></li></ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-power)</td>
  </tr>
  
 <tr>
    <td><code>ibm-private-dns</code></td>
      <td>Create an {{site.data.keyword.vpc_short}} and an {{site.data.keyword.dns_full_notm}} instance, and add the VPC as a permitted network to the DNS service instance. Then, you create different types of DNS records. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_is_vpc</code></li><li style="margin:0px; padding:0px"><code>ibm_resource_instance</code></li><li style="margin:0px; padding:0px"><code>ibm_dns_zone</code></li>
	  <li style="margin:0px; padding:0px"><code>ibm_dns_resource_record</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-private-dns)</td>
  </tr>
  
<tr>
    <td><code>ibm-resource-instance</code></td>
      <td>Create an {{site.data.keyword.cos_full_notm}} service instance with HMAC credentials, and configure custom timeouts for creating, updating, or deleting the instance. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm-resource-instance</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-resource-instance)</td>
  </tr>
  
 <tr>
    <td><code>ibm-schematics</code></td>
      <td>Retrieve the Terraform state file and output variables for a {{site.data.keyword.bpshort}} workspace by using a {{site.data.keyword.bpshort}} data source. For more information about how to use the data source, see [Managing cross-workspace state access with Terraform](/docs/schematics?topic=schematics-remote-state). </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>uses only the data</code></li><li style="margin:0px; padding:0px"><code>test</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-schematics)</td>
  </tr>
  
 
<tr>
    <td><code>ibm-storage-cos</code></td>
      <td>Set up Helm in an {{site.data.keyword.containerlong_notm}} cluster to install the {{site.data.keyword.cos_full_notm}} Helm plug-in. Then, you create an {{site.data.keyword.cos_full_notm}} service instance where you can store data from the apps in your cluster. You also learn how to create a Kubernetes persistent volume claim (PVC) to create a bucket in your {{site.data.keyword.cos_full_notm}} instance and how to deploy an app in the cluster that mounts your {{site.data.keyword.cos_full_notm}} bucket. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_resource_instance</code></li><li style="margin:0px; padding:0px"><code>ibm_container_bind_service</code></li><li style="margin:0px; padding:0px"><code>kubernetes_secret</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-storage-cos)</td>
  </tr>
  
 <tr>
    <td><code>ibm-transit-gateway</code></td>
      <td>Create an {{site.data.keyword.tg_full_notm}} service instance. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_tg_gateway</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-transit-gateway)</td>
  </tr>
  
<tr>
    <td><code>ibm-vsi-cloudinit</code></td>
      <td>Create a classic virtual server instance that is configured with an SSH key and connected to a public and private VLAN, and use CloudInit to install an Apache (httpd) web server on the instance. Your SSH key is used to create a VPN connection to your virtual server instance to start the Apache web server. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_network_vlan</code></li><li style="margin:0px; padding:0px"><code>ibm_compute_ssh_key</code></li><li style="margin:0px; padding:0px"><code>ibm_compute_vm_instance</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-vsi-cloudinit)</td>
  </tr>
  
 <tr>
    <td><code>ibm-vsi-fault-tolerance</code></td>
      <td>Create an IBM VSI virtual machine by using an {{site.data.keyword.CloudDataCents_notm}} choice. To use the template, you must provide a classic infrastructure user name and API key, as well as an {{site.data.keyword.cloud_notm}} API key. For more information about how to retrieve these values, see [Supported input parameters](/docs/terraform?topic=terraform-provider-reference#provider-parameter-ov). If the virtual server instance cannot be created in the first data center, {{site.data.keyword.cloud_notm}} tries to create the instance in the subsequent data center until the order can be completed.  </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_compute_vm_instance</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-vsi-fault-tolerance)</td>
  </tr>
<tr>
    <td><code>ibm-vsi-placement-group</code></td>
      <td>Create a classic virtual server instance with a placement group. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_compute_placement_group</code></li><li style="margin:0px; padding:0px"><code>ibm_compute_vm_instance</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-vsi-placement-group)</td>
  </tr>
  
 <tr>
    <td><code>ibm-vsi</code></td>
      <td>Create a classic virtual server instance with a private VLAN only. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_compute_vm_instance</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-vsi)</td>
  </tr>
  
 <tr>
    <td><code>ibm-website-multi-region</code></td>
      <td>Provision a highly available web site architecture across multiple regions on {{site.data.keyword.cloud_notm}} classic infrastructure with Terraform. Then, deploy a single instance of WordPress by using Ansible, and replicate WordPress across the two regions to explore the security and resiliency features of IBM Cloud Security Groups, DNS, Web Application Firewall (WAF), and global and local load balancers. For more information about how to use this template, see [Tutorial: Deploying WordPress in a highly available, cross-region web site architecture with Terraform and Ansible](/docs/terraform?topic=terraform-multi_region). </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_lbaas</code></li><li style="margin:0px; padding:0px"><code>ibm_lbaas_server_instance_attachment</code></li><li style="margin:0px; padding:0px"><code>ibm_lbaas_health_monitor</code></li>
    </ul></td>
    <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-website-multi-region)</td>
  </tr>
  
   <tr>
    <td><code>ibm-website-single-region</code></td>
      <td>Provision a single-site web site architecture on {{site.data.keyword.cloud_notm}} classic infrastructure with Terraform. Then, use Ansible to deploy multiple Apache web servers with a single MariaDB database host on your {{site.data.keyword.cloud_notm}} classic virtual servers. The private IP addresses of the Apache web servers are added to the {{site.data.keyword.cloud_notm}} Load Balancer that serves as the public endpoint for your WordPress deployment. For more information about how to use the template, see Tutorial: [Deploying WordPress on IBM Cloud classic infrastructure with Terraform and Ansible](/docs/terraform?topic=terraform-deploy_wordpress). </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_compute_ssh_key</code></li><li style="margin:0px; padding:0px"><code>ibm_compute_vm_instance</code></li><li style="margin:0px; padding:0px"><code>ibm_lbaas</code></li><li style="margin:0px; padding:0px"><code>ibm_lbaas_server_instance_attachment</code></li><li style="margin:0px; padding:0px"><code>ibm_lbaas_health_monitor</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-website-single-region)</td>
  </tr>
   <tr>
    <td><code>portworx</code></td>
      <td>Set up Helm in an {{site.data.keyword.containerlong_notm}} SDS cluster to install Portworx as a storage solution. Note that this template requires an SDS cluster on bare metal worker nodes. After you installed Portworx, you can create persistent volume claims (PVC) to store data on local storage of your worker nodes. For more information about Portworx and how to create an SDS cluster, see [Storing data on software-defined storage (SDS) with Portworx](/docs/containers?topic=containers-portworx). </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>random_id</code></li><li style="margin:0px; padding:0px"><code>kubernetes_secret</code></li><li style="margin:0px; padding:0px"><code>helm_release</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/portworx)</td>
  </tr>
	  
	  
  </tbody>
  </table>


</staging>
