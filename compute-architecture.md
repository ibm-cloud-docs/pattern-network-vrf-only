---

copyright:
  years: 2023
lastupdated: "2023-12-26"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-serviceattern-sap-on-vpc

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}

## Architecture decisions for compute

The following are compute architecture decisions for the Network architecture for data centers without a Transit Gateway service pattern.

| **Architecture decision**        | **Requirement**                                                            | **Options**                                 | **Decision**   | **Rationale**                                    |
|----------------------------------|----------------------------------------------------------------------------|---------------------------------------------|----------------|--------------------------------------------------|
| Compute (Jump Host)              | Secure connection to manage internal systems and centralize access control | \*\*·\*\*Bare Metal \*\*·\*\*Virtual Server | Virtual Server | flexible compute resources to meet compute needs |
| Compute (Proxy or Baston Server) | Provide access to Service Endpoints from non-IBM Cloud IPs                 | \*\*·\*\*Bare Metal \*\*·\*\*Virtual Server | Virtual Server | flexible compute resources to meet compute needs |

Table 6. non-TGW Compute architecture decisions
