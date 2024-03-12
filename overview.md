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

IBM Cloud contains three main environments known as Classic, Virtual Private Cloud (VPC) and Power Virtual Server (PowerVS). The Classic environment is connected by a layer 2 network and can deploy resources not limited to Virtual Server Instances (VSI), Bare Metal Servers, and dedicated VMware. A Virtual Private Cloud (VPC) is a secure, isolated virtual network that combines the security of a private cloud with the availability and scalability of IBM's public cloud and supports x86 and VMware workloads. Power Virtual Server is an IaaS offering that allows you to deploy your current IBM i, AIX and Linux workloads in a hybrid cloud environment, without refactoring. The objective of this pattern is to:

-   Illustrate network connectivity to PowerVS and Classic environments when the data center does not have VPC infrastructure.
-   Provide an IBM Solution Design for the Network elements required when deploying in a non-Transit Gateway (non-TGW) data centers or a data center that does not contain availability zones.
-   Securely connect your external locations to IBM Cloud with Direct Link, enabling access to both Classic infrastructure and PowerVS resources.
-   Focus on IBM Cloud Network elements, while ensuring requirements can be met from a performance, system availability, and connectivity perspective.

This pattern is intended to:

-   Accelerate and simplify solution design by providing a standard IBM Cloud deployment architecture reference following the [IBM Architecture Framework](/docs/architecture-framework).
-   Provide a prescriptive, end-2-end enterprise-class solution design, with diagrams, component architecture decisions along with rationale for cloud component selection to meet enterprise requirements.
