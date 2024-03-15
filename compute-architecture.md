---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-service

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}

The following are compute architecture decisions for the Network architecture for data centers without a {{site.data.keyword.tg_full_notm}} service pattern.

| **Architecture decision**        | **Requirement**                                                            | **Options**                                 | **Decision**   | **Rationale**                                    |
|----------------------------------|----------------------------------------------------------------------------|---------------------------------------------|----------------|--------------------------------------------------|
| Compute (Jump Host)              | Secure connection to manage internal systems and centralize access control | - Bare Metal  \n - Virtual Server | Virtual Server | flexible compute resources to meet compute needs |
| Compute (Proxy or Baston Server) | Provide access to Service Endpoints from non- {{site.data.keyword.cloud_notm}} IPs                 | - Bare Metal  \n - Virtual Server | Virtual Server | flexible compute resources to meet compute needs |
{: caption="Table 6. non-TGW Compute architecture decisions"}
