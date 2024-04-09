---

copyright:
  years: 2024
lastupdated: "2024-04-09"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-service

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for security
{: #security-architecture}

The following are security architecture decisions for the network architecture for data centers without a Transit Gateway service pattern.

## Architecture decisions for identity and access management
{: #ad-iam}

| Architecture decision               | Requirement                                                                                                                                                                                                            | Options                                                                                             | Decision                    | Rationale                                                                                                                                                                                                                                                                                      |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Privileged Access Management (PAM)           | Ensure that all operator actions are run securely through a bastion host  \n Implement session recording to track all activities and note any potential threats \n Manage access to resources and track commands issued | - Bring Your Own bastion host  \n - Jump server  \n - Bring Your Own bastion host with Privileged Access Management (PAM) SW | Bring Your Own bastion host or jump server | The bastion host or jump server is a Virtual Server instance that is provisioned through SSH over a private network to securely access resources within {{site.data.keyword.Bluemix_notm}}’s private network. \n \n Using PAM software is recommended when session recording, tracking, and managing all access is required. |
| Identity Access & Role Management (IAM) | Securely authenticate users for platform services and control access to resources consistently across {{site.data.keyword.Bluemix_notm}}                                                                                                            | {{site.data.keyword.iamshort}}                                                                                           | {{site.data.keyword.iamshort}}                   | Use IAM access policies to assign users, service IDs, and trusted profiles access to resources within the {{site.data.keyword.Bluemix_notm}} account.                                                                                                                                                                       |
{: caption="Table 2. non-TGW security identity and access management architecture decisions"}
