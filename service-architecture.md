---

copyright:
  years: 2025
lastupdated: "2025-01-25"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for service management
{: #ad-service-management}

The following are service management architecture decisions for the Network architecture for data centers without a {{site.data.keyword.tg_full_notm}} service pattern.

## Architecture decisions for monitoring
{: #ad-monitoring}

| Architecture decision                                   | Requirement                                                                                          | Options                                                                                          | Decision              | Rationale                                                                                                                                                                        |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Operational monitoring of cloud infrastructure and services | Monitor system health to detect issues that might impact the availability of the system and application. | - {{site.data.keyword.Bluemix_notm}} Health Dashboard  \n - Bring Your Own monitoring tool  \n - {{site.data.keyword.monitoringlong_notm}} (VPC) | {{site.data.keyword.Bluemix_notm}} Heath Dashboard | - {{site.data.keyword.Bluemix_notm}} Heath Dashboard reports the health and vitality of cloud infrastructure and services. \n \n When VPC is available, the preferred approach is {{site.data.keyword.monitoringlong_notm}}. |
{: caption="Classic data center service management monitoring architecture decisions"}

## Architecture decisions for logging
{: #ad-logging}

| Architecture decision                           | Requirement                                                                                             | Options                                                | Decision     | Rationale                                                                                                                                                               |
|-----------------------------------------------------|-------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Log monitoring of cloud infrastructure and services | Monitor operational logs to detect issues that might impact the availability of the system and application. | - Bring Your Own logging tool  \n - {{site.data.keyword.logs_full_notm}} VPC | Bring Your Own logging tool | - Bring Your Own logging tool allows for the most flexibility in meeting log monitoring requirements. \n - When VPC is available, the preferred approach is {{site.data.keyword.logs_full_notm}}. |
{: caption="Classic data center service management architecture decisions"}

## Architecture decisions for auditing
{: #ad-auditing}

| Architecture decision | Requirement                                                                                | Options                                                                | Decision            | Rationale                                                                                                                                                                                              |
|---------------------------|------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Audit logging             | Monitor audit logs to track changes to cloud resources and detect potential security problems. | - Bring Your Own Activity Tracker software  \n - {{site.data.keyword.logs_full_notm}} (VPC) | Bring Your Own Activity Tracker software | - Bring Your Own Activity Tracker allows for the most flexibility in meeting activity tracking and auditing requirements. \n - When VPC is available, the preferred approach is {{site.data.keyword.logs_full_notm}} |
{: caption="Classic data center service management auditing architecture decisions"}
