---

copyright:
  years: 2025
lastupdated: "2025-01-24"

subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Service management design
{: #service-mgmt}

The following are service management design considerations for the hybrid cloud network for classic infrastructure disaster recovery pattern.

## Monitoring
{: #monitoring}

Evaluate the level of monitoring that's required by considering the workload criticality and the metrics that need to be observed. Basic instance monitoring that is deployed in {{site.data.keyword.Bluemix}} classic environments can be viewed from the cloud portal health dashboard. To locate this information, from the Health dashboard, click Device, and then click Usage. More advanced monitoring capabilities such as troubleshooting, events and alerts, and custom dashboards can be used by integrating the cloud resources with {{site.data.keyword.monitoringlong_notm}} complementary VPC service. The agent or the Windows Prometheus Bundle are installed on the devices. Alternatively, third-party monitoring software can be deployed within IBM Cloud Classic for more detailed monitoring. For more information, see [{{site.data.keyword.monitoringlong_notm}}](/docs/monitoring?topic=monitoring-getting-started#getting-started).

## Log Analysis
{: #log-analysis}

Examine what logs are required for troubleshooting and auditing, as well as the retention policy necessary to meet the audit and compliance requirements. Third-party software can be implemented within {{site.data.keyword.Bluemix_notm}} classic to enable log analysis within {{site.data.keyword.Bluemix_notm}} classic. Alternatively, {{site.data.keyword.logs_full_notm}} complementary VPC Service can be used to manage operate system logs, application logs, and platform logs in the {{site.data.keyword.Bluemix_notm}}. {{site.data.keyword.logs_full_notm}} offers administrators, DevOps teams, and developers advanced features to filter, search, and tail log data, define alerts, and design custom views to monitor application and system logs.

For more information, see [Getting started with IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-getting-started).

IBM Log Analysis is deprecated and will no longer be supported as of 30 March 2025. The replacement service, IBM Cloud Logs is planned to be generally available late second quarter 2024. For more information, see [Getting started with IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-getting-started).
{: attention}

## Activity Tracking
{: #activity-tracking}

Consider the need to record and monitor the activities and changes made inside the {{site.data.keyword.Bluemix_notm}} account to help in investigating abnormal activity, critical actions, and to meet regulatory audit requirements. Third-party software such as Splunk and Datadog can be integrated with {{site.data.keyword.Bluemix_notm}} classic to provide security monitoring, compliance reporting, and operational intelligence. Alternatively, {{site.data.keyword.logs_full_notm}} complementary VPC Service monitors and manages activities in {{site.data.keyword.Bluemix_notm}}d. It provides a dashboard and notification for real-time monitoring. For more information, see [Getting started with IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-getting-started).

Explore details on [activity tracker events for virtual servers in Classic](/docs/virtual-servers?topic=virtual-servers-at_events) and for bare metal in Classic [here](/docs/bare-metal?topic=bare-metal-bm-at-events).

IBM Cloud Activity Tracker services are deprecated and will no longer be supported as of 30 March 2025. The replacement service, IBM Cloud Logs is planned to be generally available late second quarter 2024. For more information, see [Getting started with IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-getting-started).
{: attention}
