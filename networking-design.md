---

copyright:
  years: 2023
lastupdated: "2023-12-26"

subcollection: pattern-sap-on-vpc

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Network design
{: #network-design}

## Virtual Private Network (VPN)

Implementing a site-to-site VPN offers secure connectivity between your on-premises networks and remote locations but comes with important considerations before deployment.

-   **Bandwidth:** Analyze expected data transfer needs and choose a VPN solution with sufficient bandwidth capacity.
-   **Bandwidth Consumption:** Depending on your service provider, data transfer through the VPN may incur usage charges.
-   **Latency:** Consider the physical distance between connected sites and potential latency impact on performance-sensitive applications.
-   **Encryption Overhead:** Encryption adds processing overhead. Evaluate the impact on performance, especially for real-time applications.
-   **Hardware/Software:** Factor in the cost of VPN hardware, software licenses, and any additional infrastructure needed.
-   **Scalability:** Ensure your chosen VPN solution can accommodate future growth in network size and data transfer needs.
-   **High Availability:** Implement redundant connections or failover mechanisms to ensure connectivity even during outages.
-   **Compliance:** If your organization operates under specific compliance regulations, ensure the VPN solution meets those requirements.
-   **Management:** Managing the VPN infrastructure can require dedicated personnel or managed services, adding to the cost.
-   **Technical Expertise:** Implementing and maintaining a VPN may require technical expertise. Assess your internal IT capabilities or consider external support options.

## Gateway Appliance and Firewall

Choosing the right firewall is crucial for safeguarding your network. Here are some key considerations to guide you:

-   **Basic vs. Next-Generation:** Basic firewalls offer packet filtering, while Next-Generation Firewalls (NGFWs) provide deeper inspection, application control, intrusion prevention, and more. Choose based on your security needs and complexity.
-   **Threat Detection and Prevention:** Consider features like malware detection, intrusion prevention, and sandboxing to address evolving threats.
-   **VPN Capabilities:** If secure remote access is needed, ensure the firewall supports VPN protocols suitable to requirements.
-   **Content Filtering:** Control access to specific websites or categories based on user groups or policies.
-   **Throughput:** Ensure the firewall can handle your internet traffic volume without bottlenecks.
-   **Latency:** Evaluate potential latency impact, especially for real-time applications like video conferencing.
-   **Scalability:** Consider future network growth and choose a firewall that can accommodate increased traffic and users.
-   **Management Interface:** Choose a web-based, command-line, or physical interface that aligns with your IT team's expertise and preference.
-   **Logging and Reporting:** Robust logging and reporting capabilities are essential for monitoring activity and detecting security incidents.
-   **Updates and Support:** Opt for a firewall with regular software updates and reliable technical support from the vendor.
-   **Budget:** Firewalls range in price depending on features and capabilities. Determine budget and prioritize accordingly.
-   **Compatibility:** Ensure the firewall is compatible with existing network hardware and software.
-   **Security Certifications:** Look for firewalls that comply with recognized security standards like NSS Labs or Common Criteria.
-   **Brand Reputation:** Choose a reputable brand with a proven track record and positive customer reviews.
-   **Virtual Appliance vs. Hardware Firewalls:** Evaluate cloud-based options for flexibility and scalability but consider throughput and port sizes for latency and bottlenecks.
-   **Single vs. High Availability:** Consider single points of failure and service level requirements.

Explore and compare [gateway options](https://cloud.ibm.com/docs/vsrx?topic=vsrx-exploring-firewalls) available in IBM Cloud.

## Generic Routing Encapsulation (GRE) tunnels

GRE tunnels support the Bring Your Own IP (BYOIP) requirement.

![](image/1b8db80bf49e684e0617a22d64767e9b.png)

Figure 3. non-TGW GRE Encapsulation

1.  Client network connectivity from on-premises is accomplished through Direct Link access.
2.  A gateway is deployed in Classic which provides routing and security functions.
3.  GRE tunnel is created between the Gateway and a customer router.
4.  192.168.xx.xx (BYOIP) packets are encapsulated by 10.1.x.x (IBM assigned IP) which is routed through the BCR over the GRE and un-encapsulated on the other side.

IBM Cloud Direct Link does not offer BYOIP on the private network. The IBM Cloud backbone advertises the customer’s available routes assigned by IBM to their remote networks. Traffic with a destination IP address that is not assigned by IBM Cloud (10.0.0.0/8) is dropped by IBM Cloud. Traffic can be encapsulated between the remote client network and the IBM Cloud network for non-assigned IBM Cloud addresses by establishing GRE Tunnels between the gateway and a customer router to allow non-IBM assigned IP addresses to be passed back to the on-premises environment.

A second GRE is required between the Classic Gateway and PowerVS when non-assigned IBM Cloud addresses are used in the PowerVS environment.

For Gateways in separate regions, a third GRE is used to share non-assigned IBM Cloud addresses between Classic Gateways.

**When resiliency is required, GREs can be configured on two devices in an HA pair to eliminate single points of failure.**

Explore IBM Cloud public & private IP ranges [**here**](https://cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-ibm-cloud-ip-ranges)**.**

## Enterprise Connectivity

There are two Direct Link options available: Direct Link Dedicated and Direct Link Connect.

Enterprise connectivity considerations include:

-   Bandwidth requirements: There are a variety of bandwidth options available in [Direct Link](https://cloud.ibm.com/docs/dl?topic=dl-get-started-with-ibm-cloud-dl). Direct Link Dedicated offers higher bandwidth potential.
-   Security needs: If isolation is needed, Direct Link Dedicated provides the highest isolation for sensitive data.
-   Location and existing infrastructure: Dedicated requires colocation or dedicated circuits. [Provider location](https://cloud.ibm.com/docs/dl?topic=dl-locations#connect-locations) will also need to be considered.
-   Cost: Direct Link Connect is generally more cost effective, especially for lower bandwidth needs.
-   Deployment time: Direct Link Connect can be deployed faster due to pre-established connections.

**To avoid IP address conflicts for classic connections to a direct link, avoid IP address ranges in the 10.0.0.0/14, 10.200.0.0/14, 10.198.0.0/15, and 10.254.0.0/16 blocks for on-prem networks. On-prem routes that overlap are dropped.**

Direct Link Dedicated is ideal for:

-   Organizations with colocation facilities near IBM Cloud PoPs or data centers.
-   Network service providers delivering circuits to customers or other data centers.
-   Highly sensitive data or mission-critical applications requiring maximum security and performance.
-   Organizations that need fine-grained control over routing and traffic management. Direct link dedicated is ideal for moderate bandwidth needs, multi-cloud or hybrid cloud solution strategies.

Direct Link Dedicated use cases include:

-   Banking and finance: Transferring sensitive financial data or running critical trading applications with maximum security and performance.
-   Healthcare: Managing protected health information (PHI) with strict compliance requirements.
-   Government and defense: Handling classified data or mission-critical systems demanding the highest security levels.
-   Research and development: Transferring large datasets for research or running high-performance computing (HPC) workloads.
-   Media and entertainment: Streaming high-quality video content or managing large media files with low latency and high bandwidth.

For Direct Link Dedicated locations please visit: [https://cloud.ibm.com/docs/dl?topic=dl-locations\#dedicated-locations](https://cloud.ibm.com/docs/dl?topic=dl-locations#dedicated-locations)

Direct Link Connect is ideal for:

-   Organizations with diverse connectivity requirements (multi-cloud, hybrid cloud).
-   Organizations not located near IBM Cloud PoPs or need lower bandwidth.
-   Organizations seeking a more affordable private connection compared to Dedicated.
-   Simple and quick deployment due to pre-established connections in data centers.

Direct Link Connect use cases include:

-   Hybrid cloud architectures: Connecting on-premises infrastructure to IBM Cloud resources for seamless workload distribution and data sharing.
-   Multi-cloud deployments: Linking IBM Cloud with other cloud providers for workload flexibility and disaster recovery.
-   Remote branch offices: Connecting geographically dispersed offices to IBM Cloud for centralized applications and data access.
-   Software-as-a-Service (SaaS) applications: Accessing SaaS applications hosted on IBM Cloud with improved performance and security.

For Direct Link Connect location and providers please visit: [https://cloud.ibm.com/docs/dl?topic=dl-locations\#connect-locations](https://cloud.ibm.com/docs/dl?topic=dl-locations#connect-locations)

## Load Balancing

### Local Load balancing

Load balancing is the process of distributing network traffic efficiently among multiple servers within a single region to optimize and ensure application availability to meet specific service level requirements.

Considerations Include:

-   **Layer-4 Load Balancing for HTTP, HTTPS, and TCP traffic:** Distributes traffic based on IP addresses, ports, and basic metrics like packet arrival time. Works well for simple traffic but lacks deeper application understanding.
-   **Advanced Layer-7 Load Balancing for HTTP and HTTPS traffic:** Analyzes deeper aspects like URLs, headers, and user data. Offers granular routing based on content, user needs, and server capabilities. Ideal for complex applications.
-   **Server Health Checks:** Regularly ping servers to ensure they are healthy and can handle traffic. Unhealthy servers are removed from the pool until they recover.
-   **SSL Offload:** Takes over decryption and encryption of HTTPS traffic, freeing up server resources for application processing. Improves performance and security.
-   **Public (Internet-facing to Private) Load Balancing:** Makes internal applications accessible from the internet through a public IP address and NAT gateway. Provides security by keeping application servers hidden on a private network.
-   **Public to Public (Internet-facing) Load Balancing:** Distributes traffic among multiple public-facing servers for high availability and scalability of websites or services.
-   **Private (Internal) Load Balancing:** Distributes traffic among internal servers on a private network. Improves performance and scalability for internal applications without internet exposure.

IBM Cloud offers two Load balancer options for classic which include: IBM Cloud Load Balancer and Citrix NetScaler VPX appliance.

Explore load balancer feature options [here](https://cloud.ibm.com/docs/citrix-netscaler-vpx?topic=citrix-netscaler-vpx-explore).

Learn more about IBM Cloud Load Balancer [here](https://cloud.ibm.com/docs/loadbalancer-service?topic=loadbalancer-service-about-ibm-cloud-load-balancer)

Learn more about Citrix NetScaler VPX Load Balancer [here](https://cloud.ibm.com/docs/citrix-netscaler-vpx?topic=citrix-netscaler-vpx-about-citrix-netscaler-vpx)

### Third party Load Balancer Appliance

If you have a specific need not offered by any of the load balancer options within the IBM Cloud Catalog, or have an existing relationship established with a third-party vendor, such as F5, you can deploy it within IBM Cloud as a bring your own appliance.

### Global Load Balancing

Global Server Load Balancing (GSLB) is a technique for distributing internet traffic across geographically dispersed servers. It aims to optimize user experience and application performance by directing users to the nearest or most appropriate server based on various factors like latency, server load, and user location and is a valuable component of a disaster recovery strategy.

If Global Server Load Balancing (GSLB) is required, IBM Cloud offers a Global load balancer service through Cloud Internet Services (CIS) which routes traffic to servers across multiple geographic locations to ensure application availability.

Other third-party connectivity providers such as Akamai, network appliances from vendors like F5 and Citrix, as well as Domain Name Service can also be leveraged to meet the requirement for GSLB.

When a user tries to access a website or application, the request goes to the GSLB device. The GSLB device uses various algorithms and real-time data to determine the best server to route the request to. Factors considered include:

-   **User location:** Sending users to the closest server minimizes latency.
-   **Server load:** Distributing traffic among available servers prevents overloading any single server.
-   **Server health:** Avoiding unresponsive or overloaded servers.
-   **Application-specific criteria:** Specific services might require routing based on user type, content availability, etc.

Explore Global Server Load Balancing and other features of IBM Cloud Internet Service [here](https://cloud.ibm.com/docs/cis?topic=cis-about-ibm-cloud-internet-services-cis).

## Private Service Endpoints & Proxy Server

With IBM Cloud service endpoints, you can connect to IBM Cloud services over the IBM Cloud private network instead of the default public network.

Moving these workloads from the public network to the private network offers two advantages:

-   Enhanced Security - Cloud Services are no longer served on an internet routable IP address.
-   Cost effective - There are no billable or metered bandwidth charges on the private network.

In IBM Cloud Classic, Virtual route forwarding (VRF) must be enabled on the account to move routing to a dedicated routing table. With VRF enabled, Private Service Endpoints can be enabled, and Cloud service resources can be provisioned and accessible to the IBM Cloud private network from an IBM assigned IP address in the Classic Account.

Click to learn more on [IBM Cloud VRF](https://cloud.ibm.com/docs/dl?topic=dl-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud).

When the source address is not an IBM assigned IP address a proxy server in classic is used as an intermediary allowing access to cloud services over the Direct Link.

Verify Private Service endpoints are available for your cloud services [here](https://cloud.ibm.com/docs/account?topic=account-vrf-service-endpoint&interface=ui).

Getting started with Service Endpoints [here](https://cloud.ibm.com/docs/account?topic=account-vrf-service-endpoint&interface=ui).

In a multi-region deployment, which is described later in this document, for non-transit gateway locations, often a secondary region which does have transit gateway enabled may be chosen. In these situations, as an alternative to Private Service Endpoints and a proxy server in Classic, configuring a Virtual Private Endpoint (VPE) (complementary VPC Service in the 2nd region) can provide access to IBM Cloud Service over the private network.

Verify cloud services are VPE enabled [here](https://cloud.ibm.com/docs/vpc?topic=vpc-vpe-supported-services).

![](image/c6bb939df68977f16aad7a6f5b2e2f40.png)

Figure 4. non-TGW Service Endpoint access

## Cloud Internet Services (CIS)

Cloud Internet Services (CIS) provides global server load balancing, public domain name services, and public network security features.

Learn more about CIS [here](https://cloud.ibm.com/docs/cis?topic=cis-about-ibm-cloud-internet-services-cis)

## Domain Name Services (DNS)

Domain Name Services provides access to your systems and services via user-friendly domain names rather than IP addresses.

Key considerations are:

-   **Public vs private DNS name resolution:** ability to provide DNS based access to users on the public Internet vs internal DNS-based communication within your private environment.
-   **Integration with other existing DNS systems:** integrating your on-premises DNS service with your cloud environment.

IBM Cloud provides a flexible approach to DNS name resolution for systems hosted in classic environments.

-   **Public DNS name resolution**: translates human-friendly names into computer-readable addresses.
    -   Use the [DNS interface](https://cloud.ibm.com/docs/dns?topic=dns-how-to-use-the-dns-interface) in the IBM Cloud Portal to manage your zones and records. The domain can be hosted by any 3rd party provider. Set the Nameserver (NS) record to the provided IBM Name servers.
    -   Use the DNS function provided with Cloud Internet Services. Domain name control must be delegated to Cloud Internet Service (CIS).
-   **Private DNS name resolution**: allows integration with your on-premises DNS server over private connectivity.
    -   Configure your own DNS services in the classic environment, typical options are:
        -   Install your own custom DNS service on a virtual server.
        -   Configure DNS on your gateway device.
    -   Utilize IBM Cloud DNS Services (complementary VPC Service).
        This Service allows you to provide versatile private name resolution between classic, on-premises and VPC resources by deploying [Custom Resolvers](https://cloud.ibm.com/docs/dns-svcs?topic=dns-svcs-custom-resolver). Learn more about [IBM Cloud DNS Services](https://cloud.ibm.com/docs/dns-svcs?topic=dns-svcs-getting-started).
