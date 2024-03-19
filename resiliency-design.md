---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Resiliency design
{: #resiliency-design}


## Direct Link
{: #direct-link}

{{site.data.keyword.dl_short}} resiliency can be designed into a deployment to ensure availability. Most network resiliency is enhanced by multiple paths and dynamic routing.

The primary consideration is the level of risk a company is willing to take vs the financial cost budgeted for the deployment. Resiliency increases as you layer on services in a single point of presence (PoP) from a {{site.data.keyword.dl_short}} Dedicated connection to two {{site.data.keyword.dl_short}} Connect connections.

1.  {{site.data.keyword.dl_short}} Dedicated – is a single physical cross-connect, port, and switch.
2.  {{site.data.keyword.dl_short}} Connect – is a single virtual SDN overlay connection that rides on redundant physical hardware.
3.  (2) {{site.data.keyword.dl_short}} Dedicated – provides 2 physical cross connects, ports, and switches.
4.  (2) {{site.data.keyword.dl_short}} Connects – provides 2 virtual SDN overlay connections that ride on redundant physical hardware.

Additionally, consider whether there is an existing relationship with a telecommunication or cloud exchange provider that can be leveraged.

{{site.data.keyword.dl_short}} Connect considerations include:

-   Provided by a network cloud exchange provider.
-   Provides SDN over physical hardware – allows for physical hardware to be removed for maintenance, repair, or failure, while traffic continues to flow over a virtual connection on the remaining hardware.
-   A second path can be provisioned by ordering a port from the exchange provider with a second {{site.data.keyword.dl_short}} providing an additional layer of resiliency.

{{site.data.keyword.dl_short}} Dedicated considerations include:

-   Provided by a telecommunication provider.
-   Physical hardware – allows for a single circuit to a meet-me room with a physical cross-connect cable into a designated port. Traffic will not continue over the physical cable if the cable, hardware, or port is unavailable.
-   A second path can be provisioned leveraging the same or different telecommunications provider with a higher BGP prepend allowing for dynamic routing if there is a physical failure or scheduled maintenance activity.

Bidirectional Forwarding Detection (BFD) can be enabled on both {{site.data.keyword.dl_short}} Dedicated and {{site.data.keyword.dl_short}} Connect to detect network failures quicker, effectively reducing the time that it takes to fail over to a secondary path.

Consider the cost implications when ordering a {{site.data.keyword.dl_short}}. The secondary path and the connection to a second region can be ordered as metered instead of unmetered if the primary connection is going to be used for most of the user traffic, which reduces the monthly fee and allows for traffic diversity if there is a primary circuit failure.

Resilience considerations also include gateway and firewall appliances and GRE tunnel configurations to eliminate as many single points of failure as possible.

Learn more about {{site.data.keyword.dl_short}} diversity [here](/docs/dl?topic=dl-models-for-diversity-and-redundancy-in-direct-link).

## Multi-Region Deployment
{: #multiregion-deployment}

In the multi-region deployment pattern, a second region is chosen that has transit gateway and VPC services available to highlight the complimentary services that can be leveraged to provide extra functionality from these locations.

The multi-region deployment:

-   Provides for a second location in a separate geographical region that is used as a disaster recovery location which provides more resiliency if there is a regional failure.
-   Choosing a region that has transit gateway and VPC services available enables the use of “complimentary VPC services” such as {{site.data.keyword.dns_full_notm}}, {{site.data.keyword.vpe_full}}, {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, and {{site.data.keyword.cloudaccesstraillong_notm}} to further enhance the cloud environment.

![Illustrates a detailed network and component architecture for a
multi-region non-TGW solution architecture](cross-region-view.svg){: caption="Figure 5. Multi-Region View" caption-side="bottom"}
1.  Optional network path is accomplished through site-to-site VPN terminated on Classic Gateway.
2.  Client network connectivity from on-premises using {{site.data.keyword.dl_short}}.
3.  The gateway provides routing and security functions.
4.  Virtual jump server for remote administrative access.
5.  GREa tunnel allows BYOIP to be advertised between Classic and on-premises.
6.  GREb tunnel allows BYOIP to be advertised between Classic environments in separate regions.
7.  GREc tunnel allows BYOIP to be advertised between Classic and PowerVS
8.  Private Service Endpoints (SE) allow access to cloud services over the private network.
9.  Proxy Server as an intermediary between on-prem and cloud services.
10. Cloud Internet Services (CIS) to enhance the security, performance, and reliability of internet-facing applications and websites.
11. {{site.data.keyword.vpe_full}} as an alternative to SE and Proxy server allow access to cloud services over the private network.
12. Custom DNS resolver in Classic.
13. DNS services on VPC as an alternative to custom DNS in classic.
14. In region 2 - TGW1 advertises and routes local traffic between Classic, VPC, and PowerVS
15. In region 2 - TGW2 advertises and routes Global traffic between regions for VPC and PowerVS

While deploying the application or infrastructure across multiple regions offers several advantages, including increased fault tolerance, improved latency for geographically distributed users, and potential cost optimization it also introduces extra complexities that need careful consideration.

Key factors to consider when planning a multi-region deployment:

-   **Cost:** While multi-region deployments can improve service availability, they also incur extra costs associated with managing infrastructure and data across multiple regions. The financial impact include costs for resources (compute, storage, network) in each region, as well as potential egress charges for data transfer between regions. Closely consider the required number of network paths to each region and if metered or unmetered billing best meets the business need.
-   **Data Management:** Deciding where and how to replicate your data across regions is crucial. Consider factors like data consistency, latency, and regulatory compliance.
-   **Latency:** Replicating data across large geographic distances can introduce latency and can impact performance.
-   **Compliance:** Data privacy regulations might restrict where data can be stored and replicated.
-   **Network Connectivity:** Ensuring reliable and secure public and private communication between resources in different regions is essential and involves considerations like:
    -   **Network latency:** Higher latency between regions can slow down communication and impact user experience.
    -   **Bandwidth:** Provision adequate bandwidth to avoid bottlenecks based on the amount of data transfing between regions and time allotment of transfer window.
    -   **Security:** Implement robust security measures to protect data transfer between regions, including encryption and access controls.
-   **Disaster Recovery:** A multi-region deployment can significantly enhance disaster recovery capabilities by providing geographically separated environments. It is important to plan for how to fail over and synchronize data to a secondary region if there is an outage. Creating accurate run books and regularly testing failover action plans are critical to a successful recovery to meet recovery point objectives (RPO) and recovery time objectives (RTO).
-   **Deployment and Management:** Deploying and managing infrastructure and applications across multiple regions can be complex. Consider tools and automation to streamline the processe. Additionally, factor in the increased operational overhead associated with managing resources in different regions.
