---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Compute design
{: #compute-design}

## Workload Hosts (Classic and {{site.data.keyword.powerSys_notm}})
{: #workload-hosts}

Selecting the appropriate workload host in this architecture includes but is not limited to the following considerations:

-   Performance (CPU, Memory, storage options and network speed)
-   Virtualization & Tenancy (container, virtual or bare metal, multitenant or dedicated)
-   Security, availability, and cost considerations
-   Application-specific certification or optimization requirements

Based on workload and specific requirements, select from a range of Virtual Servers, Bare Metal servers, containers, and VMware-based solutions.

Explore [{{site.data.keyword.baremetal_short}}](/docs/bare-metal?topic=bare-metal-about-bm) for Classic.

Explore [{{site.data.keyword.virtualmachinesshort}}](/docs/virtual-servers?topic=virtual-servers-about-virtual-servers) for Classic.

Explore [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cluster-create-classic&interface=ui) and [{{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-getting-started) for classic environments.

Explore [{{site.data.keyword.vmwaresolutions_full_notm}}](/docs/vmwaresolutions?topic=vmwaresolutions-getting-started).

Explore [{{site.data.keyword.powerSys_notm}}](/docs/power-iaas?topic=power-iaas-getting-started).

## Jump server and Bastion Hosts
{: #jump-bastion}

Infrastructure requirements for Linux and Windows jump servers and Bastion hosts can vary based on factors such as the number of concurrent users, the specific use case, and the applications or services that run on the server. Following are general guidelines:

An average jump server with 8 CPU and 16 GB of RAM, can support 25 concurrent sessions of any type (100 serial-over-LAN sessions, 200 Telnet, or SSH sessions). More sessions can be supported with larger server specifications.

Table 3 contains general jump server and Bastion Host sizing guidelines for Ubuntu LTS, CentOS, or Debian Bastion Hosts:

| **Traffic** | **Concurrent Users** | **Server Size**               |
|-------------|----------------------|-------------------------------|
| Low         | 10-20                | 1-2 Cores CPU; 2-4 GB Memory  |
| Moderate    | 30-50                | 2-4 Cores CPU; 4-8 GB Memory  |
| High        | 50+                  | 4-8 Cores CPU; 8-16 GB Memory |
{: caption="Table 3. Classic Data Center Bastion Server Sizing"}

The jump server or Bastion Host can be deployed on either a {{site.data.keyword.baremetal_short_sing}} or {{site.data.keyword.BluVirtServers_short}} Instance (VSI) within the Classic environment. This pattern will use a VSI to deploy a Bastion host.

## Proxy server
{: #proxy}

The proxy server acts as an intermediary between the on-premise network and the IBM cloud services.

Some key design considerations for a proxy server include:

-	**Proxy Type:** Choose between a forward proxy (clients access internet through it) or a reverse proxy (sits in front of web servers for load balancing or security).
-	**Protocol Support:** Ensure the proxy supports the protocols used by the clients and the servers it communicates with (HTTP, HTTPS, FTP, etc.).
-	**Content Transformation:** Decide if the proxy needs to modify content (e.g., compression, encryption) before sending it to clients.
-	**Caching:** Determine how aggressively the proxy will cache content to improve performance and reduce bandwidth usage.
-	**Security:** Consider features like access control, encryption (SSL/TLS), and content filtering to protect your network.
-	**Logging and Monitoring:** Decide what data to log for troubleshooting, security audits, and usage analysis.
-	**Hardware:** Select hardware with sufficient processing power, memory, and network bandwidth to handle expected traffic.
-	**Scalability:** Design the architecture to accommodate future growth in users and traffic volume.
-	**High Availability:** Consider redundancy measures to ensure the proxy remains operational in case of failures.

Sizing considerations include:

-	**Number of Users:** More users will require a more powerful processor and more memory to handle concurrent connections.
-	**Traffic Volume:** Higher traffic volume demands faster processors, more RAM, and potentially higher network bandwidth.
-	**Proxy Functionality:** Features like caching, security (encryption), and content filtering can increase resource consumption.

General sizing guidelines include:

-	**CPU:** A multi-core processor (4 or more cores) is recommended for efficient handling of multiple connections.
-	**Memory:** 4GB or more is a good starting point, more memory may be needed for very demanding scenarios. A rule of thumb is 32MB of RAM for every 1GB of disk space for caching purposes.
-	**Disk Space:** Enough space to store the operating system, proxy server software, and potentially cached data. The amount of cache storage depends on your caching strategy.
-	**Network Interface:** A high-bandwidth network interface card (NIC) is crucial for handling incoming and outgoing traffic efficiently.

The proxy server can be deployed on either a {{site.data.keyword.baremetal_short_sing}} or {{site.data.keyword.BluVirtServers_short}} Instance (VSI) within the Classic environment. This pattern will use a VSI to deploy the proxy server.
