---
title: "Exchange Log"
date: 2026-01-09T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet fully completed. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
This view shows statistics of data exchange between GTNet instances. Logging must be enabled via the [global setting](../globalsettings/) "gt.gtnet.use.log".

### Tabs for Exchange Types
The view is divided into two tabs corresponding to the different types of data exchange:
- **Last Price**: Statistics for the exchange of current day prices.
- **Historical Prices**: Statistics for the exchange of historical price data.

### Main Table
The main table displays all GTNet instances with which data exchange has occurred. Each row can be expanded to show detailed statistics.

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

The aggregation of older entries is performed by a background process to keep the data volume manageable.
