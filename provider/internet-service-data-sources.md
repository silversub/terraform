---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-04"

keywords: terraform internet services, terraform cis, terraform provider plugin

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

# Internet Services data sources
{: #cis_data}

You can reference the output parameters for each resource in other resources or data sources by using [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external}. 

Before you start working with your data source, make sure to review the [required parameters](/docs/terraform?topic=terraform-provider-reference#required-parameters) that you need to specify in the `provider` block of your Terraform configuration file. 
{: important}


## `ibm_cis`
{: #cis}

Retrieve information about an existing {{site.data.keyword.cis_full_notm}} instance. 
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
| `name` | String | Required | The name of your {{site.data.keyword.cis_full_notm}} instance. |

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
| `domain` | String | Required | The DNS domain name that is added and managed for your {{site.data.keyword.cis_full_notm}} instance. |
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
| `name_servers` | String | The IBM CIS assigned name servers, to be passed by interpolation to the resource dns_domain_registration_nameservers. |
| `original_name_servers` | String | The name servers from when the Domain was initially registered with the DNS Registrar.|

## `ibm_cis_ip_addresses`
{: #cis_ip}

Import a list of all IP addresses that the CIS proxy uses. The CIS proxy uses these IP addresses for both `client-to-proxy` and `proxy-to-origin` communication. You can reference the IP addresses by using Terraform interpolation syntax to configure and whitelist IP addresses in firewalls, network ACLs, and security groups. 
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
| `ipv4_cidrs` | String | The IPv4 address ranges that the CIS proxy uses and that you can reference to configure and whitelist IP addresses in firewalls, network ACLs, and security groups. |
| `ipv6_cidrs` | String | The IPv6 address ranges that the CIS proxy uses and that you can reference to configure and whitelist IP addresses in firewalls, network ACLs, and security groups.|


## `ibm_cis_rate_limit`
{: #rate-limit}

Retrieve information for a rate limiting rule of an IBM Cloud Internet Services domain.
{: shortdesc}

To retrieve information about a rate limiting rule, you must have the enterprise plan for IBM Cloud Internet Services. 
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
|`cis_id`|String|Required|The ID of the IBM Cloud Internet Services instance where you created the rate limiting rule. |  
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
|`match`|List of matching rules|A list of characteristics that incoming network traffic must match to be counted towards the `threshold`. | 
|`match.request`|List of request characteristics|A list of characteristics that the incoming request must match to be counted towards the `threshold`. If no list is provided, all incoming requests are counted towards the `threshold`.|
|`match.request.url`|String|The URL that the request uses. Wildcard domains are expanded to match applicable traffic, query strings are not matched. If `*` is returned, the rule is applied to all URLs. The maximum length of this value can be 1024.|
|`match.request.schemes`|Set of strings|The scheme of the request that determines the desired protocol. Supported values are `HTTPS`, `HTTP,HTTPS`, and `ALL`. |
|`match.request.methods`|Set of strings|The HTTP methods that the incoming request can use to be counted towards the `threshold`. Supported values are `GET`, `POST`, `PUT`, `DELETE`, `PATCH`, `HEAD`, and `ALL`. You can also combine multiple methods and separate them with a comma. For example `POST,PUT`. |
|`response`|List of HTTP responses|A list of HTTP responses that outgoing packets must match before they can be returned to the client. If an incoming request matches the request criteria, but the reponse does not match the response criteria, then the request packet is not counted towards the `threshold`.| 
|`response.status`|Set of integers|The HTTP status code that the response must have so that the request is counted towards the `threshold`. The value can be between 100 and 999. |
|`response.header`|List of response headers|A list of HTTP response headers that the response packet must match so that the original request is counted towards the `threshold`.|
|`response.header.name`|String|The name of the HTTP response header.|
|`response.header.op`|String|The operator that applied to your HTTP response header. Supported values are `eq` (equals) and `ne` (not equals). |
|`response.header.value`|String|The value that the HTTP response header must match. |
|`action`|List of actions|A list of actions that you want to perform when incoming requests exceed the specified `threshold`.|
|`action.mode`|String|The type of action that you want to perform. Supported values are `simulate`, `ban`, `challenge`, or `js_challenge`. For more information about each type, see [Configure response](/docs/cis?topic=cis-cis-rate-limiting#rate-limiting-configure-response).|
|`action.timeout`|Integer|The time to wait in seconds before the action is performed. The timeout must be equal to or greater than the `period` and is valid only for actions of type `simulate` or `ban`. The value can be between 10 and 86400.|
|`action.response`|List of reponse information|A list of information that you want to return to the client, such as the `content-type` and specific body information. The information provided in this parameter overrides the default HTML error page that is returned to the client. This option is valid only for actions of type `simulate` or `ban`.  |
|`action.response.content_type`|String|The `content-type` of the body that you want to return. Supported values are `text/plain`, `text/xml`, and `application/json`.|
|`action.response.body`|String|The body of the reponse that you want to return to the client. The information must match the `action.response.content_type` that you specified. The value can have a maximum length of 1024.|
|`disabled`|Boolean|If set to **true**, rate limiting is disabled for the domain.|
|`description`|String|The description for your rate limiting rule. |
|`correlate`|List of NAT-based rate limits|If provided, NAT-based rate limiting is enabled.|
|`correlate.by`|String|If set to `nat`, NAT-based rate limiting is enabled.|
|`bypass`|List of bypass criteria|A list of key-value pairs that, when matched, allow the rate limiting rule to be ignored.  |
|`bypass.name`|String|The name of the key that you want to apply. Supported values are `url`. |
|`bypass.value`|String|The value of the key that you want to match. When `bypass.name` is set to `url`, `bypass.value` contains the URL that you want to exclude from the rate limiting rule. |



