---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Overview
{: #Overview-id}

 {{site.data.keyword.Bluemix}} contains three main environments that are known as Classic, {{site.data.keyword.vpc_full}}, and {{site.data.keyword.powerSysFull}}. The Classic environment is connected by a layer 2 network and can deploy resources that are not limited to {{site.data.keyword.BluVirtServers}}, {{site.data.keyword.BluBareMetServers}}, and dedicated {{site.data.keyword.vmwaresolutions_full_notm}}. {{site.data.keyword.vpc_full}}  is a secure, isolated virtual network that combines the security of a private cloud with the availability and scalability of IBM's public cloud and supports x86 and VMware workloads. {{site.data.keyword.powerSys_notm}} is an IaaS offering where IBM i, AIX, and Linux workloads are deployed in a hybrid cloud environment, without refactoring. The objective of this pattern is to:

-   Illustrate network connectivity to {{site.data.keyword.powerSysShort}} Workspace and Classic infrastructure resources.
-   Provide an IBM Solution Design for the Network elements that are required when deploying in Classic Data Centers or a data center that does not contain availability zones.
-   Securely connect your external locations to {{site.data.keyword.dl_full}}, enabling access to both Classic infrastructure and {{site.data.keyword.powerSys_notm}} resources.
-   To focus on {{site.data.keyword.cloud_notm}} Network elements, while ensuring requirements can be met from a performance, system availability, and connectivity perspective.

This pattern is intended to:

-   Accelerate and simplify solution design by providing a standard  {{site.data.keyword.cloud_notm}} deployment architecture reference by following the [IBM Architecture Framework](/docs/architecture-framework).
-   Provide a prescriptive, end-2-end enterprise-class solution design, with diagrams, component architecture decisions along with rationale for cloud component selection to meet enterprise requirements.
