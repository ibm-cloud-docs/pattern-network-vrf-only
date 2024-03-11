[Title]

[Company]

# Overview

IBM Cloud contains three main environments known as Classic, Virtual Private Cloud (VPC) and Power Virtual Server (PowerVS). The Classic environment is connected by a layer 2 network and can deploy resources not limited to Virtual Server Instances (VSI), Bare Metal Servers, and dedicated VMware. A Virtual Private Cloud (VPC) is a secure, isolated virtual network that combines the security of a private cloud with the availability and scalability of IBM's public cloud and supports x86 and VMware workloads. Power Virtual Server is an IaaS offering that allows you to deploy your current IBM i, AIX and Linux workloads in a hybrid cloud environment, without refactoring. The objective of this pattern is to:

-   Illustrate network connectivity to PowerVS and Classic environments when the data center does not have VPC infrastructure.
-   Provide an IBM Solution Design for the Network elements required when deploying in a non-Transit Gateway (non-TGW) data centers or a data center that does not contain availability zones.
-   Securely connect your external locations to IBM Cloud with Direct Link, enabling access to both Classic infrastructure and PowerVS resources.
-   Focus on IBM Cloud Network elements, while ensuring requirements can be met from a performance, system availability, and connectivity perspective.

This pattern is intended to:

-   Accelerate and simplify solution design by providing a standard IBM Cloud deployment architecture reference following the [IBM Architecture Framework](https://cloud.ibm.com/docs/architecture-framework).
-   Provide a prescriptive, end-2-end enterprise-class solution design, with diagrams, component architecture decisions along with rationale for cloud component selection to meet enterprise requirements.

# Reference Architecture

This reference architecture is leveraged in data centers that currently do not have VPC and Transit Gateway (TGW) services available, as of Q12023, currently centers such as Montreal 01, San Jose 03, San Jose 04, Chennai 01, and Hong Kong 02.

For an updated list of those DCs which have TGW, see [transit gateway locations](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-tg-locations#szr-table).

We will refer to this approach from now on as “non-TGW” since there is no VPC and TGW connectivity utilized.

Please note that it is a common approach to complement classic environments in these non-TGW locations with VPC services hosted in another location. This allows additional functionality that is only available with VPC services. This document will reference this approach as “complementary VPC services” and it will be highlighted in the multi-region deployment in the Resiliency section below.

## Architecture diagram

This architecture will describe on-premises data center(s) connectivity into IBM Cloud Classic, with firewall services and Power Virtual Server using a non-TGW model. The diagram includes examples to show where workload compute instances, proxy servers and jump servers would reside. Within the diagram, there are identifying numbers indicating key components in the description below.

![](image/84a49a80d8d5d70b07a564177a1ce963.png)

Figure 1. non-TGW solution architecture

1.  Client network connectivity from on-premises using redundant Direct Links.
2.  Gateway provides routing and security functions.
3.  Optional network path is accomplished through site-to-site VPN terminated on Classic Gateway.
4.  Power Virtual Server workspace, subnets, and resources
5.  GREa tunnel allows BYOIP to be advertised between Classic and on-premises.
6.  GREb tunnel allows BYOIP to be advertised between Classic environments in separate regions.
7.  GREc tunnel allows BYOIP to be advertised between Classic and PowerVS.
8.  Virtual jump server for remote administrative access.
9.  Custom DNS services virtual server
10. Proxy Server as an intermediary between on-prem and cloud services.
11. Private Service Endpoints (SE) allow access to cloud services over the private network.
12. Cloud Internet Services (CIS) to enhance the security, performance, and reliability of internet-facing applications and websites.
13. Compute Instance in PowerVS and Classic

## Design scope

Following the [Architecture Framework](https://test.cloud.ibm.com/docs-draft/architecture-framework?topic=architecture-framework-taxonomy), the non-TGW network pattern covers design considerations and architecture decisions for the following aspects and domains:

-   **Compute:** Virtual Servers, Bare Metal Servers
-   **Networking:** Enterprise Connectivity, BYOIP/Edge Gateways, Network Segmentation, Cloud Native Connectivity, Load Balancing, and DNS
-   **Security:** Identity and Access Management (IAM)
-   **Resiliency:** High Availability, Disaster Recovery
-   **Service management:** Monitoring, Logging, Auditing, Alerting, Event Management

![](image/37b9c586adbe228b621ba0265004cc7a.png)

Figure 2. non-TGW design scope

The Architecture Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. See [Introduction to the architecture framework](https://test.cloud.ibm.com/docs-draft/architecture-framework?topic=architecture-framework-intro) for more details.

## Requirements

The following represents a baseline set of requirements that are applicable to most clients and critical to successful non-TGW network deployment. The pattern assumes the client will have a requirement of geolocation, data residency, or low latency that requires resource deployment in a data center that does not have transit gateway technology.

| **Aspect**         | **Requirement**                                                                                                                                                                                                                                                                                                    |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute            | Secure remote administrative support of all devices within the IBM Cloud environment.                                                                                                                                                                                                                              |
| Network            | Private enterprise connectivity from customer data centers to IBM Cloud for access to applications, data, and services.                                                                                                                                                                                            |
|                    | Private administrative and management connectivity                                                                                                                                                                                                                                                                 |
|                    | Provide network isolation with the ability to separate applications based on attributes such as data classification, public versus private traffic flows, and internal application function.                                                                                                                       |
|                    | Provide the ability to use Bring Your Own IP (BYOIP)                                                                                                                                                                                                                                                               |
| Security           | Firewalls must be restrictively configured to provide advanced security features and prevent all traffic, both inbound and outbound, except that which is specifically required, documented, and approved and (optionally) include Intrusion Protection System (IPS) and Intrusion Detection System (IDS) services |
|                    | Distributed Denial of Service (DDoS) and Web Application Firewall (WAF) security capabilities required                                                                                                                                                                                                             |
|                    | Secure access for administration and management of the environment                                                                                                                                                                                                                                                 |
| Resiliency         | Multi-Region capability to support a disaster recovery strategy and solution that allows all production applications to be included by using cloud infrastructure DR strategies.                                                                                                                                   |
| Service management | Provide health and system monitoring with ability to monitor and correlate performance metrics and events and provide alerting across applications and infrastructure                                                                                                                                              |
|                    | Ability to diagnose issues and exceptions and identify error source                                                                                                                                                                                                                                                |

Table 1. non-TGW requirements

## 

## Components

| **Aspect**             | **Component**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | **How the component is used**                                                                                                                                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute                | Virtual Server, Bare Metal, containers or VMware solution on Classic, Power Virtual Servers – Workload Hosts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Hosting the workloads running in the classic or Power environment                                                                                                        |
|                        | [Virtual Server on Classic](https://cloud.ibm.com/docs/virtual-servers?topic=virtual-servers-about-virtual-servers) - Jump Server or Bastion Host                                                                                                                                                                                                                                                                                                                                                                                                                                                | Jump Server is deployed on a virtual server instance within classic and will be used for remote administrative support                                                   |
|                        | [Virtual Server on Classic](https://cloud.ibm.com/docs/virtual-servers?topic=virtual-servers-about-virtual-servers) - Proxy Server                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Acts as a gateway using the private network between on-prem network and cloud services                                                                                   |
| Networking             | Virtual Private Network (VPN)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Provides a secured connection into IBM Cloud over the Internet. This could be used for migrations, administrative access, and backup connectivity of private networking. |
|                        | [**Gateway Appliance in Classic**](https://cloud.ibm.com/docs/gateway-appliance?topic=gateway-appliance-getting-started-ga) [Juniper vSRX](https://cloud.ibm.com/docs/vsrx?topic=vsrx-getting-started) [Virtual Router Appliance](https://cloud.ibm.com/docs/virtual-router-appliance?topic=virtual-router-appliance-getting-started-vra) [FortiGate](https://cloud.ibm.com/docs/fortigate-10g?topic=fortigate-10g-getting-started) Bring Your Own Gateway ([BYOG](https://cloud.ibm.com/docs/gateway-appliance?topic=gateway-appliance-order-byoa)) BM or VSI (Checkpoint, Fortinet, Palo Alto) | Provides router, firewall, and VPN gateway functions for secure and reliable connectivity to cloud resources.                                                            |
|                        | Generic Routing Encapsulation (GRE) tunnels                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Supports Bring Your Own IP (BYOIP) communication between on-premise, Classic Infrastructure, and PowerVS.                                                                |
|                        | [Direct Link](https://cloud.ibm.com/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) Direct Link Dedicated Direct Link Connect                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Connect on-premise networks to the IBM Cloud using physical telco connections or virtual exchange services.                                                              |
|                        | [Load Balancers](https://cloud.ibm.com/catalog/infrastructure/load-balancer-group) Cloud Load Balancer Citrix NetScaler VPX                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Application Load Balancing for web servers, app servers, and database servers                                                                                            |
|                        | [Private Service Endpoints](https://cloud.ibm.com/docs/account?topic=account-vrf-service-endpoint&interface=ui#use-service-endpoint)                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Connect directly to cloud services without using the public network                                                                                                      |
|                        | [Cloud Internet Services (CIS)](https://test.cloud.ibm.com/docs-draft/cis?topic=cis-getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Public Load balancing of web server traffic across regions                                                                                                               |
|                        | [DNS Services](https://test.cloud.ibm.com/docs-draft/dns-svcs?topic=dns-svcs-about-dns-services)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | The Domain Name System (DNS) to associate human-friendly domain names with IP addresses                                                                                  |
| **Security**           | [IAM](https://test.cloud.ibm.com/docs-draft/account?topic=account-cloudaccess)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | IBM Cloud Identity & Access Management                                                                                                                                   |
|                        | [Cloud Internet Services (CIS)](https://test.cloud.ibm.com/docs-draft/cis?topic=cis-getting-started)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | DDoS protection and Web Application Firewall (WAF) for public connectivity                                                                                               |
|                        | [Gateway Appliance in Classic](https://cloud.ibm.com/docs/gateway-appliance?topic=gateway-appliance-getting-started-ga)                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Advanced firewall capability such as Intrusion Detection System (IDS) and Intrusion Protection System (IPS) services                                                     |
|                        | Jump Server or Bastion Host                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Jump Server is deployed on a virtual server instance within classic and will be used for remote administrative support                                                   |
| **Resiliency**         | Multi-Region Deployment                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Allows for disaster recovery in secondary region                                                                                                                         |
|                        | Multiple Direct Link connections                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Allows for network resiliency for failover & recovery                                                                                                                    |
| **Service management** | Health Dashboard                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Apps and operational monitoring                                                                                                                                          |

Table 2. non-TGW solution components

# Compute design

## Workload Hosts (Classic and Power)

Selecting the appropriate workload host in this architecture includes but is not limited to the following considerations regarding:

-   Performance (CPU, Memory, storage options and network speed)
-   Virtualization & Tenancy (container, virtual or bare metal, multitenant or dedicated)
-   Security, availability, and cost considerations
-   Application-specific requirements regarding certification or optimization

Based on workload and specific requirements, select from a range of Virtual Servers, Bare Metal servers, containers and VMware based solutions.

Explore [Bare Metal Server](https://cloud.ibm.com/docs/bare-metal?topic=bare-metal-about-bm)s for Classic.

Explore [Virtual Servers](https://cloud.ibm.com/docs/virtual-servers?topic=virtual-servers-about-virtual-servers) for Classic.

Explore [Kubernetes](https://cloud.ibm.com/docs/containers?topic=containers-cluster-create-classic&interface=ui) and [Openshift](https://cloud.ibm.com/docs/openshift?topic=openshift-getting-started) for classic environments.

Explore [VMware Solutions](https://cloud.ibm.com/docs/vmwaresolutions?topic=vmwaresolutions-getting-started).

Explore [Power Virtual Servers](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-getting-started).

## Jump Server and Bastion Hosts

Infrastructure requirements for Linux and Windows jump servers and Bastion host can vary based on factors such as the number of concurrent users, the specific use case, and the applications or services running on the server. Here are some general guidelines:

An average Jump server with 8 CPU and 16GB of RAM, can support 25 concurrent sessions of any type (100 serial-over-LAN sessions, 200 Telnet or SSH sessions). Additional sessions are supported depending on the session types or by higher server specifications.

Infrastructure requirements for Ubuntu LTS, CentOS, or Debian Bastion Hosts can be viewed below.

Table 3 contains general jump server sizing guidelines:

| **Traffic** | **Concurrent Users** | **Server Size**               |
|-------------|----------------------|-------------------------------|
| Low         | 10-20                | 1-2 Cores CPU; 2-4 GB Memory  |
| Moderate    | 30-50                | 2-4 Cores CPU; 4-8 GB Memory  |
| High        | 50+                  | 4-8 Cores CPU; 8-16 GB Memory |

Table 3. non-TGW Bastion Server Sizing

The Jump server or Bastion Host can be deployed on either a bare metal server or Virtual Server Instance (VSI) within the Classic environment.

# Network design

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

# Security design

## Identity and Access Management (IAM)

Identity and Access Management provides role-based access controls (RBAC) and is part of the zero-trust strategy that allows for least privileged access to help support regulatory and compliancy requirements. IBM Security Verify can be added to support multi-factor authentication.

Click to learn more about [Identity and Access Management (IAM)](https://cloud.ibm.com/docs/account?topic=account-iamoverview).

Click to learn more about [IAM Roles](https://cloud.ibm.com/docs/account?topic=account-userroles).

Click to learn more about [IBM Security Verify](https://www.ibm.com/verify).

## Cloud Internet Services (CIS)

In addition to providing Global Server Load balancing and Domain name services, Cloud Internet Services (CIS) provides many security features to help meet compliance requirements in a pay as you go or bundled package.

Consider leveraging Cloud Internet Services or other third-party products such as Akamai, Cloudflare, Imperva, Barracuda, or F5 to meet security requirements such as:

-   **DDoS Protection:** Shields your website from malicious attacks that flood it with traffic.
-   **Web Application Firewall (WAF):** Acts as a security guard for your website, filtering out suspicious traffic and blocking known threats like SQL injections and code injection attempts.
-   **Transport Layer Security (TLS):** Encrypts communication between your website and visitors, protecting sensitive data like passwords and credit card information.
-   **Range**: non-HTTP and HTTPS port protection Secures ports on your server beyond the standard web traffic ports (HTTP and HTTPS), protecting against attacks targeting vulnerable services.

Learn more about Cloud Internet Service feature [here](https://test.cloud.ibm.com/docs-draft/cis?topic=cis-about-ibm-cloud-internet-services-cis).

Explore Cloud Internet Services [DDoS](https://cloud.ibm.com/docs/cis?topic=cis-distributed-denial-of-service-ddos-attack-concepts).

Explore Cloud Internet Services [WAF](https://cloud.ibm.com/docs/cis?topic=cis-waf-q-and-a).

## Gateway Appliance – Firewall

IBM Cloud Classic Firewalls offer a variety of security functions to protect your cloud resources. Specific features and capabilities of IBM Cloud Classic firewalls vary depending on the vendor, hardware, software, licenses, add-on bundle, and configuration options selected.

Before making an appliance choice, consider existing vendor relationships and operation teams’ technical expertise.

In addition, consider the following needs:

-   **Number of users and devices:** How many devices will be connected to the network? A small business network will have different needs than a large business network.
-   **Types of devices:** Consider the variety of resources, data, and applications connected to the network.
-   **Bandwidth requirements:** How much data traffic will the network typically handle?
-   **Scalability:** Choose a firewall that can grow with your network needs.
-   **Type of firewall:** Different firewall types offer different levels of protection. Consider stateful inspection firewalls, application-level firewalls, or next-generation firewalls (NGFWs) depending on the security need.
-   **Security features:** Look for features like intrusion detection and prevention systems (IDS and IPS), deep packet inspection (DPI), malware protection, and content filtering.
-   **Threat intelligence:** Choose a firewall that receives regular updates on the latest threats and vulnerabilities.

Consider the security level required based on the business need:

Tier 1: Public Front-end Enterprise

Tier 2: General Internal Enterprise

Tier 3: Business Critical Enterprise

Tier 4: Highly sensitive Enterprise

Tier 5: Ultra-Secure Enterprise

| **Security Function**                                                    | **Tier 1** | **Tier 2** | **Tier 3** | **Tier 4** | **Tier 5** |
|--------------------------------------------------------------------------|------------|------------|------------|------------|------------|
| Network Firewall                                                         | ü          | ü          | ü          | ü          | ü          |
| Web Application Firewall (WAF)                                           | ü          | ü          | ü          | ü          | ü          |
| Intrusion Detection System (IDS)                                         | ü          | ü          | ü          | ü          | ü          |
| Antivirus/Antimalware Software                                           | ü          | ü          | ü          | ü          | ü          |
| Network Segmentation                                                     |            | ü          | ü          | ü          | ü          |
| Administrative Role-Based Access Controls (RBAC)                         |            | ü          | ü          | ü          | ü          |
| Virtual Private Network (VPN)                                            |            | ü          | ü          |            |            |
| Advanced Firewall with Deep Packet Inspection                            |            |            | ü          | ü          | ü          |
| Intrusion Prevention System (IPS)                                        |            |            | ü          | ü          | ü          |
| Multifactor Authentication (MFA)                                         |            |            | ü          | ü          | ü          |
| Integration with Security Incident and Event Management (SIEM) System    |            |            | ü          | ü          | ü          |
| Network Segmentation with Strict Access Controls                         |            |            |            | ü          | ü          |
| Administrative Privileged Access Management (PAM)                        |            |            |            | ü          | ü          |
| Integration with Advanced Threat Intelligence and Threat Hunting systems |            |            |            | ü          | ü          |
| Zero-Trust Architecture                                                  |            |            |            |            | ü          |
| Micro-Segmentation                                                       |            |            |            |            | ü          |
| Continuous Monitoring and Auditing                                       |            |            |            |            | ü          |
| Integration with Advanced Threat Detection and Response (ATDR) Systems   |            |            |            |            | ü          |
| Integration with Real-Time Incident Response Capabilities                |            |            |            |            | ü          |

Table 4: Non-TGW Security features

Explore Virtual Router Appliance (vRA) [here](https://cloud.ibm.com/docs/virtual-router-appliance?topic=virtual-router-appliance-getting-started-vra).

Explore Juniper vSRX license features [here](https://cloud.ibm.com/docs/vsrx?topic=vsrx-getting-started#choosing-license).

Explore Bring Your Own Gateway Appliance (BYOG) to support Checkpoint, Fortinet, and Palo Alto [here](https://cloud.ibm.com/docs/gateway-appliance?topic=gateway-appliance-order-byoa).

## VPN Security

VPN offers a valuable layer of security for public internet activities.

Key VPN Security Considerations include:

-   **Encryption Strength:** Choose a VPN with strong encryption algorithms like AES-256. Weak encryption can be cracked, exposing your data.
-   **Protocol Choice:** Popular protocols like OpenVPN and IKEv2/IPsec offer strong security and performance. Avoid outdated or less secure protocols like PPTP.
-   **Authentication:** Consider strong authentication methods like multi-factor authentication for secure data transmission.
-   **Access Control:** Granularly control access permissions to resources accessible through the VPN, limiting potential damage from breaches.
-   **Security of Endpoints:** Ensure all connected devices comply with security policies and are kept up-to-date with patches.

## Service Endpoints

With IBM Cloud service endpoints, you can connect to IBM Cloud services over the IBM Cloud private network instead of the default public network. Moving these workloads from the public network to the private network offers enhanced security as Cloud Services are no longer served on an internet routable IP address.

## Jump Server or Bastion Host

Jump Servers or Bastion hosts can offer additional levels of security and control and can be deployed on bare metal or virtual server instances within IBM Cloud Classic.

Considerations include:

1.  Enhanced Security required - All access to internal systems funnels through the jump server or Bastion host, making it easier to monitor and enforce security policies. Access can be granted or revoked to specific users and systems with granularity. By keeping internal systems directly inaccessible from the outside, minimizes potential entry points for attackers. Hackers would need to compromise the jump server first, adding an extra layer of defense.
2.  Improved Management and Auditing - Manage access to all internal systems from a single point, streamlining the process and reduce errors. All activity on the jump server or Bastion host is logged, providing a centralized record of who accessed what and when. This helps with troubleshooting, security audits, and forensic investigations.
3.  Secure Access to Legacy Systems - Jump servers and Bastion hosts can act as a bridge between modern tools and legacy systems that might not support secure protocols like SSH. You can use the jump server to tunnel secure connections to older systems.
4.  Multi-factor Authentication (MFA) - MFA can be implemented on the jump server or Bastion host itself, adding another layer of protection before granting access to internal systems.

The table below will help determine if a bastion or jumper server is needed:

| **Feature**    | **Jump Server**                                                       | **Bastion Host**                                                        |
|----------------|-----------------------------------------------------------------------|-------------------------------------------------------------------------|
| Location       | Internal Network                                                      | Network perimeter (DMZ and public subnet)                               |
| Purpose        | Manage internal systems                                               | Grant controlled access to specific internal systems for external users |
| Security Focus | Centralized access control, simplified administration                 | Secure entry point, isolation of internal systems                       |
| Attack Surface | Higher (internal systems directly exposed if jump server compromised) | Lower (internal systems protected even if bastion host compromised)     |

Table 5. non-TGW Jump Server vs Bastion Host Matrix

# Resiliency design

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

# Service management design

## Monitoring

Evaluate the level of monitoring required by considering the workload criticality and the metrics that need to be observed.

Basic instance monitoring deployed in IBM Cloud Classic environments can be viewed from the cloud portal Health Dashboard (Health Dashboard, Device \> Usage).

More advanced monitoring capabilities such as troubleshooting, events and alerts, and custom dashboards can be leveraged by integrating the cloud resources with IBM Cloud Monitoring (complementary VPC service). The agent or the Windows Prometheus Bundle are installed on the devices.

Alternatively, third party monitoring software can be deployed within IBM Cloud Classic for more detailed monitoring.

Learn more about IBM Cloud Monitoring [here](https://cloud.ibm.com/docs/monitoring?topic=monitoring-getting-started#getting-started)

## Log Analysis

Examine what logs are required for troubleshooting and auditing, as well as the retention policy necessary to meet the audit and compliance requirements.

Third party software can be implemented within IBM Cloud Classic to enable log analysis within IBM Cloud Classic.

Alternatively, IBM Log Analysis (complementary VPC Service) can be used to manage operating system logs, application logs, and platform logs in the IBM Cloud. IBM Log Analysis offers administrators, DevOps teams, and developers advanced features to filter, search, and tail log data, define alerts, and design custom views to monitor application and system logs.

Learn more about IBM Log Analysis [here](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-getting-started#getting-started)

## Activity Tracking

Consider the need to record and monitor the activities and changes made inside the IBM Cloud account to assist in investigating abnormal activity, critical actions, and to meet regulatory audit requirements.

Third party software such as Splunk and Datadog can be integrated with IBM Cloud Classic to provide security monitoring, compliance reporting, and operational intelligence.

Alternatively, IBM Cloud Activity Tracker (complementary VPC Service) monitors and manages activities in the IBM Cloud. It provides a dashboard and notification for real-time monitoring.

Learn more about IBM Activity Tracker [here](https://cloud.ibm.com/docs/activity-tracker?topic=activity-tracker-getting-started)

Explore details on activity tracker events for virtual servers in Classic [here](https://cloud.ibm.com/docs/virtual-servers?topic=virtual-servers-at_events) and for bare metal in Classic [here](https://cloud.ibm.com/docs/bare-metal?topic=bare-metal-bm-at-events)

# Architecture Decisions

Please note that this architecture pattern focusses on providing guidance for multiple use cases in the described network scenario, as such some components will list multiple options, allowing you to select the appropriate component based on your specific environments (e.g. for workload servers or type of gateway appliance).

## 

## Architecture decisions for compute

The following are compute architecture decisions for the Network architecture for data centers without a Transit Gateway service pattern.

| **Architecture decision**        | **Requirement**                                                            | **Options**                                 | **Decision**   | **Rationale**                                    |
|----------------------------------|----------------------------------------------------------------------------|---------------------------------------------|----------------|--------------------------------------------------|
| Compute (Jump Host)              | Secure connection to manage internal systems and centralize access control | \*\*·\*\*Bare Metal \*\*·\*\*Virtual Server | Virtual Server | flexible compute resources to meet compute needs |
| Compute (Proxy or Baston Server) | Provide access to Service Endpoints from non-IBM Cloud IPs                 | \*\*·\*\*Bare Metal \*\*·\*\*Virtual Server | Virtual Server | flexible compute resources to meet compute needs |

Table 6. non-TGW Compute architecture decisions

## Architecture decisions for network

The following are network architecture decisions for the Network architecture for data centers without a Transit Gateway service pattern.

### 

### Architecture decisions for enterprise connectivity

| **Architecture decision** | **Requirement**                                                                                | **Options**                                                                                   | **Decision**                                     | **Rationale**                                                         |
|---------------------------|------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------|
| Management connectivity   | Provide secure, encrypted connectivity to the cloud’s private network for management purposes. | \*\*·\*\*SSL VPN \*\*·\*\*IPsec VPN \*\*·\*\*Site-to-Site VPN on Gateway appliance in Classic | Site-to-Site VPN on Gateway appliance in Classic | Secure and suitable for production-level performance                  |
| Enterprise connectivity   | Provide connectivity between client enterprise and IBM Cloud.                                  | \*\*·\*\*Direct Link Connect \*\*·\*\*Direct Link Dedicated                                   | Direct Link Connect                              | Lower cost, quicker deployment time, SDN provides layer of resiliency |

Table 7. non-TGW network enterprise connectivity architecture decisions

### 

### Architecture decisions for BYOIP and Edge Gateways

| **Architecture decision** | **Requirement**                                                                          | **Options**                                                                                                                 | **Decision**                                                                                                               | **Rationale**                        |
|---------------------------|------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| BYOIP approach            | Provide capability for bring your own IP (BYOIP) to IBM Cloud.                           | GRE (Generic Routing Encapsulation) Tunnel                                                                                  | GRE (Generic Routing Encapsulation) Tunnel                                                                                 | Allows BYOIP routes to be advertised |
| Edge Gateways             | Capability to provide edge routing services, firewall and tunnel (VPN, GRE) termination. | **Gateway Appliance in Classic** ·Juniper vSRX ·Virtual Router Appliance ·FortiGate ·BYOG (Checkpoint, Fortinet, Palo Alto) | Select based on required [features](https://cloud.ibm.com/docs/vsrx?topic=vsrx-exploring-firewalls) and client preferences | Client preference                    |

Table 8. non-TGW network BYOIP and Edge Gateway architecture decisions

## 

### Architecture decisions for network segmentation and isolation

| **Architecture decision**          | **Requirement**                                        | **Options**                       | **Decision**                      | **Rationale**                                 |
|------------------------------------|--------------------------------------------------------|-----------------------------------|-----------------------------------|-----------------------------------------------|
| Network segmentation and isolation | Ability to provide network isolation across workloads. | VLANs, Subnets, & Security Groups | VLANs, Subnets, & Security Groups | Allows for segmentation and network isolation |

Table 9. non-TGW network segmentation and isolation architecture decisions

### 

### Architecture decisions for cloud native connectivity

| **Architecture decision**                     | **Requirement**                             | **Options**                                                                      | **Decision**                    | **Rationale**                                                                                  |
|-----------------------------------------------|---------------------------------------------|----------------------------------------------------------------------------------|---------------------------------|------------------------------------------------------------------------------------------------|
| Cloud Native Connectivity (to cloud services) | Provide secure connection to Cloud Services | \*\*·\*\*Private Cloud Service endpoints \*\*·\*\*Public Cloud Service Endpoints | Private Cloud Service endpoints | Provides private connectivity to cloud services offering enhanced security and cost efficiency |

Table 10. non-TGW network cloud native connectivity architecture decisions

## 

### Architecture decisions for load balancing

| **Architecture decision** | **Requirement**                                                                                                            | **Options**                                                                      | **Decision**                  | **Rationale**                                                                                                                                           |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Global Load balancing     | Load balancing over the public network across two regions in the event of an outage (DR) for failover to the other region. | \*\*·\*\*Cloud Internet Service (CIS) \*\*·\*\*Citrix Netscaler VPX \*\*·\*\*DNS | Cloud internet Services (CIS) | Provides a cost-effective solution while offering additional security features                                                                          |
| Load balancing (public)   | Load balancing workloads across multiple workload instances or zones over the public network.                              | \*\*·\*\*IBM Cloud Load Balancer \*\*·\*\*Citrix Netscaler VPX                   | IBM Cloud Load Balancer       | Provides wide range of load balancing function for both public and private traffic cost effectively                                                     |
| Load balancing (private)  | Load balancing workloads across multiple workload instances or zones over the private network.                             | \*\*·\*\*IBM Cloud Load Balancer \*\*·\*\*Citrix Netscaler VPX                   | IBM Cloud Load Balancer       | \*\*·\*\*Cloud Load Balancer meets small to mid-size, low complexity requirement. \*\*·\*\*Citrix Netscaler VPX meets large complex load balancer needs |

Table 11. non-TGW network load balancing architecture decisions

## 

### Architecture decisions for domain name system

| **Architecture decision** | **Requirement**                                                                                 | **Options**                                                                                                                | **Decision**         | **Rationale**                                                                                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Public DNS                | Provide DNS resolution to support the use of hostnames instead of IP addresses for applications | **·** DNS via cloud portal \*\*·\*\*Cloud Internet Services (CIS) \*\*·\*\*Third Party provider \*\*·\*\*Custom DNS on VSI | DNS via Cloud portal | Cost effective and reliable                                                                                                           |
| Private DNS               | Provide DNS resolution within IBM Cloud's private network                                       | \*\*·\*\*Custom DNS on VSI \*\*·\*\*DNS on Gateway appliance \*\*·\*\*DNS services in VPC                                  | Custom DNS on VSI    | \*\*·\*\*Can handle the most complex DNS needs \*\*·\*\*When VPC service is available, the preferred approach is DNS services in VPC. |

Table 12. non-TGW network Domain Name System architecture decisions

# Architecture decisions for security

The following are security architecture decisions for the Network architecture for data centers without a Transit Gateway service pattern.

## 

## Architecture decisions for identity and access management

| **Architecture decision**               | **Requirement**                                                                                                                                                                                                            | **Options**                                                                                             | **Decision**                    | **Rationale**                                                                                                                                                                                                                                                                                      |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Privileged Access Management            | Ensure that all operator actions are run securely through a bastion host \\n - Implement session recording to track all activities and note any potential threats \\n Manage access to resources and track commands issued | **·** BYO Bastion Host or Jump Server **·** BYO Bastion Host with Privileged Access Management (PAM) SW | BYO Bastion Host or Jump Server | The Bastion Host or Jump server is a Virtual Server instance that is provisioned through SSH over a private network to securely access resources within IBM Cloud’s private network. **˙**Using PAM software is recommended when session recording, tracking, and managing all access is required. |
| Identity Access & Role Management (IAM) | Securely authenticate users for platform services and control access to resources consistently across IBM Cloud                                                                                                            | IBM Cloud IAM                                                                                           | IBM Cloud IAM                   | Use IAM access policies to assign users, service IDs, and trusted profiles access to resources within the IBM Cloud account.                                                                                                                                                                       |

Table 13. non-TGW security identity and access management architecture decisions

## 

# Architecture decisions for resiliency

The following are resiliency architecture decisions for the Network architecture for data centers without a Transit Gateway service pattern.

## Architecture decisions for Network Resiliency

| **Architecture decision**           | **Requirement**                                                                                                       | **Options**                                                                                                                                   | **Decision**                                      | **Rationale**                                                                                                                   |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| High Availability Network path      | Ensure availability of resources if outages occurs. Support SLA targets for availability                              | \*\*·\*\*Single Direct Link Dedicated \*\*·\*\*Single Direct Link Connect \*\*·\*\*Two Direct Link Dedicated \*\*·\*\*Two Direct Link Connect | Two Direct Link Connect                           | Two Direct Link Connect provides a layer of resiliency with SDN over physical hardware while being cost effective and flexible. |
| High Availability Gateway Appliance | Ensure availability of infrastructure resources if outages occur. Support SLA targets for infrastructure availability | \*\*·\*\*Deploy Gateway Appliance of choice in HA Pair                                                                                        | Deploy Gateway Appliance of choice in HA Pair     | Ensures if one appliance unavailable access is still available through remaining gateway appliance.                             |
| High Availability Path Detection    | Ensure the quickest traffic path recovery                                                                             | \*\*·\*\*Bidirectional Forwarding Detection (BFD)                                                                                             | \*\*·\*\*Bidirectional Forwarding Detection (BFD) | provides a much faster way of detecting link failures compared to the built-in mechanisms within routing protocols              |

Table 14. non-TGW resiliency architecture decisions

## 

## Architecture decisions for disaster recovery

| **Architecture decision** | **Requirement**                                                                       | **Options**                                                                                                                                   | **Decision**               | **Rationale**                                                                                                       |
|---------------------------|---------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------------------------------------------------------------------------------------------------|
| Disaster Recovery Network | Network disaster recovery capability in secondary region to meet RTO/RPO requirements | \*\*·\*\*Single Direct Link Dedicated \*\*·\*\*Single Direct Link Connect \*\*·\*\*Two Direct Link Dedicated \*\*·\*\*Two Direct Link Connect | Single Direct Link Connect | provides a cost effective and flexible connection into a second region, with metered and unmetered billing options. |

Table 15. non-TGW disaster recovery architecture decisions

# Architecture decisions for service management

The following are service management architecture decisions for the Network architecture for data centers without a Transit Gateway service pattern.

## Architecture decisions for monitoring

| **Architecture decision**                                   | **Requirement**                                                                                          | **Options**                                                                                          | **Decision**              | **Rationale**                                                                                                                                                                        |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Operational Monitoring of Cloud infrastructure and services | Monitor system health to detect issues that might impact the availability of the system and application. | \*\*·\*\*IBM Cloud Health Dashboard \*\*·\*\*BYO Monitoring Tool \*\*·\*\*IBM Cloud Monitoring (VPC) | IBM Cloud Heath Dashboard | \*\*·\*\*IBM Cloud Heath Dashboard reports health and vitality of cloud infrastructure and services. \*\*·\*\*When VPC is available, the preferred approach is IBM Cloud Monitoring. |

## Table 16. non-TGW service management monitoring architecture decisions

## Architecture decisions for logging

| **Architecture decision**                           | **Requirement**                                                                                             | **Options**                                                | **Decision**     | **Rationale**                                                                                                                                                               |
|-----------------------------------------------------|-------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Log Monitoring of Cloud infrastructure and services | Monitor operational logs to detect issues that might impact the availability of the system and application. | \*\*·\*\*BYO Logging Tool \*\*·\*\*IBM Cloud Logging (VPC) | BYO Logging Tool | \*\*·\*\*BYO Logging tool allows for the most flexibility in meeting log monitoring requirements. **·** When VPC is available, the preferred approach is IBM Cloud Logging. |

Table 17. non-TGW service management logging architecture decisions

## Architecture decisions for auditing

| **Architecture decision** | **Requirement**                                                                                | **Options**                                                                | **Decision**            | **Rationale**                                                                                                                                                                                              |
|---------------------------|------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Audit Logging             | Monitor audit logs to track changes to cloud resources and detect potential security problems. | \*\*·\*\*BYO Activity Tracker SW \*\*·\*\*IBM Cloud Activity Tracker (VPC) | BYO Activity Tracker SW | \*\*·\*\*BYO Activity Tracker allows for the most flexibility in meeting activity tracking and auditing requirements. \*\*·\*\*When VPC is available, the preferred approach is IBM Cloud Activity Tracker |

Table 18. non-TGW service management auditing architecture decisions

## 

# References

\<Note to author\> \<example references below\>

[Placement Groups for VPC]()

ALB Multi-Zone Configuration

IBM Cloud SLA
