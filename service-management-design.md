---

copyright:
  years: 2023
lastupdated: "2023-12-26"

subcollection: pattern-Network-architecture-for-data-centers-without-a-Transit-Gateway-service

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# DevOps design
{: #devops-design}

# Service management design

## Monitoring

Evaluate the level of monitoring required by considering the workload criticality and the metrics that need to be observed.

Basic instance monitoring deployed in IBM Cloud Classic environments can be viewed from the cloud portal Health Dashboard (Health Dashboard, Device \> Usage).

More advanced monitoring capabilities such as troubleshooting, events and alerts, and custom dashboards can be leveraged by integrating the cloud resources with IBM Cloud Monitoring (complementary VPC service). The agent or the Windows Prometheus Bundle are installed on the devices.

Alternatively, third party monitoring software can be deployed within IBM Cloud Classic for more detailed monitoring.

Learn more about IBM Cloud Monitoring [here](https://cloud.ibm.com/docs/monitoring?topic=monitoring-getting-started#getting-started)

## Log Analysis

Examine what logs are required for troubleshooting and auditing, as well as the retention policy necessary to meet the audit and compliance requirements.

Third party software can be implemented within IBM Cloud Classic to enable log analysis within IBM Cloud Classic.

Alternatively, IBM Log Analysis (complementary VPC Service) can be used to manage operating system logs, application logs, and platform logs in the IBM Cloud. IBM Log Analysis offers administrators, DevOps teams, and developers advanced features to filter, search, and tail log data, define alerts, and design custom views to monitor application and system logs.

Learn more about IBM Log Analysis [here](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-getting-started#getting-started)

## Activity Tracking

Consider the need to record and monitor the activities and changes made inside the IBM Cloud account to assist in investigating abnormal activity, critical actions, and to meet regulatory audit requirements.

Third party software such as Splunk and Datadog can be integrated with IBM Cloud Classic to provide security monitoring, compliance reporting, and operational intelligence.

Alternatively, IBM Cloud Activity Tracker (complementary VPC Service) monitors and manages activities in the IBM Cloud. It provides a dashboard and notification for real-time monitoring.

Learn more about IBM Activity Tracker [here](https://cloud.ibm.com/docs/activity-tracker?topic=activity-tracker-getting-started)

Explore details on activity tracker events for virtual servers in Classic [here](https://cloud.ibm.com/docs/virtual-servers?topic=virtual-servers-at_events) and for bare metal in Classic [here](https://cloud.ibm.com/docs/bare-metal?topic=bare-metal-bm-at-events)
