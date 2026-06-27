---
title: "Exchange Log"
date: 2026-04-06T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet fully completed. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
This view shows statistics of data exchange between GTNet instances. Logging must be enabled via the [global setting](../globalsettings/) "gt.gtnet.use.log". When logging is globally disabled, the "Exchange Log" navigation entry is greyed out and not clickable.
{{% notice style="info" title="Re-login required" %}}
Changes to the global setting "gt.gtnet.use.log" are only reflected in the navigation tree after logging in again.
{{% /notice %}}
{{% notice style="info" title="Peer visibility" %}}
The main table only shows peers where logging is enabled for at least one role (supplier or consumer) set to "Overview". Peers with logging turned off do not appear.
{{% /notice %}}

### Tabs for Exchange Types
The view is divided into three tabs corresponding to the different types of data exchange:
- **Last Price (LP)**: Statistics for the exchange of current day prices.
- **Historical Prices (HP)**: Statistics for the exchange of historical price data.
- **Security Metadata**: Statistics for security metadata lookups from connected peers. Each security lookup via GTNet creates a log entry.

### Main Table
The main table displays all GTNet instances where logging is enabled. Each row can be expanded to show detailed statistics.

### Supplier and Consumer Statistics
For each instance, two separate statistics are displayed:
- **Supplier Statistics**: Shows data that was sent from this instance to others.
- **Consumer Statistics**: Shows data that was received from other instances.

### Statistics Table Columns
The statistics are presented in a hierarchical tree structure, grouped by time period. Aggregation occurs automatically from individual requests to daily, weekly, monthly, and yearly summaries.

| Column | Description |
|--------|-------------|
| Label | The time period of the statistics (e.g., date, calendar week, month, or year). |
| Sent | Number of entities (securities or currency pairs) sent in requests. |
| Updated | Number of entities for which new or updated price data was provided. |
| In response | Number of entities included in the responses. |
| Requests | Number of requests that occurred in this period. |

### Aggregation Periods
Log entries are automatically summarized according to the following periods:
- **Individual**: Each individual request is logged separately.
- **Daily**: Summary of all requests for one day.
- **Weekly**: Summary of all requests for one calendar week.
- **Monthly**: Summary of all requests for one month.
- **Yearly**: Summary of all requests for one year.

The aggregation of older entries and the deletion of old exchange messages are performed by [Job 22](../../taskdatachangemonitor/taskdescription/#22---aggregate-gtnet-exchange-logs-and-delete-old-messages) to keep the data volume manageable. The retention periods and aggregation thresholds can be configured via [Global Settings GTNet](../globalsettings/).
