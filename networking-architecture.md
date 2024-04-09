---

copyright:
  years: 2024
lastupdated: "2024-04-08"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-service

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for networking
{: #networking-architecture}

The following are network architecture decisions for the network architecture for data centers without a {{site.data.keyword.tg_full_notm}} service pattern.

## Architecture decisions for enterprise connectivity
{: #ad-enterprise-connectivity}

| Architecture decision | Requirement                                                                                | Options                                                                                   | Decision                                     | Rationale                                                         |
|---------------------------|------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------|
| Management connectivity   | Provide secure, encrypted connectivity to the cloud’s private network for management purposes. | - SSL VPN  \n - IPsec VPN  \n - Site-to-Site VPN on Gateway appliance in Classic | Site-to-Site VPN on Gateway appliance in Classic | Secure and suitable for production-level performance                  |
| Enterprise connectivity   | Provide connectivity between client enterprise and {{site.data.keyword.cloud_notm}}.                                  | - {{site.data.keyword.dl_short}} Connect  \n - {{site.data.keyword.dl_short}} Dedicated                                   | {{site.data.keyword.dl_short}}Connect                              | Lower cost, quicker deployment time, SDN provides a layer of resiliency |
{: caption="Table 1. Non-TGW network enterprise connectivity architecture decisions"}

## Architecture decisions for Bring Your Own IP (BYOIP) and edge gateways
{: #ad-byoip-gateway}

| Architecture decision | Requirement                                                                          | Options                                                                                                                 | Decision                                                                                                               | Rationale                        |
|---------------------------|------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| BYOIP approach            | Provide capability for Bring Your Own IP (BYOIP) to {{site.data.keyword.cloud_notm}}.                           | Generic Routing Encapsulation (GRE) tunnel                                                                                  | Generic Routing Encapsulation (GRE) Tunnel                                                                                 | Allows BYOIP routes to be advertised |
| Edge gateways             | Capability to provide edge routing services, firewall, and tunnel (VPN, GRE) termination. | Gateway Appliance in Classic  \n - {{site.data.keyword.vsrx_full}}  \n - {{site.data.keyword.vra}}  \n - FortiGate  \n - Bring Your Own Gateway (BYOG): Checkpoint, Fortinet, and Palo Alto | Select based on the required features. For more information, see [Exploring firewalls](/docs/vsrx?topic=vsrx-exploring-firewalls) and client preferences | Client preference                    |
{: caption="Table 2. Non-TGW network BYOIP and edge gateway architecture decisions"}

## Architecture decisions for network segmentation and isolation
{: #ad-network-segmentation}

| Architecture decision          | Requirement                                        | Options                       | Decision                      | Rationale                                 |
|------------------------------------|--------------------------------------------------------|-----------------------------------|-----------------------------------|-----------------------------------------------|
| Network segmentation and isolation | Ability to provide network isolation across workloads. | VLANs, subnets, and security Groups | VLANs, subnets, and security groups | Allows for segmentation and network isolation |
{: caption="Table 3. Non-TGW network segmentation and isolation architecture decisions"}

### Architecture decisions for cloud native connectivity
{: #ad-cloud-native}

| Architecture decision                     | Requirement                             | Options                                                                      | Decision                    | Rationale                                                                                  |
|-----------------------------------------------|---------------------------------------------|----------------------------------------------------------------------------------|---------------------------------|------------------------------------------------------------------------------------------------|
| Cloud native connectivity to cloud services | Provide secure connection to cloud services | - Private cloud service endpoints  \n - Public cloud service endpoints | Private cloud service endpoints | Provides private connectivity to cloud services offering enhanced security and cost efficiency |
{: caption="Table 4. Non-TGW network cloud native connectivity architecture decisions"}

## Architecture decisions for load balancing
{: #ad-load-balancing}

| Architecture decision | Requirement                                                                                                            | Options                                                                      | Decision                  | Rationale                                                                                                                                           |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Global load balancing     | Load balancing over the public network across two {{site.data.keyword.cis_short}} regions if an outage occurs (Disaster Recovery) for failover to the other region. | -   \n - {{site.data.keyword.vpx_full}} \n - DNS | {{site.data.keyword.cis_short}} | Provides a cost-effective solution while offering additional security features                                                                          |
| Load balancing (public)   | Load-balancing workloads across multiple workload instances or zones over the public network.                              | - {{site.data.keyword.loadbalancer_full}}  \n - {{site.data.keyword.vpx_full}}                   | {{site.data.keyword.loadbalancer_full}}       | Provides a wide range of load-balancing functions for both public and private traffic cost effectively                                                     |
| Load balancing (private)  | Load-balancing workloads across multiple workload instances or zones over the private network.                             | - {{site.data.keyword.loadbalancer_full}}  \n - {{site.data.keyword.vpx_full}}                   | {{site.data.keyword.loadbalancer_full}}       | - {{site.data.keyword.loadbalancer_full}} meets small to midsize, low complexity requirement.  \n - {{site.data.keyword.vpx_full}} meets large complex load balancer needs |
{: caption="Table 5. Non-TGW network load balancing architecture decisions"}

## Architecture decisions for domain name system
{: #ad-dns}

| Architecture decision | Requirement                                                                                 | Options                                                                                                                | Decision         | Rationale                                                                                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Public DNS                | Provide DNS resolution to support the use of hostnames instead of IP addresses for applications | - DNS through the cloud portal  \n - {{site.data.keyword.cis_short}}  \n - Third-Party provider  \n - Custom DNS on VSI | {{site.data.keyword.dns_short}} through the cloud portal | Cost-effective and reliable                                                                                                           |
| Private DNS               | Provide DNS resolution within {{site.data.keyword.cloud_notm}}'s private network                                       | - Custom DNS on VSI  \n - DNS on Gateway appliance  \n - {{site.data.keyword.dns_short}} in VPC                                  | Custom DNS on VSI    | - Can handle the most complex DNS needs. \n - When VPC service is available, the preferred approach is {{site.data.keyword.dns_short}} in VPC. |
{: caption="Table 6. Non-TGW network Domain Name System architecture decisions"}
