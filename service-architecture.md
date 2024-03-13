---

copyright:
  years: 2024
lastupdated: "2024-03-11"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-service

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for service management
{: #ad-service-management}

The following are service management architecture decisions for the Network architecture for data centers without a Transit Gateway service pattern.

## Architecture decisions for monitoring
{: #ad-monitoring}

| **Architecture decision**                                   | **Requirement**                                                                                          | **Options**                                                                                          | **Decision**              | **Rationale**                                                                                                                                                                        |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Operational Monitoring of Cloud infrastructure and services | Monitor system health to detect issues that might impact the availability of the system and application. | - IBM Cloud Health Dashboard  \n - BYO Monitoring Tool  \n - IBM Cloud Monitoring (VPC) | IBM Cloud Heath Dashboard | - IBM Cloud Heath Dashboard reports health and vitality of cloud infrastructure and services.  \n \n When VPC is available, the preferred approach is IBM Cloud Monitoring. |
{: caption="Table 16. non-TGW service management monitoring architecture decisions"}

## Architecture decisions for logging
{: #ad-logging}

| **Architecture decision**                           | **Requirement**                                                                                             | **Options**                                                | **Decision**     | **Rationale**                                                                                                                                                               |
|-----------------------------------------------------|-------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Log Monitoring of Cloud infrastructure and services | Monitor operational logs to detect issues that might impact the availability of the system and application. | - BYO Logging Tool  \n - IBM Cloud Logging (VPC) | BYO Logging Tool | - BYO Logging tool allows for the most flexibility in meeting log monitoring requirements.  \n \n - When VPC is available, the preferred approach is IBM Cloud Logging. |
{: caption="Table 17. non-TGW service management logging architecture decisions"}

## Architecture decisions for auditing
{: #ad-auditing}

| **Architecture decision** | **Requirement**                                                                                | **Options**                                                                | **Decision**            | **Rationale**                                                                                                                                                                                              |
|---------------------------|------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Audit Logging             | Monitor audit logs to track changes to cloud resources and detect potential security problems. | - BYO Activity Tracker SW  \n - IBM Cloud Activity Tracker (VPC) | BYO Activity Tracker SW | - BYO Activity Tracker allows for the most flexibility in meeting activity tracking and auditing requirements.  \n \n - When VPC is available, the preferred approach is IBM Cloud Activity Tracker |
{: caption="Table 18. non-TGW service management auditing architecture decisions"}
