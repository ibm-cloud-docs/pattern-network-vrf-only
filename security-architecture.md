---

copyright:
  years: 2023
lastupdated: "2023-12-18"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-service

keywords:

---

# Architecture decisions for security
{: #security-architecture}

The following are security architecture decisions for the Network architecture for data centers without a Transit Gateway service pattern.

##

## Architecture decisions for identity and access management

| **Architecture decision**               | **Requirement**                                                                                                                                                                                                            | **Options**                                                                                             | **Decision**                    | **Rationale**                                                                                                                                                                                                                                                                                      |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Privileged Access Management            | Ensure that all operator actions are run securely through a bastion host \\n - Implement session recording to track all activities and note any potential threats \\n Manage access to resources and track commands issued | **·** BYO Bastion Host or Jump Server **·** BYO Bastion Host with Privileged Access Management (PAM) SW | BYO Bastion Host or Jump Server | The Bastion Host or Jump server is a Virtual Server instance that is provisioned through SSH over a private network to securely access resources within IBM Cloud’s private network. **˙**Using PAM software is recommended when session recording, tracking, and managing all access is required. |
| Identity Access & Role Management (IAM) | Securely authenticate users for platform services and control access to resources consistently across IBM Cloud                                                                                                            | IBM Cloud IAM                                                                                           | IBM Cloud IAM                   | Use IAM access policies to assign users, service IDs, and trusted profiles access to resources within the IBM Cloud account.                                                                                                                                                                       |

Table 13. non-TGW security identity and access management architecture decisions
