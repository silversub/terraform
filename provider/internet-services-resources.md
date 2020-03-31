---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-31"

keywords: terraform provider, terraform resources internet service, terraform resources cis, tf provider plugin

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
{:external: target="_blank" .external}

# Internet services resources
{: #cis-resources}

Review the [{{site.data.keyword.cis_full_notm}}](/docs/infrastructure/cis?topic=cis-about-ibm-cloud-internet-services-cis) resources that you can create, modify, or delete. You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 
{: shortdesc}

Before you start working with your resource, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## `ibm_cis`
{: #cis}

Create, update, or delete an {{site.data.keyword.cis_full_notm}} instance. 

### Sample Terraform code
{: #cis-sample}

```
data "ibm_resource_group" "group" {
  name = "test"
}

resource "ibm_cis" "cis_instance" {
  name              = "test"
  plan              = "standard"
  resource_group_id = data.ibm_resource_group.group.id
  tags              = ["tag1", "tag2"]
  location          = "global"

  //User can increase timeouts
  timeouts {
    create = "15m"
    update = "15m"
    delete = "15m"
  }
}
```

### Input parameters
{: #cis-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`name`|String|Required|A descriptive name for your {{site.data.keyword.cis_full_notm}} instance.|
|`plan`|String|Required|The name of the plan for your instance. To retrieve this value, run `ibmcloud catalog service internet-svcs`. |
|`location`|String|Required|The target location where you want to create your instance.|
|`resource_group_id`|String|Optional|The ID of the resource group where you want to create the service. To retrieve this value, run `ibmcloud resource groups` or use the `ibm_resource_group` data source. If no value is specified, the `default` resource group is used.
|`tags`|Array|Optional|A list of tags that you want to associate with the instance.|

### Output parameters
{: #cis-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The CRN of the {{site.data.keyword.cis_full_notm}} instance.|
|`guid`|String|The unique identifier of the {{site.data.keyword.cis_full_notm}} instance.|
|`status`|String|The status of the {{site.data.keyword.cis_full_notm}} instance.|

### Timeouts
{: #cis-timeouts}

The following timeouts are defined for this resource.
{: shortdesc}

- **Create**: The creation of the {{site.data.keyword.cis_full_notm}} instance is considered failed if no response is received for 10 minutes.
- **Update**: The update of the {{site.data.keyword.cis_full_notm}} instance is considered failed if no response is received for 10 minutes.
- **Delete**: The deletion of the {{site.data.keyword.cis_full_notm}} instance is considered failed if no response is received for 10 minutes.

### Import
{: #cis-import}

The {{site.data.keyword.cis_full_notm}} instance can be imported using the `crn`. 

```
terraform import ibm_cis.myorg <crn>
```
{: pre}






## `ibm_cis_domain`
{: #cis-domain}

Create, update, or delete an {{site.data.keyword.cis_full_notm}} domain.
{: shortdesc}

### Sample Terraform code
{: #cis-domain-sample}

```
resource "ibm_cis_domain" "example" {
  domain = "example.com"
  cis_id = ibm_cis.instance.id
}

resource "ibm_cis" "instance" {
  name = "test-domain"
  plan = "standard"
}
```

### Input parameters
{: #cis-domain-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`domain`|String|Required|The DNS domain name that you want to add to your {{site.data.keyword.cis_full_notm}} instance. |
|`cis_id`|String|Required|The ID of the {{site.data.keyword.cis_full_notm}} instance.|


### Output parameters
{: #cis--domain-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The unique identifier of the domain.|
|`paused`|Boolean|Indicates if the domain is paused and network traffic bypasses your {{site.data.keyword.cis_full_notm}} instance. The default values is **false**.|
|`status`|String|The status of the domain. Valid values are `active`, `pending`, `initializing`, `moved`, `deleted`, and `deactivated`. After creation, the status remains pending until the DNS Registrar is updated with the CIS name servers, exported in the `name_servers` variable.|
|`name_servers`|String|The name servers that are assigned to your {{site.data.keyword.cis_full_notm}} instance. |
|`original_name_servers`|String|The name servers that were used when the domain was first registered with the DNS Registrar. |


### Import
{: #cis-domain-import}

The {{site.data.keyword.cis_full_notm}} domain can be imported using the domain ID and service instance CRN. The ID is formed from the Domain ID of the domain concatenated using a `:` character with the CRN (Cloud Resource Name).
The Domain ID and CRN will be located on the **Overview** page of the Internet Services instance under the Domain heading of the UI, or via using the `ibmcloud cis` CLI commands.
The domain ID is a 32 digit character string in the format `1aaa11111aa1a1a1111aaa111111a11a`. CRN is a 120 digit character string of the format `crn:v1:bluemix:public:internet-svcs:global:a/1aa1111a1a1111aa1a111111111111aa:11aa111a-11a1-1a11-111a-111aaa11a1a1::`.

```
terraform import ibm_cis_domain.myorg <domain-id>:<crn>
```
{: pre}

```
terraform import ibm_cis_domain.myorg 1aaa11111aa1a1a1111aaa111111a11a:crn:v1:bluemix:public:internet-svcs:global:a/1aa1111a1a1111aa1a111111111111aa:11aa111a-11a1-1a11-111a-111aaa11a1a1::
```
{: pre}


## `ibm_cis_domain_settings`
{: #cis-domain-settings}

Customize the {{site.data.keyword.cis_full_notm}} domain settings.
{: shortdesc}

### Sample Terraform code
{: #cis-domain-settings-sample}

```
resource "ibm_cis_domain_settings" "test" {
  cis_id          = ibm_cis.instance.id
  domain_id       = ibm_cis_domain.example.id
  waf             = "on"
  ssl             = "full"
  min_tls_version = "1.2"
}
```

### Input parameters
{: #cis-domain-settings-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`domain_id`|String|Required|The ID of the domain that you want to customize. |
|`cis_id`|String|Required|The ID of the {{site.data.keyword.cis_full_notm}} instance.|
|`waf`|String|Optional|Allowed values: `off`, `on`.|
|`min_tls_version`|String|Optional|The minimum TLS version that you want to allow. Allowed values are `1.1`, `1.2`, or `1.3`. |
|`ssl`|String|Optional|Allowed values: `off`, `flexible`, `full`, `strict`, `origin_pull`.|
|`automatic_https_rewrites`|String|Optional|Enable HTTPS rewrites. Allowed values are `off` and `on`. |
|`opportunistic_encryption|String|Optional|Allowed values: `off`, and `on`.|
|`cname_flattening`|String|Optional|Allowed values: `flatten_at_root`, `flatten_all`, and `flatten_none`.|

### Output parameters
{: #cis--domain-settings-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`certificate_status`|String| Value of: `none`, `initializing`, `authorizing`, or `active`.|






## `ibm_cis_dns_record`
{: #cis-dns-record}

Create, update, or delete a DNS record for a domain.
{: shortdesc}

### Sample Terraform code
{: #cis-dns-record-sample}

```
# Add a DNS record to the domain
resource "ibm_cis_dns_record" "example" {
  cis_id    = ibm_cis.instance.id
  domain_id = ibm_cis_domain.example.id
  name      = "terraform"
  content   = "192.168.0.11"
  type      = "A"
}
```

### Input parameters
{: #cis-dns-record-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`domain_id`|String|Required|The ID of the domain for which you want to add a DNS record. |
|`cis_id`|String|Required|The ID of the {{site.data.keyword.cis_full_notm}} instance.|
|`name`|String|Required|The name of the record, like for example `www`.|
|`type`|String|Required|The type of the record. Allowed values are `A`, `AAAA`, `CNAME`, `NS`, `MX`, `TXT`, `LOC`, `SRV`, `SPF`, or `CAA`. |
|`content`|String|Optional|The value of the record, like for example `192.168.127.127`. |
|`data`|Map|Optional|A map of attributes that constitute the record value. This value is required for `LOC`, `CAA` and `SRV` record types. |
|`priority`|String|Optional|The priority of the record.|
|`proxied`|Boolean|Optional|Indicates if the record receives origin protection by {{site.data.keyword.cis_full_notm}}. The default value is **false**.|


### Output parameters
{: #cis-dns-record-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String| The ID of the record. |
|`name`|String| The FQDN of the record. |
|`proxiable`|Boolean|Indicates if the record can be proxied. |
|`created_on`|String|The RFC3339 timestamp of when the record was created. |
|`modified_on`|String|The RFC3339 timestamp of when the record was last modified. |
|`data`|Map|A map of attributes that constitute the record value.|

### Import
{: #cis-dns-record-import}

The DNS record can be imported by using the `id`. The ID is formed from the DNS record ID, the domain ID and the CRN (Cloud Resource Name). All values are concatenated with a `:` character.
The Domain ID and CRN are located on the **Overview** page of the Internet Services instance under the **Domain** heading of the UI, or via using the `ibmcloud cis` CLI.

- **Domain ID**: The domain ID is a 32 digit character string of the format `1aaa11111aa1a1a1111aaa111111a11a`.
- **CRN**: The CRN is a 120 digit character string of the format `crn:v1:bluemix:public:internet-svcs:global:a/1aa1111a1a1111aa1a111111111111aa:11aa111a-11a1-1a11-111a-111aaa11a1a1::` 
- **DNS record ID**: The DNS record ID is a 32 digit character string of the form: 111a11a1aa1aa11111a111111a111111a. The ID of an existing DNS record is not available via the UI. It can be retrieved via the CIS API or via the CLI by running `ibmcloud cis dns-records <domain_id>`.

```
terraform import ibm_cis_dns_record.myorg <dns_record_ID>:<domain_ID>:<crn>
```
{: pre}

```
terraform import ibm_cis_dns_record.myorg  111a11a1aa1aa11111a111111a111111a:1aaa11111aa1a1a1111aaa111111a11a:crn:v1:bluemix:public:internet-svcs:global:a/1aa1111a1a1111aa1a111111111111aa:11aa111a-11a1-1a11-111a-111aaa11a1a1::
```
{: pre}






## `ibm_cis_global_load_balancer`
{: #cis-global-lb}

Create, update, or delete a global load balancer. 
{: shortdesc}

### Sample Terraform code
{: #cis-global-lb-sample}

```
# Define a global load balancer which directs traffic to defined origin pools
# In normal usage different pools would be set for data centers/availability zones and/or for different regions
# Within each availability zone or region we can define multiple pools in failover order

resource "ibm_cis_global_load_balancer" "example" {
  cis_id           = ibm_cis.instance.id
  domain_id        = ibm_cis_domain.example.id
  name             = "www.example.com"
  fallback_pool_id = ibm_cis_origin_pool.example.id
  default_pool_ids = [ibm_cis_origin_pool.example.id]
  description      = "example load balancer using geo-balancing"
  proxied          = true
}

resource "ibm_cis_origin_pool" "example" {
  cis_id = ibm_cis.instance.id
  name   = "example-lb-pool"
  origins {
    name    = "example-1"
    address = "192.0.2.1"
    enabled = false
  }
}
```

### Input parameters
{: #cis-global-lb-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`domain_id`|String|Required|The ID of the domain for which you want to add a global load balancer. |
|`cis_id`|String|Required|The ID of the {{site.data.keyword.cis_full_notm}} instance.|
|`name`|String|Required|The DNS name to associate with the load balancer. This values can be a hostname, like `www`, or the fully qualified domain name, such as `www.example.com`. `example.com` is also accepted.|
|`fallback_pool_id`|String|Required|The ID of the pool to use when all other pools are considered unhealthy. |
|`default_pools_ids`|String|Required|A list of pool IDs that are ordered by their failover priority. |
|`description`|String|Optional|A description of the global load balancer. |
|`proxied`|Boolean|Optional|Indicates if the host name receives origin protection by {{site.data.keyword.cis_full_notm}}. The default value is **false**.|

### Output parameters
{: #cis-global-lb-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String| The ID of the global load balancer. |
|`name`|String| The fully qualified domain name that is associated with the load balancer. |
|`created_on`|String|The RFC3339 timestamp of when the load balancer was created. |
|`modified_on`|String|The RFC3339 timestamp of when the load balancer was last modified. |

### Import
{: #cis-global-lb-import}

The DNS record can be imported by using the `id`. The ID is formed from the global load balancer ID, the domain ID, and the CRN (Cloud Resource Name). All values are concatenated with a `:` character.
The Domain ID and CRN are located on the **Overview** page of the Internet Services instance under the **Domain** heading of the UI, or via using the `ibmcloud cis` CLI.

- **Domain ID**: The domain ID is a 32 digit character string of the format `1aaa11111aa1a1a1111aaa111111a11a`.
- **CRN**: The CRN is a 120 digit character string of the format `crn:v1:bluemix:public:internet-svcs:global:a/1aa1111a1a1111aa1a111111111111aa:11aa111a-11a1-1a11-111a-111aaa11a1a1::` 
- **Global load balancer ID**: The global load balancer ID is a 32 digit character string in the format 11a11a1aa1aa11111a111111a111111a. The ID of the load balancer is not available via the UI. It can be retrieved via the CIS API or via the CLI by running `ibmcloud cis glbs <domain_id>`.

```
terraform import ibm_cis_global_load_balancer.myorg <loadbalancer_ID>:<domain_ID>:<crn>
```
{: pre}

```
terraform import ibm_cis_dns_record.myorg  111a11a1aa1aa11111a111111a111111a:1aaa11111aa1a1a1111aaa111111a11a:crn:v1:bluemix:public:internet-svcs:global:a/1aa1111a1a1111aa1a111111111111aa:11aa111a-11a1-1a11-111a-111aaa11a1a1::
```
{: pre}






## `ibm_cis_healthcheck`
{: #cis-health}

Create, update, or delete an HTTPS health check for your {{site.data.keyword.cis_full_notm}} instance. 
{: shortdesc}

### Sample Terraform code
{: #cis-health-sample}

```
resource "ibm_cis_healthcheck" "test" {
  cis_id         = ibm_cis.instance.id
  expected_body  = "alive"
  expected_codes = "2xx"
  method         = "GET"
  timeout        = 7
  path           = "/health"
  interval       = 60
  retries        = 5
  description    = "example load balancer"
}
```

### Input parameter
{: #cis-health-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`cis_id`|String|Required|The ID of the {{site.data.keyword.cis_full_notm}} instance.|
|`expected_body`|String|Required|A case-insensitive sub-string to look for in the response body. If this string is not found, the origin will be marked as unhealthy. A null value of “” is allowed to match on any content. |
|`expected_codes`|String|Required|The expected HTTP response code or code range of the health check. Example: 200.|
|`method`|String|Optional|The HTTP method to use for the health check. Default: `GET`.|
|`timeout`|Integer|Optional|The timeout in seconds before marking the health check as failed. Default: 5.|
|`path`|String|Optional|The endpoint path to health check against. Default: `/`.|
|`interval`|Integer|Optional|The interval between each health check. Shorter intervals may improve failover time, but will increase load on the origins as we check from multiple locations. Default: 60.|
|`retries`|Integer|Optional|The number of retries to attempt in case of a timeout before marking the origin as unhealthy. Retries are attempted immediately. Default: 2.|
|`type`|String|Optional|The protocol to use for the health check. Currently supported protocols are `http` and `https`. Default: `http`.
|`description`|String|Optional|A description for your health check. |


### Output parameter
{: #cis-health-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String| The ID of the load balancer to monitor. |
|`created_on`|String|The RFC3339 timestamp of when the health check was created. |
|`modified_on`|String|The RFC3339 timestamp of when the health check was last modified. |

### Import
{: #cis-health-import}

The health check can be imported by using the `id`. The ID is formed from the health check ID and the CRN (Cloud Resource Name). All values are concatenated with a `:` character.
The CRN can be located on the **Overview** page of the Internet Services instance under the **Domain** heading of the UI, or via using the `ibmcloud cis` CLI.

- **CRN**: The CRN is a 120 digit character string of the format `crn:v1:bluemix:public:internet-svcs:global:a/1aa1111a1a1111aa1a111111111111aa:11aa111a-11a1-1a11-111a-111aaa11a1a1::` 
- **Healthcheck ID**: The health check ID is a 32 digit character string in the format 1aaaa111111aa11111111111a1a11a1. The ID of a health check is not available via the UI. It can be retrieved programmatically via the CIS API or via the CLI by running `ibmcloud cis glb-monitors`.

```
terraform import ibm_cis_healthcheck.myorg <healthcheck_ID>:<crn>
```
{: pre}

```
terraform import ibm_cis_healthcheck.myorg 1aaaa111111aa11111111111a1a11a1:crn:v1:bluemix:public:internet-svcs:global:a/1aa1111a1a1111aa1a111111111111aa:11aa111a-11a1-1a11-111a-111aaa11a1a1::
```
{: pre}






## `ibm_cis_origin_pool`
{: #cis-origin-pool}

Create, update, or delete an origin pool for your {{site.data.keyword.cis_full_notm}} instance.
{: shortdesc}

### Sample Terraform code
{: #cis-origin-pool-sample}

```
resource "ibm_cis_origin_pool" "example" {
  cis_id = ibm_cis.instance.id
  name   = "example-pool"
  origins {
    name    = "example-1"
    address = "192.0.2.1"
    enabled = false
  }
  origins {
    name    = "example-2"
    address = "192.0.2.2"
    enabled = false
  }
  description        = "example load balancer pool"
  enabled            = false
  minimum_origins    = 1
  notification_email = "someone@example.com"
  check_regions      = ["WEU"]
}
```

### Input parameter 
{: #cis-origin-pool-input}

Review the input parameters that you can specify for your resource. 
{: shortdesc}

|Name|Data type|Required/ optional|Description|
|----|-----------|-----------|---------------------|
|`cis_id`|String|Required|The ID of the {{site.data.keyword.cis_full_notm}} instance.|
|`name`|String|Required|A short name (tag) for the pool. Only alphanumeric characters, hyphens, and underscores are allowed.|
|`origins`|Array|Required|A list of origins within this pool. Traffic directed to this pool is balanced across all currently healthy origins, provided the pool itself is healthy. It’s a complex value. See description below.|
|`check_regions`|Array|Required| A list of regions (specified by region code) from which to run health checks. If the list is empty, all regions are included, but you must use the Enterprise plan. This is the default setting. Region codes can be found on the [Cloudflare’s website](https://developers.cloudflare.com/load-balancing/understand-basics/traffic-steering/#geo-steering-enterprise-plans-only){: external}.
|`description`|String|Optional|A description for your origin pool. | 
|`enabled`|Boolean|Required|If set to **true**, this pool is enabled and can receive incoming network traffic. Disabled pools do not receive network traffic and are excluded from health checks. Disabling a pool causes any load balancers that use the pool to failover to the next pool (if applicable).|
|`minimum_origins`|Integer|Optional| The minimum number of origins that must be healthy for this pool to serve traffic. If the number of healthy origins falls below this number, the pool will be marked unhealthy and we will failover to the next available pool. Default: 1.|
|`monitor`|String|Optional|The ID of the monitor to use for health checking origins within this pool.|
|`notification_email`|String|Optional|The email address to send health status notifications to. This can be an individual mailbox or a mailing list.|


### Output parameter
{: #cis-origin-pool-output}

Review the output parameters that you can access after your resource is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String| The ID of the origin pool. |
|`created_on`|String|The RFC3339 timestamp of when the origin pool was created. |
|`modified_on`|String|The RFC3339 timestamp of when the origin pool was last modified. |

### Import
{: #cis-origin-pool-import}

The origin pool can be imported by using the `id`. The ID is formed from the origin pool ID and the CRN (Cloud Resource Name). All values are concatenated with a `:` character.
The CRN can be located on the **Overview** page of the Internet Services instance under the **Domain** heading of the UI, or via using the `ibmcloud cis` CLI.

- **CRN**: The CRN is a 120 digit character string of the format `crn:v1:bluemix:public:internet-svcs:global:a/1aa1111a1a1111aa1a111111111111aa:11aa111a-11a1-1a11-111a-111aaa11a1a1::` 
- **Origin pool ID**: The origin pool ID is a 32 digit character string in the format 1aaaa111111aa11111111111a1a11a1. The ID of a origin pool is not available via the UI. It can be retrieved programmatically via the CIS API or via the CLI by running `ibmcloud cis glb-pools`.

```
terraform import ibm_cis_origin_pool.myorg <origin_pool_ID>:<crn>
```
{: pre}

```
terraform import ibm_cis_origin_pool.myorg 1aaaa111111aa11111111111a1a11a1:crn:v1:bluemix:public:internet-svcs:global:a/1aa1111a1a1111aa1a111111111111aa:11aa111a-11a1-1a11-111a-111aaa11a1a1::
```
{: pre}





