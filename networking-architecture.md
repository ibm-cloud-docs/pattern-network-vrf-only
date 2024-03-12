---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-service

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for networking
{: #networking-architecture}

The following are network architecture decisions for the Network architecture for data centers without a Transit Gateway service pattern.

## Architecture decisions for enterprise connectivity
{: #ad-enterprise-connectivity}

| **Architecture decision** | **Requirement**                                                                                | **Options**                                                                                   | **Decision**                                     | **Rationale**                                                         |
|---------------------------|------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------|
| Management connectivity   | Provide secure, encrypted connectivity to the cloud’s private network for management purposes. | - SSL VPN  \n - IPsec VPN  \n - Site-to-Site VPN on Gateway appliance in Classic | Site-to-Site VPN on Gateway appliance in Classic | Secure and suitable for production-level performance                  |
| Enterprise connectivity   | Provide connectivity between client enterprise and IBM Cloud.                                  | - Direct Link Connect  \n - Direct Link Dedicated                                   | Direct Link Connect                              | Lower cost, quicker deployment time, SDN provides layer of resiliency |
{: caption="Table 7. non-TGW network enterprise connectivity architecture decisions"}

## Architecture decisions for BYOIP and Edge Gateways
{: #ad-byoip-gateway}

| **Architecture decision** | **Requirement**                                                                          | **Options**                                                                                                                 | **Decision**                                                                                                               | **Rationale**                        |
|---------------------------|------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| BYOIP approach            | Provide capability for bring your own IP (BYOIP) to IBM Cloud.                           | GRE (Generic Routing Encapsulation) Tunnel                                                                                  | GRE (Generic Routing Encapsulation) Tunnel                                                                                 | Allows BYOIP routes to be advertised |
| Edge Gateways             | Capability to provide edge routing services, firewall and tunnel (VPN, GRE) termination. | **Gateway Appliance in Classic**  \n - Juniper vSRX  \n - Virtual Router Appliance  \n - FortiGate  \n - BYOG (Checkpoint, Fortinet, Palo Alto) | Select based on required [features](/docs/vsrx?topic=vsrx-exploring-firewalls) and client preferences | Client preference                    |
{: caption="Table 8. non-TGW network BYOIP and Edge Gateway architecture decisions"}

## Architecture decisions for network segmentation and isolation
{: #ad-network-segmentation}

| **Architecture decision**          | **Requirement**                                        | **Options**                       | **Decision**                      | **Rationale**                                 |
|------------------------------------|--------------------------------------------------------|-----------------------------------|-----------------------------------|-----------------------------------------------|
| Network segmentation and isolation | Ability to provide network isolation across workloads. | VLANs, Subnets, & Security Groups | VLANs, Subnets, & Security Groups | Allows for segmentation and network isolation |
{: caption="Table 9. non-TGW network segmentation and isolation architecture decisions"}

### Architecture decisions for cloud native connectivity
{: #ad-cloud-native}

| **Architecture decision**                     | **Requirement**                             | **Options**                                                                      | **Decision**                    | **Rationale**                                                                                  |
|-----------------------------------------------|---------------------------------------------|----------------------------------------------------------------------------------|---------------------------------|------------------------------------------------------------------------------------------------|
| Cloud Native Connectivity (to cloud services) | Provide secure connection to Cloud Services | - Private Cloud Service endpoints  \n - Public Cloud Service Endpoints | Private Cloud Service endpoints | Provides private connectivity to cloud services offering enhanced security and cost efficiency |
{: caption="Table 10. non-TGW network cloud native connectivity architecture decisions"}

## Architecture decisions for load balancing
{: #ad-load-balancing}

| **Architecture decision** | **Requirement**                                                                                                            | **Options**                                                                      | **Decision**                  | **Rationale**                                                                                                                                           |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Global Load balancing     | Load balancing over the public network across two regions in the event of an outage (DR) for failover to the other region. | - Cloud Internet Service (CIS)  \n - Citrix Netscaler VPX  \n - DNS | Cloud internet Services (CIS) | Provides a cost-effective solution while offering additional security features                                                                          |
| Load balancing (public)   | Load balancing workloads across multiple workload instances or zones over the public network.                              | - IBM Cloud Load Balancer  \n - Citrix Netscaler VPX                   | IBM Cloud Load Balancer       | Provides wide range of load balancing function for both public and private traffic cost effectively                                                     |
| Load balancing (private)  | Load balancing workloads across multiple workload instances or zones over the private network.                             | - IBM Cloud Load Balancer  \n - Citrix Netscaler VPX                   | IBM Cloud Load Balancer       | - Cloud Load Balancer meets small to mid-size, low complexity requirement.  \n - Citrix Netscaler VPX meets large complex load balancer needs |
{: caption="Table 11. non-TGW network load balancing architecture decisions"}

## Architecture decisions for domain name system
{: #ad-dns}

| **Architecture decision** | **Requirement**                                                                                 | **Options**                                                                                                                | **Decision**         | **Rationale**                                                                                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Public DNS                | Provide DNS resolution to support the use of hostnames instead of IP addresses for applications | - DNS via cloud portal  \n - Cloud Internet Services (CIS)  \n - Third Party provider  \n - Custom DNS on VSI | DNS via Cloud portal | Cost effective and reliable                                                                                                           |
| Private DNS               | Provide DNS resolution within IBM Cloud's private network                                       | - Custom DNS on VSI  \n - DNS on Gateway appliance  \n - DNS services in VPC                                  | Custom DNS on VSI    | - Can handle the most complex DNS needs.  \n - When VPC service is available, the preferred approach is DNS services in VPC. |
{: caption="Table 12. non-TGW network Domain Name System architecture decisions"}
