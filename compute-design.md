---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-service

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Compute design
{: #compute-design}

## Workload Hosts (Classic and {{site.data.keyword.powerSys_notm}})
{: #workload-hosts}

Selecting the appropriate workload host in this architecture includes but is not limited to the following considerations regarding:

-   Performance (CPU, Memory, storage options and network speed)
-   Virtualization & Tenancy (container, virtual or bare metal, multitenant or dedicated)
-   Security, availability, and cost considerations
-   Application-specific requirements regarding certification or optimization

Based on workload and specific requirements, select from a range of Virtual Servers, Bare Metal servers, containers and VMware based solutions.

Explore [{{site.data.keyword.baremetal_short}}](/docs/bare-metal?topic=bare-metal-about-bm) for Classic.

Explore [{{site.data.keyword.virtualmachinesshort}}](/docs/virtual-servers?topic=virtual-servers-about-virtual-servers) for Classic.

Explore [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cluster-create-classic&interface=ui) and [{{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-getting-started) for classic environments.

Explore [{{site.data.keyword.vmwaresolutions_full_notm}}](/docs/vmwaresolutions?topic=vmwaresolutions-getting-started).

Explore [{{site.data.keyword.powerSys_notm}}](/docs/power-iaas?topic=power-iaas-getting-started).

## Jump Server and Bastion Hosts
{: #jump-bastion}

Infrastructure requirements for Linux and Windows jump servers and Bastion host can vary based on factors such as the number of concurrent users, the specific use case, and the applications or services running on the server. Here are some general guidelines:

An average Jump server with 8 CPU and 16GB of RAM, can support 25 concurrent sessions of any type (100 serial-over-LAN sessions, 200 Telnet or SSH sessions). Additional sessions are supported depending on the session types or by higher server specifications.

Infrastructure requirements for Ubuntu LTS, CentOS, or Debian Bastion Hosts can be viewed below.

Table 3 contains general jump server and Bastion Host sizing guidelines:

| **Traffic** | **Concurrent Users** | **Server Size**               |
|-------------|----------------------|-------------------------------|
| Low         | 10-20                | 1-2 Cores CPU; 2-4 GB Memory  |
| Moderate    | 30-50                | 2-4 Cores CPU; 4-8 GB Memory  |
| High        | 50+                  | 4-8 Cores CPU; 8-16 GB Memory |
{: caption="Table 3. non-TGW Bastion Server Sizing"}

The Jump server or Bastion Host can be deployed on either a {{site.data.keyword.baremetal_short_sing}} or {{site.data.keyword.BluVirtServers_short} Instance (VSI) within the Classic environment.
