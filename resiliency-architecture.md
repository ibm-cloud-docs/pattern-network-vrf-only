---

copyright:
  years: 2024
lastupdated: "2024-10-02"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for resiliency
{: #ad-resiliency}

The following are resiliency architecture decisions for the hybrid cloud network for classic infrastructure disaster recovery pattern.

## Architecture decisions for Network Resiliency
{: #ad-network-resiliency}

| Architecture decision           | Requirement                                                                                                       | Options                                                                                                                                   | Decision                                      | Rationale                                                                                                                   |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| High availability network path      | Ensure availability of resources if outages occur. Support SLA targets for availability                              | - Single {{site.data.keyword.dl_full_notm}} Dedicated  \n - Single {{site.data.keyword.dl_full_notm}} Connect   \n - Two {{site.data.keyword.dl_full_notm}} Dedicated  \n - Two {{site.data.keyword.dl_full_notm}} Connect | Two {{site.data.keyword.dl_full_notm}} Connect                           | Two {{site.data.keyword.dl_full_notm}} Connect provides a cost effective resilient solution with a short deployment interval and is flexible to meet both hybrid and multi-cloud strategies. |
| High availability gateway appliance | Ensure availability of infrastructure resources if outages occur. Support SLA targets for infrastructure availability | Deploy Gateway Appliance of choice in high availability pair                                                                                        | Deploy Gateway Appliance of choice in high availability  pair     | Ensures if one appliance is unavailable access is still available through remaining gateway appliance.                             |
| High availability path detection    | Ensure the quickest traffic path recovery                                                                             | Bidirectional Forwarding Detection (BFD)                                                                                             | Bidirectional Forwarding Detection (BFD) | Provides a much faster way of detecting link failures compared to the built-in mechanisms within routing protocols.              |
{: caption="Table 1. Classic data center resiliency architecture decisions"}

## Architecture decisions for disaster recovery
{: #ad-dr}

| Architecture decision | Requirement                                                                       | Options                                                                                                                                   | Decision               | Rationale                                                                                                       |
|---------------------------|---------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------------------------------------------------------------------------------------------------|
| Disaster recovery network | Network disaster recovery capability in secondary region to meet recovery time objective (RTO) recovery point objective (RPO) requirements | - Single {{site.data.keyword.dl_full_notm}} Dedicated  \n - Single {{site.data.keyword.dl_full_notm}} Connect  \n - Two {{site.data.keyword.dl_full_notm}} Dedicated  \n - Two {{site.data.keyword.dl_full_notm}} Connect | Single {{site.data.keyword.dl_full_notm}} Connect | Provides a cost effective and flexible connection into a second region, with metered and unmetered billing options. |
| Global load balancing     | Load balancing over the public network across two regions if there's an outage (DR) for failover to the other region. | - {{site.data.keyword.cis_short}}   \n - {{site.data.keyword.vpx_full}} \n - DNS | {{site.data.keyword.cis_short}} | Provides a cost-effective solution and offers extra security features                                                                          |
{: caption="Table 2. Classic data center disaster recovery architecture decisions"}
