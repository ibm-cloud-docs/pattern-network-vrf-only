---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for resiliency
{: #ad-resiliency}

The following are resiliency architecture decisions for the Network architecture for data centers without a Transit Gateway service pattern.

## Architecture decisions for Network Resiliency
{: #ad-network-resiliency}

| **Architecture decision**           | **Requirement**                                                                                                       | **Options**                                                                                                                                   | **Decision**                                      | **Rationale**                                                                                                                   |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| High Availability Network path      | Ensure availability of resources if outages occur. Support SLA targets for availability                              | - Single {{site.data.keyword.dl_full_notm}} Dedicated  \n - Single {{site.data.keyword.dl_full_notm}} Connect   \n - Two {{site.data.keyword.dl_full_notm}} Dedicated  \n - Two {{site.data.keyword.dl_full_notm}} Connect | Two {{site.data.keyword.dl_full_notm}} Connect                           | Two {{site.data.keyword.dl_full_notm}} Connect provides a layer of resiliency with SDN over physical hardware, is cost effective, and flexible. |
| High Availability Gateway Appliance | Ensure availability of infrastructure resources if outages occur. Support SLA targets for infrastructure availability | Deploy Gateway Appliance of choice in HA Pair                                                                                        | Deploy Gateway Appliance of choice in HA Pair     | Ensures if one appliance is unavailable access is still available through remaining gateway appliance.                             |
| High Availability Path Detection    | Ensure the quickest traffic path recovery                                                                             | Bidirectional Forwarding Detection (BFD)                                                                                             | Bidirectional Forwarding Detection (BFD) | Provides a much faster way of detecting link failures compared to the built-in mechanisms within routing protocols.              |
{: caption="Table 14. Classic Data Center resiliency architecture decisions"}

## Architecture decisions for disaster recovery
{: #ad-dr}

| **Architecture decision** | **Requirement**                                                                       | **Options**                                                                                                                                   | **Decision**               | **Rationale**                                                                                                       |
|---------------------------|---------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------------------------------------------------------------------------------------------------|
| Disaster Recovery Network | Network disaster recovery capability in secondary region to meet RTO/RPO requirements | - Single {{site.data.keyword.dl_full_notm}} Dedicated  \n - Single {{site.data.keyword.dl_full_notm}} Connect  \n - Two {{site.data.keyword.dl_full_notm}} Dedicated  \n - Two {{site.data.keyword.dl_full_notm}} Connect | Single {{site.data.keyword.dl_full_notm}} Connect | Provides a cost effective and flexible connection into a second region, with metered and unmetered billing options. |
{: caption="Table 15. Classic Data Center disaster recovery architecture decisions"}
