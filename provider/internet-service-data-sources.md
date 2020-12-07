---

copyright:
  years: 2017, 2020
lastupdated: "2020-12-07"

keywords: terraform internet services, terraform cis, terraform provider plugin

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

# Internet Services data sources
{: #cis_data}

You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 

Before you start working with your data source, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}

## `ibm_cis`
{: #cis}

Retrieve information about an {{site.data.keyword.cis_full_notm}} instance. 
{: shortdesc}

### Sample Terraform code
{: #cis-sample}

The following example retrieves information about an {{site.data.keyword.cis_full_notm}} instance. 
{: shortdesc}

```
data "ibm_cis" "cis_instance" {
  name              = "myinstance"
}
```

### Input parameters
{: #cis-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required/optional|Description|
|----|-----------|------|--------|
| `name` | String | Required | The name of an {{site.data.keyword.cis_full_notm}} instance. |

### Output parameters
{: #cis-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
| `id` | String | The CRN of your instance. |
| `guid` | String| The unique identifier of the instance.|
| `plan` | String | The service plan for the instance. |
| `location` | String | The location of your instance. |
| `status` | String | The status of your instance. |

## `ibm_cis_custom_pages`
{: #cis-custom-pages}

 Imports a read only copy of an existing {{site.data.keyword.cis_full_notm}} custom pages resource. For more information about custom page, refer to [CIS custom page](/docs/cis?topic=cis-custom-page).
 {: shortdesc}

### Sample Terraform code
{: #cis-custom-pages-sample}

```
# Get custom pages of the domain

data "ibm_cis_custom_pages" "custom_pages" {
    cis_id    = data.ibm_cis.cis.id
    domain_id = data.ibm_cis_domain.cis_domain.domain_id
}
```

### Output parameters
{: #cis-custom-pages-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
|`created_on`|String|Create data and time of the custom page.|
| `cis_id` | String | The ID of the CIS service instance.  |
| `description` | String | The description of the custom page.|
| `domain_id` | String | The domain ID to change custom page. |
| `id` | String | The custom page ID. It is a combination of `<page_id>, <domain_id>, <cis_id>` attributes concatenated with `:`.|
|`modified_on`|String|Modified data and time of the custom page.|
| `page_id ` | String | The custom page identifier. Valid values are `basic_challenge`, `waf_challenge`, `waf_block`, `ratelimit_block`, `country_challenge`, `ip_block`, `under_attack`, `500_errors`, `1000_errors`, `always_online`. |
| `preview_target` | String | The target custom page.|
| `required_tokens` | String | The custom page required token which is expected from the URL page.|
| `state` | String | The custom page state. This is set default when there is an empty URL and can customize when URL is set with some URL.|
| `url` | String | The URL for custom page settings. By default URL is set with empty string `""`. Setting a duplicate empty string throws an error.|


## `ibm_cis_domain`
{: #cis_domain}

Retrieve information about an {{site.data.keyword.cis_full_notm}} domain. 
{: shortdesc}

### Sample Terraform code
{: #cis-domain-sample}

The following example retrieves information about an {{site.data.keyword.cis_full_notm}} domain. 
{: shortdesc}

```
data "ibm_cis_domain" "cis_instance_domain" {
  domain = "mydomain.com"
  cis_id = ibm_cis.instance.id
}

data "ibm_cis" "cis_instance" {
  name = "myinstance"
}
```

### Input parameters
{: #cis-domain-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required/optional|Description|
|----|-----------|------|--------|
| `domain` | String | Required | The DNS domain name that is added and managed for an {{site.data.keyword.cis_full_notm}} instance. |
| `cis_id` | String | Required | The ID of the {{site.data.keyword.cis_full_notm}} instance. |

### Output parameters
{: #cis-domain-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
| `id` | String | The unique identifier of your domain. |
| `paused` | Boolean | If set to **true**, network traffic to this domain is paused. If set to **false**, network traffic to this domain is permitted. The default value is **false**.  |
| `status` | String | The status of your domain. Valid values are `active`, `pending`, `initializing`, `moved`, `deleted`, and `deactivated`. After creation, the status remains pending until the DNS Registrar is updated with the CIS name servers, exported in the ‘name_servers’ variable. |
| `name_servers` | String | The {{site.data.keyword.cis_full_notm}} assigned name servers, to be passed by interpolation to the resource dns_domain_registration_nameservers. |
| `original_name_servers` | String | The name servers from when the Domain was initially registered with the DNS Registrar.|

## `ibm_cis_edge_functions_actions`
{: #cis-edge-functions-actions-ds}

Retrieve information about an {{site.data.keyword.cis_full_notm}} edge function actions resource.
{: shortdesc}

### Sample Terraform code
{: #cis-edge-functions-actions-dssample}

The following example retrieves information about an {{site.data.keyword.cis_full_notm}} edge function actions resource.
{: shortdesc}

```
data "ibm_cis_edge_functions_actions" "test_actions" {
    cis_id    = data.ibm_cis.cis.id
    domain_id = data.ibm_cis_domain.cis_domain.domain_id
}
```

### Input parameters
{: #cis-edge-functions-actions-dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required/optional|Description|
|----|-----------|------|--------|
| `domain_id` | String | Required | The ID of the domain to add an edge functions action. |
| `cis_id` | String | Required | The ID of the {{site.data.keyword.cis_full_notm}} instance. |

### Output parameters
{: #cis-edge-functions-actions-dsoutput}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
| `created_on` | String | An action created date. |
| `etag` | String | An action E-Tag. |
| `handler` | String | An action handler methods.  |
| `modified_on` | String | An action modified date. |
| `routes` | String | An action route detail.|
| `routes.action_name` | String | An action route detail.|
| `routes.pattern_url` | String | The Route pattern. It is a domain name in which the action is performed.|
| `routes.request_limit_fail_open` | String | An action request limit fail open.|
| `routes.trigger_id` | String | The Trigger ID of an action.|


## `ibm_cis_edge_functions_triggers`
{: #cis-edge-functions-triggers-ds}

Retrieve information about an {{site.data.keyword.cis_full_notm}} edge function triggers resource.
{: shortdesc}

### Sample Terraform code
{: #cis-edge-functions-triggers-dssample}

The following example retrieves information about an {{site.data.keyword.cis_full_notm}} edge function actions resource.
{: shortdesc}

```
data "ibm_cis_edge_functions_triggers" "test_triggers" {
    cis_id    = data.ibm_cis.cis.id
    domain_id = data.ibm_cis_domain.cis_domain.domain_id
}
```

### Input parameters
{: #cis-edge-functions-triggers-dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required/optional|Description|
|----|-----------|------|--------|
| `domain_id` | String | Required | The ID of the domain to add an edge functions triggers. |
| `cis_id` | String | Required | The ID of the {{site.data.keyword.cis_full_notm}} instance. |

### Output parameters
{: #cis-edge-functions-triggers-dsoutput}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
| `action_name` | String | An action script for execution.|
| `pattern_url` | String | The Route pattern. It is a domain name in which the action is performed.|
| `request_limit_fail_open` | String | An action request limit fail open.|
| `trigger_id` | String | The route ID of an action trigger.|

## `ibm_cis_firewall`
{: #cis-firewallds}

Retrieves an existing {{site.data.keyword.cis_full_notm}} instance. For more information, see [firewall rule actions](/docs/cis?topic=cis-actions).
{: shortdesc}

### Sample Terraform code
{: #cis-firewall-dssample}

```
data "ibm_cis_firewall" "lockdown" {
  cis_id    = ibm_cis.instance.id
  domain_id = ibm_cis_domain.example.id
  firewall_type = "lockdowns"
}
```
IBM Terraform provider supports only lockdowns rules.
{: note}

### Input parameters
{: #cis-firewall-dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required / optional|Description|
|----|-----------|-----------|---------------------|
|`cis_id`|String|Required|The ID of the {{site.data.keyword.cis_full_notm}} instance where you want to create the firewall.|
|`domain_id`|String|Required|The ID of the domain where you want to add the lockdown.|
|`firewall_type`|String|Required|The type of firewall that you want to create for your domain. Supported values are `lockdowns`, `access_rules`, and `ua_rules`. Consider the following information when choosing your firewall type: <ul><li><strong><code>access_rules</code></strong>: Access rules allow, challenge, or block requests to your website. You can apply access rules to one domain only or all domains in the same service instance.</li><li><strong><code>ua_rules</code></strong>: Apply firewall rules only if the user agent that is used by the client matches the user agent that you defined. </li><li><strong><code>lockdowns</code></strong>: Allow access to your domain for specific IP addresses or IP address ranges only. If you choose this firewall type, you must define your firewall rules in the `lockdown` input parameter.</li></ul>|

### Output parameters
{: #cis-firewall-dsoutput}

Review the output parameters that you can access after your data source is created. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|--------|
|`id`|String|The ID of the record. The ID is composed of `<firewall_type>,<lockdown_id/access_rule_id/ua_rule_id>,<domain_ID>,<cis_crn>`. Attributes are concatenated with `:`.|
|`lockdown.lockdown_id`|List|List of lockdown to be created. The data describing a lockdowns rule.|
|`lockdown.paused`|Boolean|Required|If set to **true**, the firewall rule is disabled. If set to **false**, the firewall rule is enabled.|
|`lockdown.description`|String|A description for your firewall rule.|
|`lockdown.priority`|Integer|The priority of the firewall rule. A low number is associated with a high priority. |
|`lockdown.urls`|List of URLs|A list of URLs that you want to include in your firewall rule. You can specify wildcard URLs. The URL pattern is escaped before use.|
|`lockdown.configurations`|List of IP addresses|A list of IP address or CIDR ranges that you want to allow access to the URLs that you defined in `lockdown.urls`. |
|`lockdown.configurations.target`|String|Specify if you want to target an `ip` or `ip_range`.|
|`lockdown.configurations.value`|String|The IP addresses or CIDR. |
|`access_rule`|String|Create the data describing the access rule. |
|`access_rule.rule_id`|String| The access rule ID. |
|`access_rule.notes`|String| The free text for notes. |
|`access_rule.mode`|String| The mode of access rule. The valid modes are `block`, `challenge`, `whitelist`, `js_challenge`.|
|`access_rule.configuration`|List| The Configuration of firewall. (MaxItems: 1) |
|`access_rule.configuration.target`|String| The request property to target. Valid values are `ip`, `ip_range`, `asn`, `country`. |
|`access_rule.configuration.value`|String| IP address or CIDR or Autonomous or Country code. |
|`ua_rule`|String|Create the data describing the user agent rule. |
|`ua_rule.ua_rule_id`|String| The user agent rule ID. |
|`ua_rule.description `|String|The free text for description. |
|`ua_rule.mode`|String|The mode of access rule. The valid modes are `block`, `challenge`,  `js_challenge`. |
|`ua_rule.paused`|String|Whether the rule is currently disabled. |
|`ua_rule.configuration`|List| The Configuration of firewall. |
|`ua_rule.configuration.target`|String| The request property to target. Valid values are `ua`. |
|`ua_rule.configuration.value`|String| The exact User Agent string to match the rule. |

Exactly one of `lockdown`, `access_rule`, and `ua_rule` is allowed for the respective firewall types `lockdowns`, `access_rules`, and `ua_rules`.
{: note}

## `ibm_cis_global_load_balancers`
{: #cis-global-lb-ds}

Retrieve information 24 X 7 availability and performance of your application by using the {{site.data.keyword.cis_full_notm}} global load balancers. For more information, refer to [CIS global loadbalancer](/docs/cis?topic=cis-configure-glb).Import the details of an existing {{site.data.keyword.cis_full_notm}} global load balancers as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.
{: shortdesc}

### Sample Terraform code
{: #cis-global-lb-dssample}

The following example retrieves information about an {{site.data.keyword.cis_full_notm}} global load balancer resource.
{: shortdesc}

```
data "ibm_cis_global_load_balancers" "test" {
  cis_id    = var.cis_crn
  domain_id = var.zone_id
}
```

### Input parameters
{: #cis-global-lb-dsinput}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required/optional|Description|
|----|-----------|------|--------|
| `domain_id` | String | Required | The ID of the domain to retrieve the load balancers from. |
| `cis_id` | String | Required | The resource CRN ID of the CIS on which zones were created. |

### Output parameters
{: #cis-global-lb-dsoutput}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
| `id` | String | The Load balancer ID, domain ID and CRN. For example, `id:domain-id:crn`. |
| `description` | String | Free text description. |
| `default_pool_ids` | String | A list of pool IDs ordered by their failover priority. Used whenever region or pop pools are not defined. |
| `fallback_pool_id` | String | The pool ID to use when all other pools are detected as unhealthy. |
| `glb_id` | String | The Load balancer ID. |
| `enabled` | String | Indicates if the load balancer is enabled or not. Region and pop pools are not currently implemented in this version of the provider. |
| `name` | String | The DNS name to associate with the load balancer. This can be a hostname, for example, `www` or the fully qualified name `www.example.com`, or `example.com`. |
| `proxied` | String | Whether the hostname gets IBM's origin protection. Defaults to `false`.  |
| `pop_pools` | String | A set containing mappings of IBM Point-of-Presence (PoP) identifiers to a list of pool IDs (ordered by their failover priority) for the PoP (datacenter). This feature is only available to enterprise customers.|
| `pop_pools.pop` | String | A 3-letter code for the Point-of-Presence. Multiple entries should not be specified with the same PoP.|
| `pop_pools.pool_ids` | String | A list of pool IDs in failover priority to use for traffic reaching the given PoP.|
| `region_pools` | String | A set containing mappings of region or country codes to a list of pool IDs (ordered by their failover priority) for the given region.|
| `region_pools.region` | String | A region code. Multiple entries is not allowed with the same region.|
| `region_pools.pool_ids` | String | A list of pool IDs in failover priority to use in the given region.|
| `session_affinity` | String | Associates all requests coming from an end-user with a single origin. IBM will set a cookie on the initial response to the client, such that consequent requests with the cookie in the request will go to the same origin, as long as it is available. |
| `ttl` | String | Time to live (TTL) of the DNS entry for the IP address returned by this load balancer.  |


## `ibm_cis_healthchecks`
{: #cis-healthchecks}

Retrieve information about an {{site.data.keyword.cis_full_notm}} global load balancer health monitor or check as a read-only data source.
{: shortdesc}

### Sample Terraform code
{: #cis-healthchecks-sample}

The following example retrieves information about an {{site.data.keyword.cis_full_notm}} domain. 
{: shortdesc}

```
data "ibm_cis_glb_health_checks" "test" {
  cis_id = var.cis_crn
}
```

### Input parameters
{: #cis-healthchecks-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required/optional|Description|
|----|-----------|------|--------|
| `cis_id` | String | Required | The resource CRN ID of the {{site.data.keyword.cis_full_notm}} on which zones were created. |

### Output parameters
{: #cis-healthchecks-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
| `allow_insecure` | String | Do not validate the certificate when health check uses `HTTPS`.|
| `created_on` | String | The RFC3339 timestamp of when the load balancer monitor was created.|
| `description` | String | Free text description.|
| `expected_body` | String | The requested body.|
| `expected_codes` | String | The expected HTTP response code or code range of the health check. For example, `2xx`.|
| `headers` | String | The health check header.|
| `id` | String | The load balancer monitor ID and CRN. For example, `monitor_id:crn`.|
| `interval` | String | The interval between each health check. Shorter intervals improve failover time, but can increase load on the origins as you check from multiple locations. The default value is `60`.|
| `modified_on` | String | The RFC3339 timestamp of when the load balancer monitor was last modified.|
| `monitor_id` | String | The load balancer monitor ID.|
| `method` | String | The HTTP method to use for the health check.|
| `path` | String | The endpoint path to health check.|
| `port` | String | The TCP port to use for the health check.|
| `retries` | String | The number of retries to attempt in case of a timeout before marking the origin as unhealthy. Retries are attempted immediately. The default value is `2`.|
| `timeout` | String | The timeout (in seconds) before marking the health check as failed. The default value is `5`.|
| `type` | String | The protocol to use for the health check. Currently supported protocols are `HTTP`, `HTTPS`, and `TCP`. The default value is `HTTP`.|
| `follow_redirects` | String | Follow redirects if returned by the origin.|

## `ibm_cis_ip_addresses`
{: #cis_ip}

Import a list of all IP addresses that the CIS proxy uses. The CIS proxy uses these IP addresses for both `client-to-proxy` and `proxy-to-origin` communication. You can reference the IP addresses by using Terraform interpolation syntax to configure and allowed IP addresses in firewalls, network ACLs, and security groups. 
{: shortdesc}

### Sample Terraform code
{: #cis-ip-sample}

The following example retrieves information about IP addresses that {{site.data.keyword.cis_full_notm}} uses for name servers. 
{: shortdesc}

```
data "ibm_cis_ip_addresses" "cisname" {
}
```

### Input parameters
{: #cis-ip-input}

No input parameters are required for this data source. 

### Output parameters
{: #cis-ip-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
| `ipv4_cidrs` | String | The IPv4 address ranges that the CIS proxy uses and that you can reference to configure and allowed IP addresses in firewalls, network ACLs, and security groups. |
| `ipv6_cidrs` | String | The IPv6 address ranges that the CIS proxy uses and that you can reference to configure and allowed IP addresses in firewalls, network ACLs, and security groups.|

## `ibm_cis_origin_pools`
{: #origin-pools}

Retrieves an {{site.data.keyword.cis_full_notm}} origin pool resource. This provides a pool of origins that is used by an {{site.data.keyword.cis_full_notm}} Global Load Balancer. This resource is associated with an {{site.data.keyword.cis_full_notm}} instance and optionally an {{site.data.keyword.cis_full_notm}} Healthcheck monitor resource.
{: shortdesc}

### Sample Terraform code
{: #origin-pools-sample}

```
data "ibm_cis_origin_pools" "test" {
  cis_id = var.cis_crn
}
```

### Input parameters
{: #origin-pools-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required/optional|Description|
|----|-----------|------|--------|
|`cis_id`|String|Required|The ID of the CIS service instance. |  

### Output parameters
{: #origin-pools-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
|`created_on`|String|Created RFC3339 timestamp of the Load Balancer. |
|`description`|String|The description of the origin pool. |
|`enabled`|String|The default value is `enabled`. Disabled pools do not receive traffic, and are excluded from health checks. Disabling a pool cause any Load Balancers using it to failover to the next pool (if any). |
|`healthy`|String|The status of the origin pool. |
|`id`|String|The ID of the Load Balancer pool.|
|`modified_on`|String|Last modified RFC3339 timestamp of the Load Balancer. |
|`monitor`|String|The ID of the monitor to use for health checking origins within this pool.|
|`name`|String|A short name `tag` for the pool. Only alphanumeric characters, hyphens, and underscores are allowed. |
|`notification_email`|String|The Email address to send health status notifications. This can be an individual mailbox or a mailing list.|
|`origins`|String|The list of origins within this pool. Traffic directed at this pool is balanced across all currently healthy origins, provided the pool itself is healthy. Description of it's complex value is stated. |
|`origins.name`|String|A human-identifiable name of the origin. |
|`origins.address`|String|The IP address `IPv4` or `IPv6` of the origin, or the publicly addressable hostname. Hostnames entered is resolved directly to the origin, and not be a hostname proxied by CIS.|
|`origins.enabled`|String|The default value is `enable`. Disabled origins do not receive traffic, and are excluded from health checks. The origin is disabled only for the current pool.|
|`origins.weight`|String|The weight of the origin pool.|
|`origins.healthy`|String|The status of origins health.|
|`origins.disabled_at`|String|The disabled date and time.|
|`origins.failure_reason`|String|The failure reason.|


## `ibm_cis_rate_limit`
{: #rate-limit}

Retrieve information for a rate limiting rule of an {{site.data.keyword.cis_full_notm}} domain.
{: shortdesc}

To retrieve information about a rate limiting rule, you must have the enterprise plan for an {{site.data.keyword.cis_full_notm}}. 
{: note}

### Sample Terraform code
{: #rate-limit-sample}

```
data "ibm_cis_rate_limit" "ratelimit" {
    cis_id = data.ibm_cis.cis.id
    domain_id = data.ibm_cis_domain.cis_domain.id
}
```
{: codeblock}

### Input parameters
{: #rate-limit-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

|Name|Data type|Required/optional|Description|
|----|-----------|------|--------|
|`cis_id`|String|Required|The ID of the {{site.data.keyword.cis_full_notm}} instance where you created the rate limiting rule. |  
|`domain_id`|String|Required|The ID of the domain where you created the rate limiting rule. |

### Output parameters
{: #rate-limit-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

|Name|Data type|Description|
|----|-----------|----------|
|`id`|String|The record ID of the rate limiting rule in the format `<rule_ID>:<domain_ID>:<cis_ID>`.|
|`rule_id`|String|The ID of the rate limiting rule. |
|`threshold`|Integer|The number of requests received within a specific time period (`period`) before connections to the domain are refused. The threshold value can be between 2 and 1000000. |
|`period`|Integer|The period of time in seconds where incoming requests to a domain are counted. If the number of requests exceeds the `threshold`, then connections to the domain are refused. The `period` value can be between 1 and 3600. |
|`match`|List of matching rules|A list of characteristics that incoming network traffic must match to be counted toward the `threshold`. | 
|`match.request`|List of request characteristics|A list of characteristics that the incoming request must match to be counted toward the `threshold`. If no list is provided, all incoming requests are counted toward the `threshold`.|
|`match.request.url`|String|The URL that the request uses. Wildcard domains are expanded to match applicable traffic, query strings are not matched. If `*` is returned, the rule is applied to all URLs. The maximum length of this value can be 1024.|
|`match.request.schemes`|Set of strings|The scheme of the request that determines the protocol that you want. Supported values are `HTTPS`, `HTTP,HTTPS`, and `ALL`. |
|`match.request.methods`|Set of strings|The HTTP methods that the incoming request can use to be counted toward the `threshold`. Supported values are `GET`, `POST`, `PUT`, `DELETE`, `PATCH`, `HEAD`, and `ALL`. You can also combine multiple methods and separate them with a comma. For example `POST,PUT`. |
|`response`|List of HTTP responses|A list of HTTP responses that outgoing packets must match before they can be returned to the client. If an incoming request matches the request criteria, but the response does not match the response criteria, then the request packet is not counted toward the `threshold`.| 
|`response.status`|Set of integers|The HTTP status code that the response must have so that the request is counted toward the `threshold`. The value can be between 100 and 999. If you want to use multiple response codes, you must separate them with a comma, such as `401,403`.|
|`response.header`|List of response headers|A list of HTTP response headers that the response packet must match so that the original request is counted toward the `threshold`.|
|`response.header.name`|String|The name of the HTTP response header.|
|`response.header.op`|String|The operator that applied to your HTTP response header. Supported values are `eq` (equals) and `ne` (not equals). |
|`response.header.value`|String|The value that the HTTP response header must match. |
|`action`|List of actions|A list of actions that you want to perform when incoming requests exceed the specified `threshold`.|
|`action.mode`|String|The type of action that you want to perform. Supported values are `simulate`, `ban`, `challenge`, or `js_challenge`. For more information, about each type, see [Configure response](/docs/cis?topic=cis-cis-rate-limiting#rate-limiting-configure-response).|
|`action.timeout`|Integer|The time to wait in seconds before the action is performed. The timeout must be equal to or greater than the `period` and is valid only for actions of type `simulate` or `ban`. The value can be between 10 and 86400.|
|`action.response`|List of response information|A list of information that you want to return to the client, such as the `content-type` and specific body information. The information provided in this parameter overrides the default HTML error page that is returned to the client. This option is valid only for actions of type `simulate` or `ban`.  |
|`action.response.content_type`|String|The `content-type` of the body that you want to return. Supported values are `text/plain`, `text/xml`, and `application/json`.|
|`action.response.body`|String|The body of the response that you want to return to the client. The information must match the `action.response.content_type` that you specified. The value can have a maximum length of 1024.|
|`disabled`|Boolean|If set to **true**, rate limiting is disabled for the domain.|
|`description`|String|The description for your rate limiting rule. |
|`correlate`|List of NAT-based rate limits|If provided, NAT-based rate limiting is enabled.|
|`correlate.by`|String|If set to `nat`, NAT-based rate limiting is enabled.|
|`bypass`|List of bypass criteria|A list of key-value pairs that, when matched, allow the rate limiting rule to be ignored.  |
|`bypass.name`|String|The name of the key that you want to apply. Supported values are `url`. |
|`bypass.value`|String|The value of the key that you want to match. When `bypass.name` is set to `url`, `bypass.value` contains the URL that you want to exclude from the rate limiting rule. |
