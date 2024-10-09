---

copyright:
  years: 2024
lastupdated: "2024-10-09"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}

The following are compute architecture decisions for the hybrid cloud network for classic infrastructure disaster recovery pattern.

| Architecture decision        | Requirement                                                            | Options                                 | Decision   | Rationale                                    |
|----------------------------------|----------------------------------------------------------------------------|---------------------------------------------|----------------|--------------------------------------------------|
| Compute: Bastion host              | Secure connection to manage internal systems and centralized access control | - Bare Metal  \n - Virtual Server | Virtual Server | Flexible compute resources to meet compute needs |
| Compute: Proxy server | Provide access to service endpoints from IPs not assigned by {{site.data.keyword.cloud_notm}}                 | - Bare Metal  \n - Virtual Server | Virtual Server | Flexible compute resources to meet compute needs |
{: caption="Classic data center compute architecture decisions"}
