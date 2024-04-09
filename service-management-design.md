---

copyright:
  years: 2024
lastupdated: "2024-04-09"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-service

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Service management design
{: #service-mgmt}

## Monitoring
{: #monitoring}

Evaluate the level of monitoring required by considering the workload criticality and the metrics that need to be observed.

Basic instance monitoring that is deployed in {{site.data.keyword.Bluemix}} classic environments can be viewed from the cloud portal Health Dashboard (Health Dashboard, Device \> Usage).

More advanced monitoring capabilities such as troubleshooting, events and alerts, and custom dashboards can be used by integrating the cloud resources with {{site.data.keyword.monitoringlong_notm}}, complementary VPC service. The agent or the Windows Prometheus Bundle are installed on the devices.

Alternatively, third party monitoring software can be deployed within {{site.data.keyword.Bluemix_notm}} classic for more detailed monitoring.

For more information, see [Getting started with IBM Cloud Monitoring](/docs/monitoring?topic=monitoring-getting-started#getting-started)

## Log Analysis
{: #log-analysis}

Examine what logs are required for troubleshooting and auditing, as well as the retention policy necessary to meet the audit and compliance requirements.

Third party software can be implemented within {{site.data.keyword.Bluemix_notm}} classic to enable log analysis within {{site.data.keyword.Bluemix_notm}} classic.

Alternatively, {{site.data.keyword.loganalysislong_notm}}, complementary VPC Service, can be used to manage to operate system logs, application logs, and platform logs in the {{site.data.keyword.Bluemix_notm}}. {{site.data.keyword.loganalysislong_notm}} offers administrators, DevOps teams, and developers advanced features to filter, search, and tail log data, define alerts, and design custom views to monitor application and system logs.

For more information, see [Getting started with IBM Log Analysis](/docs/log-analysis?topic=log-analysis-getting-started#getting-started).

## Activity Tracking
{: #activity-tracking}

Consider the need to record and monitor the activities and changes made inside the {{site.data.keyword.Bluemix_notm}} account to help investigating abnormal activity, critical actions, and to meet regulatory audit requirements.

Third party software such as Splunk and Datadog can be integrated with {{site.data.keyword.Bluemix_notm}} classic to provide security monitoring, compliance reporting, and operational intelligence.

Alternatively, {{site.data.keyword.cloudaccesstraillong_notm}}, a complementary VPC Service, monitors and manages activities in {{site.data.keyword.Bluemix_notm}}. It provides a dashboard and notification for real-time monitoring.

For more information, see [Getting started with Activity Tracker](/docs/activity-tracker?topic=activity-tracker-getting-started).