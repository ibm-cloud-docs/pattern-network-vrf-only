---

copyright:
  years: 2023
lastupdated: "2023-12-26"

subcollection: pattern-sap-on-vpc

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Resiliency design
{: #resiliency-design}


## Direct Link

Direct Link resiliency can be designed into a deployment to ensure availability. Most network resiliency is enhanced by multiple paths and dynamic routing.

The primary consideration is the level of risk a company is willing to take vs the financial cost budgeted for the deployment. Resiliency increases as you layer on services in a single point of presence (PoP) from a Direct Link Dedicated connection to two Direct link Connect connections.

1.  Direct Link Dedicated – is a single physical cross connect, port, and switch.
2.  Direct Link Connect – is a single virtual SDN connection that rides on top of redundant physical hardware.
3.  (2) Direct Link Dedicated – provides 2 physical cross connects, ports, and switches.
4.  (2) Direct Link Connects – provides 2 virtual SDN connections that ride on top of redundant physical hardware.

Additionally, consider if there is an existing relationship with a telecommunication or cloud exchange provider that can be leveraged.

Direct Link Connect considerations include:

-   Provided by a network cloud exchange provider.
-   Provides SDN over physical hardware – allows for physical hardware to be removed for maintenance, repair, or failure, while traffic continues to flow over a virtual connection on top of the remaining hardware.
-   A second path can be provisioned by ordering a port from the exchange provider with a second direct link adding an additional layer of resiliency.

Direct Link Dedicated considerations include:

-   Provided by a telecommunication provider.
-   Physical hardware – allows for a single circuit to a meet-me room with a physical cross connect cable into a designated port. Traffic will not continue over the physical cable if the cable, hardware, or port becomes unavailable.
-   A second path can be provisioned leveraging the same or different telecommunications provider with a higher BGP prepend allowing for dynamic routing in the event of a physical failure or scheduled maintenance activity.

Bidirectional Forwarding Detection (BFD) can be enabled on both Direct Link Dedicated and Direct Link Connect to detect network failures quicker, effectively reducing the time it takes to failover to a secondary path.

Consider the cost implications when ordering Direct Link. The secondary path and the connection to a second region can be ordered as metered instead of unmetered if the primary connection is going to be used for most of the user traffic saving on the monthly fee and still allowing for traffic diversity in the event of a primary circuit failure.

Considerations for resilience should also include gateway and firewall appliances and GRE tunnel configurations to eliminate as many single points of failure as possible.

Learn more about Direct Link diversity [here](https://cloud.ibm.com/docs/dl?topic=dl-models-for-diversity-and-redundancy-in-direct-link).

## Multi-Region Deployment

In the multi-region deployment pattern, a second region should be chosen that has transit gateway and VPC services available to highlight the complimentary services that can be leveraged to provide additional functionality from these locations.

The multi-region deployment:

-   Provides a second location in a separate geographical region that is used as a disaster recovery location providing even more resiliency in the event of a regional failure.
-   Choosing a region that has transit gateway and VPC services available also enables the use of additional “complimentary VPC services” such as Cloud DNS services, VPC Virtual Private Endpoint (VPE), IBM Cloud Monitoring, IBM Log Analysis, and IBM Activity Tracker to further enhance the cloud environment.

![](image/ce9cb48144e74ea7416587ee47d839f0.png)

Figure 5. Multi Region View

1.  Optional network path is accomplished through site-to-site VPN terminated on Classic Gateway.
2.  Client network connectivity from on-premises using Direct Link.
3.  Gateway provides routing and security functions.
4.  Virtual jump server for remote administrative access.
5.  GREa tunnel allows BYOIP to be advertised between Classic and on-premises.
6.  GREb tunnel allows BYOIP to be advertised between Classic environments in separate regions.
7.  GREc tunnel allows BYOIP to be advertised between Classic and PowerVS
8.  Private Service Endpoints (SE) allow access to cloud services over the private network.
9.  Proxy Server as an intermediary between on-prem and cloud services.
10. Cloud Internet Services (CIS) to enhance the security, performance, and reliability of internet-facing applications and websites.
11. Virtual Private Endpoint (VPE for VPC) as an alternative to SE & Proxy server allow access to cloud services over the private network.
12. Custom DNS resolver in Classic.
13. DNS services on VPC as an alternative to custom DNS in classic.
14. In region 2 - TGW1 advertises & routes local traffic between Classic, VPC, and PowerVS
15. In region 2 - TGW2 advertises & routes Global traffic between regions for VPC and PowerVS

While deploying your application or infrastructure across multiple regions offers several advantages, including increased fault tolerance, improved latency for geographically distributed users, and potential cost optimization it also introduces additional complexities that need careful consideration.

Here are some key factors to think about when planning a multi-region deployment:

-   **Cost:** While multi-region deployments can improve service availability, they also incur additional costs associated with managing infrastructure and data across multiple regions. This includes costs for resources (compute, storage, network) in each region, as well as potential egress charges for data transfer between regions. Consider closely the required number of network paths to each region and if metered or unmetered billing best meets the business need.
-   **Data Management:** Deciding where and how to replicate your data across regions is crucial. Consider factors like data consistency, latency, and regulatory compliance.
-   **Latency:** Replicating data across large geographic distances can introduce latency. This can impact performance.
-   **Compliance:** Data privacy regulations may restrict where data can be stored and replicated.
-   **Network Connectivity:** Ensuring reliable and secure public and private communication between resources in different regions is essential. This involves considerations like:
    -   **Network latency:** Higher latency between regions can slow down communication and impact user experience.
    -   **Bandwidth:** Depending on the amount of data transfer between regions, you may need to provision adequate bandwidth to avoid bottlenecks.
    -   **Security:** Implement robust security measures to protect data transfer between regions, including encryption and access controls.
-   **Disaster Recovery:** A multi-region deployment can significantly enhance disaster recovery capabilities by providing geographically separated environments. It is important to plan for how to failover and synchronize data to a secondary region in case of an outage. Creating accurate run books and regularly testing failover action plans are critical to a successful recovery to meet recovery point objectives (RPO) and recovery time objectives (RTO).
-   **Deployment and Management:** Deploying and managing infrastructure and applications across multiple regions can be complex. Consider using tools and automation to streamline these processes. Additionally, factor in the increased operational overhead associated with managing resources in different regions.
