---
title: "Monitoring GTNet"
date: 2026-08-31T22:54:47+01:00
draft: false
weight: 45
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
GTNet provides several ways to monitor the status and activity of data exchanges. One view shows what has been set up, the other what is actually happening; the background tasks reveal whether the automatic parts are running.

## Static Overview: Exchange Price Data
The [Exchange Price Data](../exchange/) view provides a static overview of which instruments are configured for exchange and which counterparts can supply data:
- View all securities and currency pairs with their exchange settings
- Expand rows to see supplier details (which counterparts can provide data for each instrument)
- Useful for understanding the current configuration state

## Dynamic View: Exchange Log
The [Exchange Log](../exchangelog/) view offers a dynamic, time-based view of actual exchange activity:
- See statistics for both intraday and historical price exchanges
- Track supplier and consumer statistics per connected instance
- View aggregated data by day, week, month, and year
- Requires logging to be enabled via **g.gnet.use.log** in [Global Settings](../globalsettings/)

{{% notice style="note" title="What the statistics do not tell you" %}}
The statistics record how many instruments were sent and how many were updated as a result. They therefore say something about the reliability of a delivery, but nothing about the correctness of the delivered prices. See the section on the limits of GTNet in the [overview](../).
{{% /notice %}}

## Message Delivery Attempts
Outgoing messages sent to one or more counterparts in the background can be tracked per recipient in the [Setup GTNet](../setup/) view. Expand the row under which the sent message is stored. The **"Delivery attempts"** area is visible to administrators only and shows its number of entries in the title.

The table identifies the message timestamp and code, target domain, attempt status, and the number of transmissions actually attempted. "Last attempt" and "Last error" help diagnose a failed transmission; "Delivered at" records successful completion. A skipped attempt does not increase "Try count", because no connection to the counterpart was made.

| Attempt status | Meaning |
|----------------|---------|
| Queued | The message has been queued but has not yet been evaluated for this counterpart. |
| Waiting for handshake | The credentials needed for sending are missing. Delivery can continue after a new handshake. |
| Retryable failure | A transmission was attempted and failed. The background task tries again later. |
| Delivered | The counterpart accepted the message. |
| Peer out of service | The counterpart will permanently no longer be contacted. |
| Expired | The announcement is no longer effective and will therefore no longer be delivered. |

{{< mermaid >}}
stateDiagram-v2
    [*] --> Queued
    Queued --> WaitingForHandshake: Credentials missing
    Queued --> RetryableFailure: Transmission failed
    Queued --> Delivered: Message accepted
    WaitingForHandshake --> RetryableFailure: Transmission after handshake failed
    WaitingForHandshake --> Delivered: Message accepted after handshake
    RetryableFailure --> RetryableFailure: Another attempt failed
    RetryableFailure --> Delivered: Another attempt succeeded
    Queued --> OutOfService: Counterpart discontinued
    WaitingForHandshake --> OutOfService: Counterpart discontinued
    RetryableFailure --> OutOfService: Counterpart discontinued
    Queued --> Expired: Announcement no longer effective
    WaitingForHandshake --> Expired: Announcement no longer effective
    RetryableFailure --> Expired: Announcement no longer effective

    state "Waiting for handshake" as WaitingForHandshake
    state "Retryable failure" as RetryableFailure
    state "Peer out of service" as OutOfService
{{< /mermaid >}}

For the indication under **"GT Message"**, all recipients of a message are summarized. As soon as at least one counterpart has accepted the message, it counts as delivered overall, even if other targets could not be reached. A red failure highlight only appears when all targets have ended without success; while delivery is still possible, the message remains pending. The "Delivery attempts" area is therefore the authoritative view when assessing a broadcast in detail.

For maintenance and discontinuation announcements, an open attempt ends no later than the point at which the announcement ceases to be effective. The full process is described under [Maintenance and Discontinuation](../setup/availability/#delivery-of-the-announcements).

## Background Tasks
Several background tasks handle GTNet operations automatically. These can be monitored in the [Task Data Change Monitor](../../taskdatachangemonitor/):

| Task ID | Name | Description |
|---------|------|-------------|
| 20 | Track online status | Checks the online/busy status of all configured counterparts once shortly after server start; Online requires a successful GTNet protocol response, and counterparts without a completed outbound handshake are set to "Unknown". Counterparts that are out of service or inside an announced maintenance window are skipped. A single instance can be re-checked at any time via the "Check status now" context menu entry in the [GTNet server overview](../setup/). |
| 22 | Aggregate exchange logs | Summarizes exchange log entries into daily, weekly, monthly, and yearly periods |
| 23 | Synchronize configurations | Syncs exchange configurations with connected counterparts and updates supplier details |
| 24 | Broadcast settings changes | Sends settings updates to all connected counterparts when local settings change |
| 25 | Deliver future messages | Handles scheduled messages like maintenance announcements and sets instances whose announced discontinuation date has been reached to "Out of service" |

For detailed descriptions of each task, see [Background Tasks](../../taskdatachangemonitor/taskdescription/).
