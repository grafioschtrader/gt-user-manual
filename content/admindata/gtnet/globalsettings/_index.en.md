---
title: "Global Settings GTNet"
date: 2026-01-18T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}

The GTNet functionality is controlled via specific global parameters. These parameters can be found in the [Global Settings](../../globalsettings/) view and are identified by the prefix "gt.gtnet".

### Administrator-Managed Parameters
These parameters can be configured by the administrator to customize GTNet behavior.

#### gt.gtnet.use
Enables or disables the entire GTNet functionality. A value of 0 means disabled, a non-zero value enables GTNet. By default, GTNet is disabled. Without this activation, no data exchange with other instances takes place.

#### gt.gtnet.use.log
Controls the logging of data exchanges. When enabled (non-zero value), all exchange operations between instances are recorded. These logs can be viewed in the [Exchange Log](../exchangelog/) view. Logging is disabled by default and can be enabled for diagnostic purposes or to monitor network activity.

#### gt.gtnet.lastprice.delay.seconds
Defines an additional delay in seconds for calculating the freshness threshold for last price requests. This value is used together with the watchlist timeout to determine whether a received price is still current enough. Prices older than (current time - watchlist timeout - this delay value) are rejected as stale. The default value is 300 seconds (5 minutes).

#### gt.gtnet.del.message.recv
Controls the retention period for price exchange messages in PropertyString format. The format is "LP=days,HP=days", where:
- **LP** (LastPrice): Number of days before last price exchange messages (codes 60, 61) are deleted
- **HP** (HistoryPrice): Number of days before historical price exchange messages (codes 80, 81) are deleted

The valid range for both values is 1-10 days. The default value is "LP=1,HP=5", meaning last price messages are deleted after 1 day and historical price messages after 5 days. This cleanup is performed by [background task 22](../../../admindata/taskdatachangemonitor/taskdescription/#22---aggregate-gtnet-exchange-logs-and-delete-old-messages).

#### gt.gtnet.log.aggregate.days
Controls the thresholds for exchange log aggregation in PropertyString format. The format is "D=days,W=days,M=days,Y=days", where:
- **D**: Number of days before individual entries are aggregated into daily summaries
- **W**: Number of days before daily entries are aggregated into weekly
- **M**: Number of days before weekly entries are aggregated into monthly
- **Y**: Number of days before monthly entries are aggregated into yearly

The default value is "D=1,W=7,M=30,Y=365". This aggregation is performed by [background task 22](../../../admindata/taskdatachangemonitor/taskdescription/#22---aggregate-gtnet-exchange-logs-and-delete-old-messages).

### System-Managed Parameters
These parameters are automatically set by the system and should not be modified manually.

#### gt.gtnet.my.entry.id
Contains the unique ID of the own instance in GTNet. This value is automatically set when the own server is registered in the [GTNet and Messages](../setup/) view. The ID serves as a reference for identifying the own instance in the network.

#### gt.gtnet.exchange.sync.timestamp
Stores the timestamp of the last successful synchronization of exchangeable entities. This value is automatically updated after a synchronization process is completed. It is used to determine which entries have changed since the last synchronization.

---

## Status Synchronization Between Servers
GTNet automatically keeps server status information synchronized between connected instances. This section explains when status changes are published to remote servers and when updates from remote servers reach your instance.

### Outgoing Status Changes (Your Server → Remote Servers)
Status changes from your server are broadcast to all remote servers with which you have an established communication (completed handshake with exchanged tokens).

#### Automatic Broadcasts

| Event | Message Type | Trigger |
|-------|--------------|---------|
| Server Startup | Server is now online | When the application starts and GTNet is enabled (`gt.gtnet.use` > 0) |
| Server Shutdown | Server is now offline | When the application shuts down gracefully |
| Busy Status Change | Server is busy / Server is no longer busy | When the "Usage Restricted" checkbox is changed in [GTNet and Messages](../setup/) |
| Settings Update | Settings updated | When settings like Daily Request Limit, Accept Request, Server State, or Max Limit are changed in [GTNet and Messages](../setup/) |

#### Manual Broadcasts
The following status messages can be sent manually via the context menu in the [GTNet and Messages](../setup/) view:

| Message Type | Purpose |
|--------------|---------|
| Maintenance Planned | Announce a scheduled maintenance window with start and end time |
| Maintenance Cancelled | Cancel a previously announced maintenance window |
| Operation Discontinued | Announce permanent service discontinuation with effective date |
| Operation Discontinuation Cancelled | Cancel a previously announced discontinuation |

### Incoming Status Changes (Remote Servers → Your Server)
Your server receives status updates from remote servers in two ways:

#### 1. Explicit Status Messages
When a remote server broadcasts a status change (online, offline, busy, maintenance, etc.), your server receives and stores this message. These messages are visible in the message history of the [GTNet and Messages](../setup/) view.

#### 2. Automatic Settings Synchronization
Every message received from a remote server includes the sender's current GTNet configuration in the message envelope. Your server automatically extracts and updates the remote server's settings from this envelope, ensuring that:
- Daily request limits are always current
- Accept request states (Closed, Open, Push-Open) are synchronized
- Server states (None, Open, Push-Open) are up to date
- Maximum instrument limits are current

This means that even without explicit status messages, your local copy of a remote server's settings stays synchronized through regular communication.

### Server Lifecycle Events

#### Application Startup
When your GT server starts:
1. If `gt.gtnet.use` is enabled (value > 0), GTNet functionality is activated
2. An "Server is now online" message is automatically sent to all connected peers
3. Remote servers update their records to show your server as online

#### Application Shutdown
When your GT server shuts down gracefully:
1. An "Server is now offline" message is automatically sent to all connected peers
2. Remote servers update their records to show your server as offline

{{% notice note %}}
If the server crashes or is forcibly terminated, the offline message cannot be sent. In this case, remote servers will only detect the unavailability when they attempt to communicate and receive no response.
{{% /notice %}}

### Practical Implications

#### For Administrators
- Changes made in the GTNet and Messages edit dialog are immediately broadcast to all connected peers
- No manual action is required to notify peers of configuration changes
- The message history shows both sent and received status messages for auditing purposes

#### For Network Participants
- Your server automatically respects the current limits and states of remote servers
- When a remote server becomes busy, your server reduces request frequency
- When a remote server goes offline, no requests are attempted until it comes back online
