---

copyright:
  years: 2024
lastupdated: "2024-04-08"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Overview
{: #Overview-id}

 {{site.data.keyword.Bluemix}} contains three main environments that are known as Classic, {{site.data.keyword.vpc_full}}, and {{site.data.keyword.powerSysFull}}. The Classic environment is connected by a layer 2 network and can deploy resources that are not limited to {{site.data.keyword.BluVirtServers}}, {{site.data.keyword.BluBareMetServers}}, and dedicated {{site.data.keyword.vmwaresolutions_full_notm}}. {{site.data.keyword.vpc_full}} is a secure, isolated virtual network that combines the security of a private cloud with the availability and scalability of {{site.data.keyword.IBM_notm}}'s public cloud and supports x86 and VMware workloads. {{site.data.keyword.powerSys_notm}} is an IaaS offering that allows you to deploy your current {{site.data.keyword.IBM_notm}} i, AIX, and Linux workloads in a hybrid cloud environment, without refactoring. The objective of this pattern is to:

- Illustrate network connectivity to {{site.data.keyword.powerSys_notm}} and Classic environments when the data center does not have {{site.data.keyword.vpc_short}} infrastructure.
- Provide an {{site.data.keyword.IBM_notm}} solution design for the network elements required when deploying in a non-Transit Gateway (non-TGW) data centers or a data center that does not contain availability zones.
- Securely connect your external locations to {{site.data.keyword.dl_full}}, enabling access to both classic infrastructure and {{site.data.keyword.powerSys_notm}} resources.
- Focus on {{site.data.keyword.cloud_notm}} network elements, while ensuring that requirements can be met from a performance, system availability, and connectivity perspective.

This pattern is intended to:

- Accelerate and simplify solution design by providing a standard {{site.data.keyword.cloud_notm}} deployment architecture reference following the [IBM Architecture Framework](/docs/architecture-framework).
- Provide a prescriptive, end-to-end enterprise-class solution design, with diagrams, component architecture decisions along with rationale for cloud component selection to meet enterprise requirements.
