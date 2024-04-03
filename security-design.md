---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Security design
{: #security-design}

## Identity and Access Management (IAM)
{: #IAM}

{{site.data.keyword.iamshort}} provides role-based access controls (RBAC) and is part of the zero-trust strategy that allows for least privileged access to help support regulatory and compliancy requirements. IBM Security Verify can be added to support multi-factor authentication.

Click to learn more about [{{site.data.keyword.iamshort}} (IAM)](/docs/account?topic=account-iamoverview).

Click to learn more about [IAM Roles](/docs/account?topic=account-userroles).

Click to learn more about [IBM Security Verify](https://www.ibm.com/verify) {: external} .

## Cloud Internet Services (CIS)
{: #CIS}

In addition to providing Global Server Load balancing and Domain name services, {{site.data.keyword.cis_full_notm}} (CIS) provides many security features to help meet compliance requirements as either a pay as you go or bundled service package option.

Consider leveraging {{site.data.keyword.cis_full_notm}} or other third-party products such as Akamai, Cloudflare, Imperva, Barracuda, or F5 to meet security requirements such as:

-   **DDoS Protection:** Shields your website from malicious attacks that flood it with traffic.
-   **Web Application Firewall (WAF):** Acts as a security guard for your website, filtering out suspicious traffic and blocking known threats like SQL injections and code injection attempts.
-   **Transport Layer Security (TLS):** Encrypts communication between your website and visitors, protecting sensitive data like passwords and credit card information.
-   **Range**: non-HTTP and HTTPS port protection Secures ports on your server beyond the standard web traffic ports (HTTP and HTTPS), protecting against attacks that target vulnerable services.

In this pattern {{site.data.keyword.cis_full_notm}} DDos and WAF features are used to meet security requirments.

Learn more about the Cloud Internet Service features [here](/docs-draft/cis?topic=cis-about-ibm-cloud-internet-services-cis).

Explore {{site.data.keyword.cis_full_notm}} [DDoS](/docs/cis?topic=cis-distributed-denial-of-service-ddos-attack-concepts).

Explore {{site.data.keyword.cis_full_notm}} [WAF](/docs/cis?topic=cis-waf-q-and-a).

## Gateway Appliance – Firewall
{: #security-gateway-appliance}

IBM Cloud Classic Firewalls offer various security functions to protect your cloud resources. Specific features and capabilities of IBM Cloud Classic firewalls vary depending on the vendor, hardware, software, licenses, add-on bundle, and configuration options selected.

Consider existing vendor relationships and operation teams’ technical expertise before making an appliance choice.

In addition, consider the following needs:

-   **Number of users and devices:** How many devices are connected to the network? A small business network has different needs than a large business network.
-   **Types of devices:** Consider the variety of resources, data, and applications connected to the network.
-   **Bandwidth requirements:** How much data traffic does the network typically handle?
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
| Network Firewall                                                         | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Web Application Firewall (WAF)                                           | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Intrusion Detection System (IDS)                                         | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Antivirus/Antimalware Software                                           | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Network segmentation                                                     |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Administrative Role-Based Access Controls (RBAC)                         |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Virtual Private Network (VPN)                                            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |            |            |
| Advanced Firewall with Deep Packet Inspection                            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Intrusion Prevention System (IPS)                                        |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Multifactor Authentication (MFA)                                         |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Integration with Security Incident and Event Management (SIEM) System    |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Network segmentation with Strict Access Controls                         |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Administrative Privileged Access Management (PAM)                        |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Integration with Advanced Threat Intelligence and Threat Hunting systems |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Zero-Trust Architecture                                                  |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Micro-Segmentation                                                       |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Continuous Monitoring and Auditing                                       |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Integration with Advanced Threat Detection and Response (ATDR) Systems   |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
| Integration with Real-Time Incident Response Capabilities                |            |            |            |            | ![Checkmark icon](../../icons/checkmark-icon.svg)          |
{: caption="Table 4: Classic Data Center Security features"}

IBM Classic data centers support four gateway appliance and firewall options including Juniper vSRX, Virtual Router Appliance, FortiGate (FSA 10 Gbps and vFSA), and bring your own gateway appliance (BYOG - Checkpoint, Cisco, and Palo Alto).  This pattern supports personal choice based on security requirements and operational expertise.

Explore {{site.data.keyword.vra}} (vRA) [here](/docs/virtual-router-appliance?topic=virtual-router-appliance-getting-started-vra).

Explore {{site.data.keyword.vsrx_full}} license features [here](/docs/vsrx?topic=vsrx-getting-started#choosing-license).

Explore FortiGate FSA 10Gbps [here](/docs/fortigate-10g?topic=fortigate-10g-getting-started).

Explore Virtual FortiGate Security Appliance (vFSA) [here](/docs/vfsa?topic=vfsa-getting-started)

Explore Bring Your Own Gateway Appliance (BYOG) to support Checkpoint, Cisco, and Palo Alto [here](/docs/gateway-appliance?topic=gateway-appliance-order-byoa).

## VPN Security
{: #vpn-security}

VPN offers a valuable layer of security for public internet activities.

Key VPN Security Considerations include:

-   **Encryption Strength:** Choose a VPN with strong encryption algorithms like AES-256. Weak encryption can be cracked, exposing your data.
-   **Protocol Choice:** Popular protocols like OpenVPN, IKEv2, and IPsec offer strong security and performance. Avoid outdated or less secure protocols like PPTP.
-   **Authentication:** Consider strong authentication methods like multi-factor authentication for secure data transmission.
-   **Access Control:** Granularly control access permissions to resources accessible through the VPN, limiting potential damage from breaches.
-   **Security of Endpoints:** Ensure that all connected devices comply with security policies and are kept up-to-date with patches.

IBM Classic data centers offer three options for implementing a virtual private network connection from a remote site into IBM cloud, including SSL VPN, IPsec VPN, and VPN gateway appliance on Classic. This pattern supports the VPN gateway appliance on Classic to meet the private administrative and management connectivity requirements.

## Service Endpoints
{: #security-service-endpoints}

With IBM Cloud service endpoints, you can connect to IBM Cloud services over the IBM Cloud Private network instead of the default public network. Moving these workloads from the public network to the private network offers enhanced security as Cloud Services are no longer served on an internet routable IP address.

## Jump Server or Bastion Host
{: #security-jump-bastion}

Jump servers or Bastion hosts can offer extra levels of security and control and can be deployed on bare metal or virtual server instances within IBM Cloud Classic.

Considerations include:

1.  Enhanced Security required - All access to internal systems funnels through the jump server or Bastion host, making it easier to monitor and enforce security policies. Access can be granted or revoked to specific users and systems with granularity. By keeping internal systems directly inaccessible from the outside, it minimizes potential entry points for attackers. Hackers would need to compromise the jump server first, adding an extra layer of defense.
2.  Improved Management and Auditing - Manage access to all internal systems from a single point, streamlining the process and reduce errors. All activity on the jump server or Bastion host is logged, providing a centralized record of who accessed what and when. This helps with troubleshooting, security audits, and forensic investigations.
3.  Secure Access to Legacy Systems - Jump servers and Bastion hosts can act as a bridge between modern tools and legacy systems that might not support secure protocols like SSH. You can use the jump server to tunnel secure connections to older systems.
4.  Multi-factor Authentication (MFA) - MFA can be implemented on the jump server or Bastion host itself, adding another layer of protection to internal system access.

The following table helps determine whether a bastion host or jumper server is needed:

| **Feature**    | **Jump Server**                                                       | **Bastion Host**                                                        |
|----------------|-----------------------------------------------------------------------|-------------------------------------------------------------------------|
| Location       | Internal Network                                                      | Network perimeter (DMZ and public subnet)                               |
| Purpose        | Manage internal systems                                               | Grant controlled access to specific internal systems for external users |
| Security Focus | Centralized access control, which simplifies administration                 | Secure entry point, isolation of internal systems                       |
| Attack Surface | Higher (internal systems directly exposed if the jump server is compromised) | Lower (internal systems protected even if the bastion host is compromised)     |
{: caption="Table 5. Classic Data Center Jump server vs Bastion Host Matrix"}

In this pattern, a Bastion host is deployed on a virtual server instance in a classic data center to control secure remote administrative access of all devices within the IBM Cloud environment.
