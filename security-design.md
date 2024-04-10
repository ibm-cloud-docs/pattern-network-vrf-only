---

copyright:
  years: 2024
lastupdated: "2024-04-09"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-service

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Security design
{: #security-design}

## Identity and Access Management (IAM)
{: #IAM}

{{site.data.keyword.iamshort}} provides role-based access controls (RBAC) and is part of the zero trust strategy that allows for least privileged access to help support regulatory and compliancy requirements. {{site.data.keyword.IBM}} Security Verify can be added to support multi-factor authentication. For more information, see [How IAM works](/docs/account?topic=account-iamoverview).

To learn more about IBM Security Verify, see [IBM Security Verify](/verify).
{: note}

## Cloud Internet Services (CIS)
{: #CIS}

In addition to providing Global server load balancing (GSLB) and Domain name services, {{site.data.keyword.cis_full_notm}} (CIS) provides many security features to help meet compliance requirements in a Pay-As-You-Go or bundled package.

Consider using {{site.data.keyword.cis_full_notm}} or other third-party products such as Akamai, Cloudflare, Imperva, Barracuda, or F5 to meet security requirements:

- [DDoS Protection](/docs/cis?topic=cis-distributed-denial-of-service-ddos-attack-concepts): Shields your website from malicious attacks that flood it with traffic.
- [Web Application Firewall (WAF)](/docs/cis?topic=cis-waf-q-and-a): Acts as a security guard for your website, filtering out suspicious traffic and blocking known threats like SQL injections and code injection attempts.
- Transport Layer Security (TLS): Encrypts communication between your website and visitors, protecting sensitive data like passwords and credit card information.
- Range: non-HTTP and HTTPS port protection secures ports on your server beyond the standard web traffic ports (HTTP and HTTPS), protecting against attacks targeting vulnerable services.

For more information, see [About IBM Cloud Internet Services](/docs/cis?topic=cis-about-ibm-cloud-internet-services-cis).

## Gateway appliance: Firewall
{: #gateway-appliance}

IBM Cloud classic firewalls offer various security functions to protect your cloud resources. Specific features and capabilities of {{site.data.keyword.Bluemix_notm}} classic firewalls vary depending on the vendor, hardware, software, licenses, add-on bundle, and configuration options selected.

Before making an appliance choice, consider existing vendor relationships and operation teams’ technical expertise.

In addition, consider the following needs:

- Number of users and devices: How many devices are connected to the network? A small business network has different needs than a large business network.
- Types of devices: Consider the variety of resources, data, and applications connected to the network.
- Bandwidth requirements: How much data traffic will the network typically handle?
- Scalability: Choose a firewall that can grow with your network needs.
- Type of firewall: Different firewall types offer different levels of protection. Consider stateful inspection firewalls, application-level firewalls, or next-generation firewalls (NGFWs) depending on the security need.
- Security features: Look for features like intrusion detection and prevention systems (IDS and IPS), deep packet inspection (DPI), malware protection, and content filtering.
- Threat intelligence: Choose a firewall that receives regular updates on the latest threats and vulnerabilities.

Consider the security level required based on the business need:

* Tier 1: Public Front-end Enterprise
* Tier 2: General Internal Enterprise
* Tier 3: Business Critical Enterprise
* Tier 4: Highly sensitive Enterprise
* Tier 5: Ultra-Secure Enterprise

| Security function                                                    | Tier 1 | Tier 2 | Tier 3 | Tier 4 | Tier 5 |
|--------------------------------------------------------------------------|------------|------------|------------|------------|------------|
| Network Firewall                                                         | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Web Application Firewall (WAF)                                           | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Intrusion Detection System (IDS)                                         | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Antivirus/Antimalware Software                                           | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Network segmentation                                                     |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Administrative role-based access controls (RBAC)                         |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Virtual Private Network (VPN)                                            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |            |            |
| Advanced Firewall with Deep Packet Inspection                            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Intrusion Prevention System (IPS)                                        |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Multifactor Authentication (MFA)                                         |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Integration with Security Incident and Event Management (SIEM) System    |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Network segmentation with Strict Access Controls                         |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Administrative Privileged Access Management (PAM)                        |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Integration with Advanced Threat Intelligence and Threat Hunting systems |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Zero trust Architecture                                                  |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Micro-Segmentation                                                       |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Continuous Monitoring and Auditing                                       |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Integration with Advanced Threat Detection and Response (ATDR) Systems   |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Integration with Real-Time Incident Response Capabilities                |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
{: caption="Table 1: Non-TGW Security features"}

### Firewall references 
{: #firewall-links}

1. Explore [{{site.data.keyword.vra}} (vRA)](/docs/virtual-router-appliance?topic=virtual-router-appliance-getting-started-vra).
2. Explore [{{site.data.keyword.vsrx_full}} license features here](/docs/vsrx?topic=vsrx-getting-started#choosing-license).
3. Explore [Bring Your Own Gateway Appliance (BYOG) to support Checkpoint, Fortinet, and Palo Alto](/docs/gateway-appliance?topic=gateway-appliance-order-byoa).

## VPN security
{: #vpn}

VPN offers a valuable layer of security for public internet activities.

Key VPN Security Considerations include:

- Encryption strength: Choose a VPN with strong encryption algorithms like AES-256. Weak encryption can be cracked, exposing your data.
- Protocol choice: Protocols like OpenVPN and IKEv2/IPsec offer strong security and performance. Avoid outdated or less secure protocols like PPTP.
- Authentication: Consider strong authentication methods like multi-factor authentication for secure data transmission.
- Access control: Granularly control access permissions to resources accessible through the VPN, limiting potential damage from breaches.
- Security of endpoints: Ensure that all connected devices comply with security policies and are kept up to date with patches.

## Service endpoints
{: #service-endpoints}

With {{site.data.keyword.Bluemix_notm}} service endpoints, you can connect to {{site.data.keyword.Bluemix_notm}} services over the {{site.data.keyword.Bluemix_notm}} private network instead of the default public network. Moving these workloads from the public network to the private network offers enhanced security as cloud services are no longer served on an internet routable IP address.

## Jump server or bastion host
{: #jump-bastion}

Jump servers or bastion hosts can offer more levels of security and control and can be deployed on bare metal or virtual server instances within {{site.data.keyword.Bluemix_notm}} classic.

Considerations include:

1. Enhanced security required: All access to internal systems funnels through the jump server or Bastion host, making it easier to monitor and enforce security policies. Access can be granted or revoked to specific users and systems with granularity. By keeping internal systems directly inaccessible from the outside,it minimizes potential entry points for attackers. Hackers would need to compromise the jump server first, adding an extra layer of defense.
2. Improved management and auditing: Manage access to all internal systems from a single point, streamlining the process and reduce errors. All activity on the jump server or Bastion host is logged, providing a centralized record of who accessed what and when. This helps with troubleshooting, security audits, and forensic investigations.
3. Secure access to legacy systems: Jump servers and bastion hosts can act as a bridge between modern tools and legacy systems that might not support secure protocols like SSH. You can use the jump server to tunnel secure connections to older systems.
4.  Multi-factor Authentication (MFA): MFA can be implemented on the jump server or bastion host itself, adding another layer of protection before granting access to internal systems.

The following table can help determine whether a bastion or jumper server is needed:

| Feature    | Jump server                                                       | Bastion host                                                        |
|----------------|-----------------------------------------------------------------------|-------------------------------------------------------------------------|
| Location       | Internal network                                                      | Network perimeter (DMZ and public subnet)                               |
| Purpose        | Manage internal systems                                               | Grant controlled access to specific internal systems for external users |
| Security focus | Centralized access control, which is simplified administration                 | Secure entry point, isolation of internal systems                       |
| Attack surface | Higher (internal systems directly exposed if jump server compromised) | Lower (internal systems protected even if bastion host is compromised)     |
{: caption="Table 2. non-TGW Jump Server vs Bastion Host Matrix"}
