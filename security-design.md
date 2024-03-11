---

copyright:
  years: 2023
lastupdated: "2023-12-26"

subcollection: pattern-sap-on-vpc

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Security design
{: #security-design}

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
