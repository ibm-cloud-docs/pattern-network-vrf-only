---

copyright:
  years: 2024
lastupdated: "2024-04-10"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for networking
{: #networking-architecture}

The following are network architecture decisions for the hybrid cloud network for classic infrastructure disaster recovery pattern.

## Architecture decisions for enterprise connectivity
{: #ad-enterprise-connectivity}

| Architecture decision | Requirement                                                                               |Options                                                                                   | Decision                                     | Rationale                                                         |
|---------------------------|------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------|
| Management connectivity   | Provide secure, encrypted connectivity to the cloud’s private network for management purposes. | - SSL VPN  \n - IPsec VPN  \n - Site-to-Site VPN on Gateway appliance in classic | Site-to-Site VPN on Gateway appliance in classic | Secure and suitable for production-level performance                  |
| Enterprise connectivity   | Provide connectivity between client enterprise and {{site.data.keyword.cloud_notm}}.                                  | - {{site.data.keyword.dl_short}} Connect  \n - {{site.data.keyword.dl_short}} Dedicated                                   | {{site.data.keyword.dl_short}} Connect                              | cost effective, quicker deployment time, and supports hybrid and multi-cloud deployment strategies. |
{: caption="Table 1. Classic Data Center network enterprise connectivity architecture decisions"}

## Architecture decisions for Bring Your Own IP and Edge gateways
{: #ad-byoip-gateway}

| Architecture decision | Requirement                                                                          | Options                                                                                                                 | Decision                                                                                                               | Rationale                        |
|---------------------------|------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| Bring Your Own IP (BYOIP) approach            | Provide capability for BYOIP to {{site.data.keyword.cloud_notm}}.                           | Generic Routing Encapsulation (GRE) tunnel                                                                                  | GRE tunnel                                                                                 | Allows BYOIP routes to be advertised |
| Edge gateways             | Capability to provide edge routing services, firewall, and tunnel (VPN, GRE) termination. | Gateway Appliance in Classic  \n - {{site.data.keyword.vsrx_full}}  \n - {{site.data.keyword.vra}}  \n - FortiGate  \n - BYOG (Checkpoint, Cisco, Palo Alto) | Select based on required [features](/docs/fortigate-10g?topic=fortigate-10g-exploring-firewalls&_ga=2.226674782.2123413376.1603312051-1873021910.1602082701) and client preferences | Client preference                    |
{: caption="Table 2. Classic Data Center network BYOIP and Edge Gateway architecture decisions"}

## Architecture decisions for network segmentation and isolation
{: #ad-network-segmentation}

| Architecture decision          | Requirement                                        | Options                       | Decision                      | Rationale                                 |
|------------------------------------|--------------------------------------------------------|-----------------------------------|-----------------------------------|-----------------------------------------------|
| Network segmentation and isolation | Ability to provide network isolation across workloads. | VLANs, subnets, and security groups | VLANs, subnets, and security groups | Allows for segmentation and network isolation |
{: caption="Table 3. Classic Data Center network segmentation and isolation architecture decisions"}

### Architecture decisions for cloud native connectivity
{: #ad-cloud-native}

| Architecture decision                     | Requirement                             | Options                                                                      | Decision                    | Rationale                                                                                  |
|-----------------------------------------------|---------------------------------------------|----------------------------------------------------------------------------------|---------------------------------|------------------------------------------------------------------------------------------------|
| Cloud Native Connectivity to cloud services | Provide secure connection to cloud services | - Private Cloud service endpoints  \n - Public Cloud service endpoints | Private Cloud service endpoints | Provides private connectivity to cloud services, enhanced security, and cost efficiency |
{: caption="Table 4. Classic Data Center network cloud native connectivity architecture decisions"}

## Architecture decisions for load balancing
{: #ad-load-balancing}

| Architecture decision | Requirement                                                                                                            | Options                                                                      | Decision                  | Rationale                                                                                                                                           |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Global load balancing     | Load balancing over the public network across two regions if there's an outage (DR) for failover to the other region. | - {{site.data.keyword.cis_short}}   \n - {{site.data.keyword.vpx_full}} \n - DNS | {{site.data.keyword.cis_short}} | Provides a cost-effective solution and offers extra security features                                                                          |
| Load balancing: Public   | Load-balancing workloads across multiple workload instances or zones over the public network.                              | - {{site.data.keyword.loadbalancer_full}}  \n - {{site.data.keyword.vpx_full}}                   | {{site.data.keyword.loadbalancer_full}}       | Provides a wide range of load-balancing functions for both public and private traffic cost effectively                                                     |
| Load balancing: Private  | Load balancing workloads across multiple workload instances or zones over the private network.                             | - {{site.data.keyword.loadbalancer_full}}  \n - {{site.data.keyword.vpx_full}}                   | {{site.data.keyword.loadbalancer_full}}       | - {{site.data.keyword.loadbalancer_full}} meets small to midsize, low complexity requirement. \n - {{site.data.keyword.vpx_full}} meets large complex load balancer needs |
{: caption="Table 5. Classic Data Center network load-balancing architecture decisions"}

## Architecture decisions for domain name system
{: #ad-dns}

| Architecture decision | Requirement                                                                                 | Options                                                                                                                | Decision         | Rationale                                                                                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Public DNS                | Provide DNS resolution to support the use of hostnames instead of IP addresses for applications | - DNS through the cloud portal  \n - {{site.data.keyword.cis_short}}  \n - Third-party provider  \n - Custom DNS on VSI | {{site.data.keyword.dns_short}} through the cloud portal | Cost-effective and reliable                                                                                                           |
| Private DNS               | Provide DNS resolution within the {{site.data.keyword.cloud_notm}} private network                                       | - Custom DNS on VSI  \n - DNS on Gateway appliance  \n - {{site.data.keyword.dns_short}} in VPC                                  | Custom DNS on VSI    | - Custom DNS on VSI can handle the most complex DNS needs. \n - When VPC service is available, the preferred approach is {{site.data.keyword.dns_short}} in VPC. |
{: caption="Table 6. Classic Data Center network Domain Name System architecture decisions"}
