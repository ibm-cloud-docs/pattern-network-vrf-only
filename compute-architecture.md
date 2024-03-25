---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}

The following are compute architecture decisions for the Network architecture for data centers without a {{site.data.keyword.tg_full_notm}} service pattern.

| **Architecture decision**        | **Requirement**                                                            | **Options**                                 | **Decision**   | **Rationale**                                    |
|----------------------------------|----------------------------------------------------------------------------|---------------------------------------------|----------------|--------------------------------------------------|
| Compute (Bastion Host)              | Secure connection to manage internal systems and centralized access control | - Bare Metal  \n - Virtual Server | Virtual Server | Flexible compute resources to meet compute needs |
| Compute (Proxy Server) | Provide access to Service Endpoints from IPs not assigned by {{site.data.keyword.cloud_notm}}                 | - Bare Metal  \n - Virtual Server | Virtual Server | Flexible compute resources to meet compute needs |
{: caption="Table 6. Classic Data Center Compute architecture decisions"}
