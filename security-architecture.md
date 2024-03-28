---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for security
{: #security-architecture}

The following are security architecture decisions for the hybrid cloud network for classic infrastructure disaster recovery pattern.

## Architecture decisions for identity and access management
{: #ad-iam}

| **Architecture decision**               | **Requirement**                                                                                                                                                                                                            | **Options**                                                                                             | **Decision**                    | **Rationale**                                                                                                                                                                                                                                                                                      |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Privileged Access Management            | Ensure that all operator actions are run securely through a bastion host  \n Implement session recording to track all activities and note any potential threats  \n Manage access to resources and track commands issued | - BYO Bastion Host  \n - Jump server  \n - BYO Bastion Host with Privileged Access Management (PAM) software | BYO Bastion Host or Jump server | The Bastion Host or Jump server is a Virtual Server instance that is provisioned through SSH over a private network to securely access resources within the IBM Cloud private network.  \n \n Using PAM software is recommended when session recording, tracking, and managing all access is required. |
| Identity Access & Role Management (IAM) | Securely authenticate users for platform services and control access to resources consistently across IBM Cloud                                                                                                            | {{site.data.keyword.iamshort}}                                                                                           | {{site.data.keyword.iamshort}}                   | Use IAM access policies to assign users, service IDs, and trusted profiles access to resources within the IBM Cloud account.                                                                                                                                                                       |
{: caption="Table 13. Classic Data Center security identity and access management architecture decisions"}
