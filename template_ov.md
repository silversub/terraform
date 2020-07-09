
  </tbody>
  </table>

## Object Storage resources

  <table>
    <thead>
    <th>Name</th>
    <th>Description</th>
    <th>IBM Cloud Provider resources</th>
    <th>View in GitHub</th>
  </thead>
  <tbody>
   <tr>
     <td><code>ibm-cos-bucket</code></td>
      <td>Create an [IBM Cloud Object Storage](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage) service instance in IBM Cloud and your first bucket to persistently store data.</td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_resource_group</code></li><li style="margin:0px; padding:0px"><code>ibm_resource_instance</code></li><li style="margin:0px; padding:0px"><code>ibm_cos_bucket</code></li></ul></td>
      <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cos-bucket)</td>
  </tr>
  <tr>
  </tbody>
  </table>

## Power Systems resources

  <table>
    <thead>
    <th>Name</th>
    <th>Description</th>
    <th>IBM Cloud Provider resources</th>
    <th>View in GitHub</th>
   </thead>
  <tbody>
 <tr>
    <td><code>ibm-power</code></td>
      <td>Create a {{site.data.keyword.powersys_notm}} instance with a public and a private network that mounts the system volumes. You can also create an SSH key to access the instance. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_pi_key</code></li><li style="margin:0px; padding:0px"><code>ibm_pi_network</code></li><li style="margin:0px; padding:0px"><code>ibm_pi_volume</code></li>
	  <li style="margin:0px; padding:0px"><code>ibm_pi_instance</code></li></ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-power)</td>
  </tr>
  </tbody>
  </table>

## Resource Management resources

  <table>
    <thead>
    <th>Name</th>
    <th>Description</th>
    <th>IBM Cloud Provider resources</th>
    <th>View in GitHub</th>
   </thead>
  <tbody>
  <tr>
    <td><code>ibm-resource-instance</code></td>
      <td>Create an {{site.data.keyword.cos_full_notm}} service instance with HMAC credentials, and configure custom timeouts for creating, updating, or deleting the instance. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm-resource-instance</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-resource-instance)</td>
  </tr>
  </tbody>
  </table>

## Schematics Data resources

   <table>
    <thead>
    <th>Name</th>
    <th>Description</th>
    <th>IBM Cloud Provider resources</th>
    <th>View in GitHub</th>
   </thead>
  <tbody>
 <tr>
    <td><code>ibm-schematics</code></td>
      <td>Retrieve the Terraform state file and output variables for a {{site.data.keyword.bpshort}} workspace by using a {{site.data.keyword.bpshort}} data source. For more information about how to use the data source, see [Managing cross-workspace state access with Terraform](/docs/schematics?topic=schematics-remote-state). </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>N/A</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-schematics)</td>
  </tr>
   </tbody>
  </table>

## VPC infrastructure resources (Gen 1 compute)

  <table>
  <thead>
    <th>Name</th>
    <th>Description</th>
    <th>IBM Cloud Provider resources</th>
    <th>View in GitHub</th>
  </thead>
  <tbody>
  <tr>
    <td><code>ibm-is-ng</code></td>
      <td>Create a Virtual Private Cloud (VPC) for Generation 2 compute, configure a VPC load balancer with custom routing rules, and add a virtual server instance to your VPC that you can access from the internet by using a public IP address. Then, create another VPC Gen 2 and configure it with a VPN gateway with custom IPSec and IKE networking rules. You also learn how to create VPC Gen 2 block storage volumes.  </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_is_vpc</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpc_route</code></li><li style="margin:0px; padding:0px"><code>ibm_is_subnet</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb_listener</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb_listener_policy</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb_listener_policy_rule</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpn_gateway</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpn_gateway_connection</code></li><li style="margin:0px; padding:0px"><code>ibm_is_ssh_key</code></li><li style="margin:0px; padding:0px"><code>ibm_is_instance</code></li><li style="margin:0px; padding:0px"><code>ibm_is_floating_ip</code></li><li style="margin:0px; padding:0px"><code>ibm_is_security_group_rule</code></li><li style="margin:0px; padding:0px"><code>ibm_is_ipsec_policy</code></li><li style="margin:0px; padding:0px"><code>ibm_is_ike_policy</code></li><li style="margin:0px; padding:0px"><code>ibm_is_volume</code></li><li style="margin:0px; padding:0px"><code>ibm_is_public_gateway</code></li></ul></td>
      <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/blob/master/examples/ibm-is-ng)</td>
  </tr>
  <tr>
    <td><code>ibm-is-vpc</code></td>
      <td>Create a Virtual Private Cloud (VPC) for Generation 1 compute, configure a VPC load balancer with custom routing rules, and add a virtual server instance to your VPC that you can access from the internet by using a public IP address. Then, create another VPC Gen 1 and configure it with a VPN gateway with custom IPSec and IKE networking rules. You also learn how to create VPC Gen 1 block storage volumes.  </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_is_vpc</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpc_route</code></li><li style="margin:0px; padding:0px"><code>ibm_is_subnet</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb_listener</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb_listener_policy</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb_listener_policy_rule</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpn_gateway</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpn_gateway_connection</code></li><li style="margin:0px; padding:0px"><code>ibm_is_ssh_key</code></li><li style="margin:0px; padding:0px"><code>ibm_is_instance</code></li><li style="margin:0px; padding:0px"><code>ibm_is_floating_ip</code></li><li style="margin:0px; padding:0px"><code>ibm_is_security_group_rule</code></li><li style="margin:0px; padding:0px"><code>ibm_is_ipsec_policy</code></li><li style="margin:0px; padding:0px"><code>ibm_is_ike_policy</code></li><li style="margin:0px; padding:0px"><code>ibm_is_volume</code></li><li style="margin:0px; padding:0px"><code>ibm_is_public_gateway</code></li></ul></td>
      <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/blob/master/examples/ibm-is-vpc)</td>
  </tr>
   <tr>
    <td><code>ibm-transit-gateway</code></td>
      <td>Create an {{site.data.keyword.tg_full_notm}} service instance. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_tg_gateway</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-transit-gateway)</td>
  </tr>
  </tbody>
  </table>

## VPC infrastructure resources (Gen 2 compute)

  <table>
  <thead>
    <th>Name</th>
    <th>Description</th>
    <th>IBM Cloud Provider resources</th>
    <th>View in GitHub</th>
  </thead>
  <tbody>
  <tr>
    <td><code>ibm-is-ng</code></td>
      <td>Create a Virtual Private Cloud (VPC) for Generation 2 compute, configure a VPC load balancer with custom routing rules, and add a virtual server instance to your VPC that you can access from the internet by using a public IP address. Then, create another VPC Gen 2 and configure it with a VPN gateway with custom IPSec and IKE networking rules. You also learn how to create VPC Gen 2 block storage volumes.  </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_is_vpc</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpc_route</code></li><li style="margin:0px; padding:0px"><code>ibm_is_subnet</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb_listener</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb_listener_policy</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb_listener_policy_rule</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpn_gateway</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpn_gateway_connection</code></li><li style="margin:0px; padding:0px"><code>ibm_is_ssh_key</code></li><li style="margin:0px; padding:0px"><code>ibm_is_instance</code></li><li style="margin:0px; padding:0px"><code>ibm_is_floating_ip</code></li><li style="margin:0px; padding:0px"><code>ibm_is_security_group_rule</code></li><li style="margin:0px; padding:0px"><code>ibm_is_ipsec_policy</code></li><li style="margin:0px; padding:0px"><code>ibm_is_ike_policy</code></li><li style="margin:0px; padding:0px"><code>ibm_is_volume</code></li><li style="margin:0px; padding:0px"><code>ibm_is_public_gateway</code></li></ul></td>
      <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/blob/master/examples/ibm-is-ng)</td>
  </tr>
  <tr>
    <td><code>ibm-is-vpc</code></td>
      <td>Create a Virtual Private Cloud (VPC) for Generation 1 compute, configure a VPC load balancer with custom routing rules, and add a virtual server instance to your VPC that you can access from the internet by using a public IP address. Then, create another VPC Gen 1 and configure it with a VPN gateway with custom IPSec and IKE networking rules. You also learn how to create VPC Gen 1 block storage volumes.  </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_is_vpc</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpc_route</code></li><li style="margin:0px; padding:0px"><code>ibm_is_subnet</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb_listener</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb_listener_policy</code></li><li style="margin:0px; padding:0px"><code>ibm_is_lb_listener_policy_rule</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpn_gateway</code></li><li style="margin:0px; padding:0px"><code>ibm_is_vpn_gateway_connection</code></li><li style="margin:0px; padding:0px"><code>ibm_is_ssh_key</code></li><li style="margin:0px; padding:0px"><code>ibm_is_instance</code></li><li style="margin:0px; padding:0px"><code>ibm_is_floating_ip</code></li><li style="margin:0px; padding:0px"><code>ibm_is_security_group_rule</code></li><li style="margin:0px; padding:0px"><code>ibm_is_ipsec_policy</code></li><li style="margin:0px; padding:0px"><code>ibm_is_ike_policy</code></li><li style="margin:0px; padding:0px"><code>ibm_is_volume</code></li><li style="margin:0px; padding:0px"><code>ibm_is_public_gateway</code></li></ul></td>
      <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/blob/master/examples/ibm-is-vpc)</td>
  </tr>
   <tr>
    <td><code>ibm-transit-gateway</code></td>
      <td>Create an {{site.data.keyword.tg_full_notm}} service instance. </td>
      <td><ul style="margin:0px 0px 0px 20px; padding:0px"><li style="margin:0px; padding:0px"><code>ibm_tg_gateway</code></li>
    </ul></td>
	  <td>[Open](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-transit-gateway)</td>
  </tr>
  </tbody>
  </table>
</staging>