---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-service

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Network design
{: #network-design}

## Virtual Private Network (VPN)
{: #vpn}

Implementing a site-to-site VPN offers secure connectivity between your on-premises networks and remote locations but comes with important considerations before deployment.

-   **Bandwidth:** Analyze expected data transfer needs and choose a VPN solution with sufficient bandwidth capacity.
-   **Bandwidth Consumption:** Depending on the service provider, data transfer through the VPN might incur usage charges.
-   **Latency:** Consider the physical distance between connected sites and the potential latency impact on performance-sensitive applications.
-   **Encryption Overhead:** Encryption adds processing overhead. Evaluate the impact on performance, especially for real-time applications.
-   **Hardware and Software:** Factor in the cost of VPN hardware, software licenses, and any additional infrastructure needed.
-   **Scalability:** Ensure the chosen VPN solution can accommodate future growth in network size and data transfer needs.
-   **High Availability:** Implement redundant connections or failover mechanisms to ensure connectivity even during outages.
-   **Compliance:** If your organization operates under specific compliance regulations, ensure that the VPN solution meets those requirements.
-   **Management:** Managing the VPN infrastructure can require dedicated personnel or managed services, adding to the cost.
-   **Technical Expertise:** Implementing and maintaining a VPN might require technical expertise. Assess your internal IT capabilities or consider external support options.

## Gateway Appliance and Firewall
{: #gateway-appliance}

Choosing the right firewall is crucial for safeguarding your network. Review the key consideration guidance below.

-   **Basic versus Next-Generation:** Basic firewalls offer packet filtering, while Next generation Firewalls (NGFWs) provide deeper inspection, application control, intrusion prevention, and more. Choose based on your security needs and complexity.
-   **Threat Detection and Prevention:** Consider features like malware detection, intrusion prevention, and sandboxing to address evolving threats.
-   **VPN Capabilities:** If secure remote access is needed, ensure that the firewall supports VPN protocols suitable to meet the requirements.
-   **Content Filtering:** Control access to specific websites or categories based on user groups or policies.
-   **Throughput:** Ensure that the firewall can meet the internet traffic volume without bottlenecks.
-   **Latency:** Evaluate potential latency impact, especially for real-time applications like video conferencing.
-   **Scalability:** Consider future network growth and choose a firewall that can accommodate increased traffic and users.
-   **Management Interface:** Choose a web-based, command-line, or physical interface that aligns with your IT team's expertise and preference.
-   **Logging and Reporting:** Robust logging and reporting capabilities are essential for monitoring activity and detecting security incidents.
-   **Updates and Support:** Opt for a firewall with regular software updates and reliable technical support from the vendor.
-   **Budget:** Firewalls range in price based on features and capabilities. Determine a budget and prioritize.
-   **Compatibility:** Ensure that the firewall is compatible with existing network hardware and software.
-   **Security Certifications:** Look for firewalls that comply with recognized security standards like NSS Labs or Common Criteria.
-   **Brand Reputation:** Choose a reputable brand with a proven track record and positive customer reviews.
-   **Virtual Appliance versus Hardware Firewalls:** Evaluate cloud-based options for flexibility and scalability but consider throughput and port sizes for latency and bottlenecks.
-   **Single versus High Availability:** Consider single points of failure and service level requirements.

Explore and compare [gateway options](/docs/vsrx?topic=vsrx-exploring-firewalls) available in  {{site.data.keyword.cloud_notm}}.

## Generic Routing Encapsulation (GRE) tunnels
{: #gre}

GRE tunnels support the Bring Your Own IP (BYOIP) requirement.

![Illustrates the details of GRE for a non-TGW solution architecture](GRE.svg){: caption="Figure 3. non-TGW GRE Encapsulation" caption-side="bottom"}
1.  Client network connectivity from on-premises is accomplished through {{site.data.keyword.dl_short}} access.
2.  A gateway is deployed in Classic, which provides routing and security functions.
3.  A GRE tunnel is created between the Gateway and a customer router.
4.  192.168.xx.xx (BYOIP) packets are encapsulated by 10.1.x.x (IBM assigned IP) which is routed through the BCR over the GRE and unencapsulated on the other side.

{{site.data.keyword.BluDirectLink}} does not offer BYOIP on the private network. The  {{site.data.keyword.cloud_notm}} backbone advertises the customer’s available routes that are assigned by IBM to their remote networks. Traffic with a destination IP address that is not assigned by  {{site.data.keyword.cloud_notm}} (10.0.0.0/8) is dropped by  {{site.data.keyword.cloud_notm}}. Traffic can be encapsulated between the remote client network and the  {{site.data.keyword.cloud_notm}} network for nonassigned  {{site.data.keyword.cloud_notm}} addresses by establishing GRE Tunnels between the gateway and a customer router.  This allows non-IBM assigned IP addresses to be passed back to the on-premises environment.

A second GRE is required between the Classic Gateway and {{site.data.keyword.powerSys_notm}} when nonassigned  {{site.data.keyword.cloud_notm}} addresses are used in the {{site.data.keyword.powerSys_notm}} environment.

For Gateways in separate regions, a third GRE is used to share nonassigned  {{site.data.keyword.cloud_notm}} addresses between Classic Gateways.

**When resiliency is required, GREs can be configured on two devices in an HA pair to eliminate single points of failure.**

Explore  {{site.data.keyword.cloud_notm}} public and private IP ranges [here](/docs/cloud-infrastructure?topic=cloud-infrastructure-ibm-cloud-ip-ranges).

## Enterprise Connectivity
{: #entrerprise-connectivity}

The two Direct Link options available are Direct Link Dedicated and Direct Link Connect.

Enterprise connectivity considerations include:

-   **Bandwidth requirements:** There are various bandwidth options available in [{{site.data.keyword.BluDirectLink}}](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl). Direct Link Dedicated offers higher bandwidth potential.
-   **Security needs:** If isolation is needed, {{site.data.keyword.dl_short}} Dedicated provides the highest isolation for sensitive data.
-   **Location and existing infrastructure:** Dedicated requires colocation or dedicated circuits. Consider the [Provider location](/docs/dl?topic=dl-locations#connect-locations).
-   **Cost:** Direct Link Connect is generally more cost effective, especially for smaller bandwidth needs.
-   **Deployment time:** Direct Link Connect can be deployed faster due to pre-established connections.

**To avoid IP address conflicts for classic connections to a {{site.data.keyword.dl_short}}, avoid IP address ranges in the 10.0.0.0/14, 10.200.0.0/14, 10.198.0.0/15, and 10.254.0.0/16 blocks for on-prem networks. On-prem routes that overlap are dropped.**

### Direct Link Dedicated
{: #direct-link-dedicated}

{{site.data.keyword.dl_short}} Dedicated is ideal for:

-   Organizations with colocation facilities near  {{site.data.keyword.cloud_notm}} PoPs or data centers.
-   Network service providers that deliver circuits to customers or other data centers.
-   Highly sensitive data or mission-critical applications that require maximum security and performance.
-   Organizations that need fine-grained control over routing and traffic management. Direct link dedicated is ideal for moderate bandwidth needs, multi-cloud, or hybrid cloud solution strategies.

{{site.data.keyword.dl_short}} Dedicated use cases include:

-   Banking and finance: Transferring sensitive financial data or running critical trading applications with maximum security and performance.
-   Healthcare: Managing protected health information (PHI) with strict compliance requirements.
-   Government and defense: Handling classified data or mission-critical systems that demand the highest security levels.
-   Research and development: Transferring large data sets for research or running high-performance computing (HPC) workloads.
-   Media and entertainment: Streaming high-quality video content or managing large media files with low latency and high bandwidth.

Review available Direct Link Dedicated locations [here](/docs/dl?topic=dl-locations#dedicated-locations).

### Direct Link Connect
{: #direct-link-connect}

{{site.data.keyword.dl_short}} Connect is ideal for:

-   Organizations with diverse connectivity requirements (multi-cloud, hybrid cloud).
-   Organizations that are not located near  {{site.data.keyword.cloud_notm}} PoPs or need smaller bandwidths.
-   Organizations seeking a more affordable private connection compared to Dedicated.
-   Simple and quick deployment due to pre-established connections in data centers.

{{site.data.keyword.dl_short}} Connect use cases include:

-   Hybrid cloud architectures: Connecting on-premises infrastructure to  {{site.data.keyword.cloud_notm}} resources for seamless workload distribution and data sharing.
-   Multi-cloud deployments: Linking  {{site.data.keyword.cloud_notm}} with other cloud providers for workload flexibility and disaster recovery.
-   Remote branch offices: Connecting geographically dispersed offices to  {{site.data.keyword.cloud_notm}} for centralized applications and data access.
-   Software-as-a-Service (SaaS) applications: Accessing SaaS applications that are hosted on  {{site.data.keyword.cloud_notm}} with improved performance and security.

Review available Direct Link Connect locations and providers [here](/docs/dl?topic=dl-locations#connect-locations).

## Load Balancing
{: #load-balancing}

### Local Load balancing
{: #local-load-balancing}

Load balancing is the process of distributing network traffic efficiently among multiple servers within a single region to optimize and ensure application availability to meet specific service level requirements.

Considerations Include:

-   **Layer-4 Load Balancing for HTTP, HTTPS, and TCP traffic:** Distributes traffic based on IP addresses, ports, and basic metrics like packet arrival time. Works well for simple traffic but lacks deeper application understanding.
-   **Advanced Layer-7 Load Balancing for HTTP and HTTPS traffic:** Analyzes the deeper aspects like URLs, headers, and user data. Offers granular routing based on the content, user needs, and server capabilities. Ideal for complex applications.
-   **Server Health Checks:** Regularly ping servers to ensure they are healthy and can handle traffic. Unhealthy servers are removed from the pool until they recover.
-   **SSL offload:** Takes over the decryption and encryption of HTTPS traffic, freeing up server resources for application processing. Improves performance and security.
-   **Public (Internet-facing to Private) Load Balancing:** Makes the internal applications accessible from the internet through a public IP address and NAT gateway. Provides an additional layer of security by keeping application servers hidden on a private network.
-   **Public to Public (Internet-facing) Load Balancing:** Distributes traffic among multiple public-facing servers for high availability and scalability of websites or services.
-   **Private (Internal) Load Balancing:** Distributes traffic among internal servers on a private network. Improves performance and scalability for internal applications without internet exposure.

 {{site.data.keyword.cloud_notm}} offers two Load balancer options for classic, which include: {{site.data.keyword.loadbalancer_full}} and {{site.data.keyword.vpx_full}} appliance.

Explore load balancer feature options [here](/docs/citrix-netscaler-vpx?topic=citrix-netscaler-vpx-explore).

Learn more about {{site.data.keyword.loadbalancer_full}} [here](/docs/loadbalancer-service?topic=loadbalancer-service-about-ibm-cloud-load-balancer)

Learn more about {{site.data.keyword.vpx_full}} Load Balancer [here](/docs/citrix-netscaler-vpx?topic=citrix-netscaler-vpx-about-citrix-netscaler-vpx)

### Third party Load Balancer Appliance
{: #third-party-load-balancer}

When there is a specific need or function that is not offered by any of the load balancer options within the  {{site.data.keyword.cloud_notm}} catalog, or an existing relationship is established with a third-party vendor, such as F5, you can deploy a third-party solution within  {{site.data.keyword.cloud_notm}} as a bring your own appliance.

## Global Load Balancing
{: #global-load-balancing}

Global Server Load Balancing (GSLB) is a technique for distributing internet traffic across geographically dispersed servers. It aims to optimize user experience and application performance by directing users to the nearest or most appropriate server based on various factors like latency, server load, and user location.  GSLB is a valuable component of a disaster recovery strategy.

If Global Server Load Balancing (GSLB) is required,  {{site.data.keyword.cloud_notm}} offers a Global load balancer service through {{site.data.keyword.cis_full_notm}}, which routes traffic to servers across multiple geographic locations to ensure application availability.

Other third-party connectivity providers such as Akamai, network appliances from vendors like F5 and Citrix, as well as Domain Name Service can also be leveraged to meet the requirement for GSLB.

When a user tries to access a website or application, the request goes to the GSLB device. The GSLB device uses various algorithms and real-time data to determine the best server to route the request to. Factors that are considered include:

-   **User location:** Sending users to the closest server minimizes latency.
-   **Server load:** Distributing traffic among available servers prevents overloading any single server.
-   **Server health:** Avoiding unresponsive or overloaded servers.
-   **Application-specific criteria:** Specific services might require routing based on user type, content availability, and so on.

Explore Global Server Load Balancing and other features of {{site.data.keyword.cis_full_notm}} [here](/docs/cis?topic=cis-about-ibm-cloud-internet-services-cis).

## Private Service Endpoints & Proxy Server
{: #service-endpoints}

With  {{site.data.keyword.cloud_notm}} service endpoints, you can connect to  {{site.data.keyword.cloud_notm}} services over the  {{site.data.keyword.cloud_notm}} Private network instead of the default public network.

Moving these workloads from the public network to the private network offers two advantages:

-   Enhanced Security - Cloud Services are no longer served on an internet routable IP address.
-   Cost-effective - the private network doesn not incure billable or metered bandwidth charges.

In  {{site.data.keyword.cloud_notm}} Classic, Virtual route forwarding (VRF) must be enabled on the account to move routing to a dedicated routing table. When VRF is enabled on the {{site.data.keyword.cloud_notm}} account, Private Service Endpoints can be provisioned. Cloud service resources can then be accessed by the  {{site.data.keyword.cloud_notm}} Private network from an IBM assigned IP address in the Classic Account.

Click to learn more on [{{site.data.keyword.cloud_notm}} VRF](/docs/dl?topic=dl-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud).

When the source address is not an IBM assigned IP address, a proxy server in classic is used as an intermediary allowing access to cloud services over the Direct Link.

Verify that Private Service endpoints are available for your cloud services [here](/docs/account?topic=account-vrf-service-endpoint&interface=ui).

Getting started with Service Endpoints [here](/docs/account?topic=account-vrf-service-endpoint&interface=ui).

In a multi-region deployment, which is described later in this document, for nontransit gateway locations, often a secondary region that has transit gateway services enabled can be chosen. In these situations, as an alternative to Private Service Endpoints and a proxy server in Classic, configuring a {{site.data.keyword.vpe_full}} (complementary {{site.data.keyword.vpc_short}} Service in the 2nd region) can provide access to  {{site.data.keyword.cloud_notm}} Services over the private network.

Verify that cloud services are {{site.data.keyword.vpe_short}} enabled [here](/docs/vpc?topic=vpc-vpe-supported-services).

![Illustrates the details of SE versus {{site.data.keyword.vpe_short}} for a non-TGW solution architecture](SE-vs-VPE.svg){: caption="Figure 4. non-TGW Service Endpoint access" caption-side="bottom"}

## Cloud Internet Services (CIS)
{: #cloud-internet-services}

{{site.data.keyword.cis_short}} provides global server load balancing, public domain name services, and public network security features.

Learn more about {{site.data.keyword.cis_short}} [here](/docs/cis?topic=cis-about-ibm-cloud-internet-services-cis)

## Domain Name Services (DNS)
{: #DNS}

 {{site.data.keyword.dns_short}} provides access to your systems and services through user-friendly domain names rather than IP addresses.

Key considerations are:

-   **Public versus private DNS name resolution:** ability to provide DNS-based access to users on the public Internet versus internal DNS-based communication within your private environment.
-   **Integration with other existing DNS systems:** integrating your on-premises DNS service with your cloud environment.

 {{site.data.keyword.cloud_notm}} provides a flexible approach to DNS name resolution for systems hosted in classic environments.

-   **Public DNS name resolution**: translates human-friendly names into computer-readable addresses.
    -   Use the [DNS interface](/docs/dns?topic=dns-how-to-use-the-dns-interface) in the  {{site.data.keyword.cloud_notm}} Portal to manage your zones and records. The domain can be hosted by any third-party provider. Set the name server (NS) record to the provided IBM name servers.
    -   Use the DNS function provided with {{site.data.keyword.cis_short}}. Domain name control must be delegated to {{site.data.keyword.cis_short}}.
-   **Private DNS name resolution**: allows integration with your on-premises DNS server over private connectivity.
    -   To configure DNS services in the classic environment, typical options are:
        -   Install your own custom DNS service on a virtual server.
        -   Configure DNS on your gateway device.
    -   Utilize {{site.data.keyword.dns_full_notm}} (complementary VPC Service).
        This Service provides versatile private name resolution between classic, on-premises, and {{site.data.keyword.vpc_short}} resources by deploying [Custom Resolvers](/docs/dns-svcs?topic=dns-svcs-custom-resolver). Learn more about [{{site.data.keyword.dns_full_notm}}](/docs/dns-svcs?topic=dns-svcs-getting-started).
