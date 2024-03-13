---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-service

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
| High Availability Network path      | Ensure availability of resources if outages occurs. Support SLA targets for availability                              | - Single Direct Link Dedicated  \n - Single Direct Link Connect   \n - Two Direct Link Dedicated  \n - Two Direct Link Connect | Two Direct Link Connect                           | Two Direct Link Connect provides a layer of resiliency with SDN over physical hardware while being cost effective and flexible. |
| High Availability Gateway Appliance | Ensure availability of infrastructure resources if outages occur. Support SLA targets for infrastructure availability | Deploy Gateway Appliance of choice in HA Pair                                                                                        | Deploy Gateway Appliance of choice in HA Pair     | Ensures if one appliance unavailable access is still available through remaining gateway appliance.                             |
| High Availability Path Detection    | Ensure the quickest traffic path recovery                                                                             | Bidirectional Forwarding Detection (BFD)                                                                                             | Bidirectional Forwarding Detection (BFD) | provides a much faster way of detecting link failures compared to the built-in mechanisms within routing protocols              |
{: caption="Table 14. non-TGW resiliency architecture decisions"}

## Architecture decisions for disaster recovery
{: #ad-dr}

| **Architecture decision** | **Requirement**                                                                       | **Options**                                                                                                                                   | **Decision**               | **Rationale**                                                                                                       |
|---------------------------|---------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------------------------------------------------------------------------------------------------|
| Disaster Recovery Network | Network disaster recovery capability in secondary region to meet RTO/RPO requirements | - Single Direct Link Dedicated  \n - Single Direct Link Connect  \n - Two Direct Link Dedicated  \n - Two Direct Link Connect | Single Direct Link Connect | provides a cost effective and flexible connection into a second region, with metered and unmetered billing options. |
{: caption="Table 15. non-TGW disaster recovery architecture decisions"}
