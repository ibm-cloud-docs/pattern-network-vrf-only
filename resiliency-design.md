---

copyright:
  years: 2024
lastupdated: "2024-04-11"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Resiliency design
{: #resiliency-design}

The following are resiliency design considerations for the hybrid cloud network for classic infrastructure disaster recovery pattern.

## Direct Link
{: #direct-link}

{{site.data.keyword.dl_short}} resiliency can be designed into a deployment to ensure availability. Most network resiliency is enhanced with multiple paths and dynamic routing.

The primary consideration is the level of risk a company is willing to take vs the financial cost budgeted for the deployment. Resiliency increases as you layer on services in a single point of presence (PoP) from a {{site.data.keyword.dl_short}} Dedicated connection to two {{site.data.keyword.dl_short}} connect connections.

1. {{site.data.keyword.dl_short}} Dedicated: A single physical cross-connect, port, and switch.
2. {{site.data.keyword.dl_short}} Connect: A single virtual SDN connection from an exchange provider.
3. (2) {{site.data.keyword.dl_short}} Dedicated: Provides two physical ports and cross connects, which can be deployed on separate switches to ensure resiliency.
4. (2) {{site.data.keyword.dl_short}} Connects: Provides two virtual SDN connections from an exchange provider, which can be provisioned on separate switches to ensure resiliency.

Also, consider whether there is an existing relationship with a telecommunication or cloud exchange provider that can be used.

{{site.data.keyword.dl_short}} Connect considerations include:

- Provided by a network cloud exchange provider.
- Provides SDN over physical hardware for a more cost-effective solution with a short deployment timeline.
- A second path can be provisioned by ordering a second {{site.data.keyword.dl_short}} connect port that is provisioned on a separate switch from the exchange provider for an extra layer of resiliency.

{{site.data.keyword.dl_short}} Dedicated considerations include:

- Provided by a telecommunication provider.
- Physical hardware – allows for a single circuit to a meet-me room with a physical cross-connect cable into a designated port. Traffic will not continue over the physical cable if the cable, hardware, or port is unavailable.
- A second path can be provisioned using the same or different telecommunications provider with a higher BGP prepend allowing for dynamic routing if there is a physical failure or scheduled maintenance activity.

Bidirectional Forwarding Detection (BFD) can be enabled on both {{site.data.keyword.dl_short}} Dedicated and {{site.data.keyword.dl_short}} Connect to detect network failures quicker, effectively reducing the time that it takes to fail over to a secondary path. Consider the cost implications when ordering a {{site.data.keyword.dl_short}}. The secondary path and the connection to a second region can be ordered as metered instead of unmetered if the primary connection is going to be used for most of the user traffic, which reduces the monthly fee and allows for traffic diversity if there is a primary circuit failure. Resilience considerations should include gateway or firewall appliances and GRE tunnel configurations to eliminate as many single points of failure as possible.
This pattern includes two {{site.data.keyword.dl_short}} connect circuits to meet the resiliency requires with a cost effective, flexible, and quick deployable solution. For more information, see [Models for diversity in Direct Link](/docs/dl?topic=dl-models-for-diversity-and-redundancy-in-direct-link).

## Multi-region deployment
{: #multiregion-deployment}

In the multi-region deployment pattern, a second region is chosen that has Transit Gateway and VPC services available to highlight the complimentary services that can be used to provide extra functionality from these locations.

The multi-region deployment:

- Provides for a second location in a separate geographical region that is used as a disaster recovery location, which provides more resiliency if there is a regional failure.
- Choosing a region that has Transit Gateway and VPC services available enables the use of complimentary VPC services such as {{site.data.keyword.dns_full_notm}}, {{site.data.keyword.vpe_full}}, {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, and {{site.data.keyword.cloudaccesstraillong_notm}} to further enhance the cloud environment.

While deploying the application or infrastructure across multiple regions offers several advantages, including increased fault tolerance, improved latency for geographically distributed users, and potential cost optimization, it also introduces extra complexities that need careful consideration.

Key factors to consider when planning a multi-region deployment:

- Cost: While multi-region deployments can improve service availability, they also incur extra costs associated with managing infrastructure and data across multiple regions. The financial impact includes costs for resources (compute, storage, network) in each region, as well as potential egress charges for data transfer between regions. Closely consider the required number of network paths to each region and if metered or unmetered billing best meets the business need.
- Data management: Deciding where and how to replicate your data across regions is crucial. Consider factors like data consistency, latency, and regulatory compliance.
- Latency: Replicating data across large geographic distances can introduce latency and can impact performance.
- Compliance: Data privacy regulations might restrict where data can be stored and replicated.
- Network connectivity: Ensuring reliable and secure public and private communication between resources in different regions is essential and involves considerations like:
    - Network latency: Higher latency between regions can slow down communication and impact user experience.
    - Bandwidth: Provision adequate bandwidth to avoid bottlenecks based on the amount of data that is transfering between regions and the time allotment of transfer window.
    - Security: Implement robust security measures to protect data transfer between regions, including encryption and access controls.
- Disaster recovery: A multi-region deployment can significantly enhance disaster recovery capabilities by providing geographically separated environments. It is important to plan for how to fail over and synchronize data to a secondary region if there is an outage. Creating accurate run books and regularly testing failover action plans are critical to a successful recovery to meet recovery point objectives (RPO) and recovery time objectives (RTO).
- Deployment and management: Deploying and managing infrastructure and applications across multiple regions can be complex. Consider tools and automation to streamline the processes. Also, factor in the increased operational overhead associated with managing resources in different regions.
