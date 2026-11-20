---
title: "GTNet"
date: 2026-10-16T12:00:00+02:00
draft: false
weight: 10
archetype: "default"
---
The following background tasks maintain the connection to the other GTNet instances and look after the data arising from it. Task 20 checks after the start of the application which instances are reachable. Tasks 23 and 24 align the exchange configurations and the settings with the partners, tasks 25 and 26 deliver messages in the background, and task 22 condenses the exchange logs and deletes old messages. Task 51 uses GTNet to find securities for import positions. How GTNet works as a whole is described under [GTNet]({{% relref "/admindata/gtnet" %}}).

## 20 - Track online status updates of GTNet instances {#JOB20}
This task type checks and updates the online and busy status of all configured GTNet servers. For each peer whose handshake is complete, a ping message is sent and reachability is derived from the reply. The online and busy flags as well as the per-exchange-kind server state are updated accordingly. Peers that are out of service and peers currently inside an announced maintenance window are skipped: the former must not be put back into service by an accidentally successful ping, and for the latter a planned shutdown would otherwise be recorded as an outage. The task runs once, roughly 30 seconds after the application starts, so that the server is fully initialised and ready to accept HTTP requests before the check.

To make the displayed online status reflect the actual reachability of the GT application, the following rules apply:
- **Online**: The ping was delivered and the peer responded with a valid GTNet protocol reply. An open TCP connection alone is not sufficient — HTTP error responses from an upstream reverse proxy (for example 502 or 503) result in Offline.
- **Offline**: The peer is unreachable, or only replies with an HTTP error.
- **Unknown**: The outbound handshake with the peer is not complete (the token required for outbound requests is missing). Without that token no authenticated ping can be sent, so any stale "Online" flag left over from an inbound handshake phase is reset to "Unknown".

In addition, administrators can re-check the status of a single instance at any time via the **"Check status now"** context menu entry in the [GTNet server overview](../../../gtnet/setup/), without having to wait for a server restart.

## 22 - Aggregate GTNet exchange logs and delete old messages {#JOB22}
This task type performs two maintenance operations: log aggregation and deletion of old exchange messages.

**Log aggregation**: GTNet exchange log entries are aggregated from shorter to longer time periods in a rolling manner. The aggregation occurs in multiple levels: individual entries are aggregated into daily summaries, daily into weekly, weekly into monthly, and monthly into yearly. The global parameter **g.gnet.log.aggregate.days** controls the thresholds for each level in the format "D=1,W=7,M=30,Y=365".

**Message deletion**: Old exchange messages are automatically deleted to reduce storage requirements. The global parameter **g.gnet.del.message.recv** controls the retention period in the format "LP=1,HP=5,SL=5,SS=10":
- **LP** (LastPrice): Number of days before last price exchange messages are deleted
- **HP** (HistoryPrice): Number of days before historical price exchange messages are deleted
- **SL** (SecurityLookup): Number of days before security lookup messages are deleted
- **SS** (ServerStatus): Number of days before the server status announcements "Server is now offline" and "Settings updated" are deleted

The default schedule runs daily at 3:00 AM UTC. The aggregated data is displayed in the [Exchange Log](../../../../admindata/gtnet/exchangelog/) view. For more information about the configuration parameters, see [Global Settings GTNet](../../../../admindata/gtnet/globalsettings/).

## 23 - Synchronize GTNet exchange configurations {#JOB23}
This task type synchronizes GTNetExchange configurations with connected GTNet peers and updates the GTNetSupplierDetail entries. It operates in two modes: full recreation mode recreates all supplier detail entries for each peer, while incremental mode only synchronizes changes since the last sync. The default schedule runs daily at 2:00 AM UTC in full recreation mode. This task can also be triggered manually from the [GTNet setup](../../../../admindata/gtnet/setup/) interface or automatically after a data exchange request is accepted.

## 24 - Broadcast GTNet settings changes {#JOB24}
This task type broadcasts settings changes to all configured GTNet peers. When local GTNet entity settings change (such as maxLimit, acceptRequest, serverState, or dailyRequestLimit), this task notifies all connected peers by sending a settings update message. This task is not scheduled but is triggered automatically whenever GTNet settings are saved. Running as a background task prevents the user interface from blocking during network operations when settings are changed.

## 25 - Deliver pending GTNet future messages {#JOB25}
This task type handles the delivery of broadcast messages that have a scheduled execution date in the future. This includes maintenance window announcements, server discontinuation notices, and their corresponding cancellation messages. The task performs five operations: creating delivery attempts for new partners whose handshake completed after a pending message was created, processing cancellation messages, delivering pending messages to their targets, cleaning up expired messages, and finally setting those instances to "Out of service" whose announced discontinuation date has been reached. The default schedule runs every 5 hours, but the task is also triggered immediately when any of these message types is sent; the switch to "Out of service" therefore happens within a few hours of the announced date. For more information about GTNet message types, see [GTNet setup](../../../../admindata/gtnet/setup/), and about the two announcements see [Maintenance and Discontinuation](../../../../admindata/gtnet/setup/availability/).

## 26 - Deliver GTNet admin messages to targets {#JOB26}
This task type delivers pending admin messages to multiple GTNet targets. The task is automatically triggered when an administrator sends a message to multiple selected domains via the [Admin Messages](../../../../admindata/gtnet/setup/msgadmin/) view. Asynchronous processing in the background ensures the user interface responds immediately while delivery occurs separately.

The task performs the following steps:
- Queries all pending delivery attempts for admin messages
- Delivers messages to the respective targets via the M2M interface
- Updates the delivery status after successful transmission
- Logs failed delivery attempts

{{% callout info %}}
This task cannot be created manually. It is created exclusively by the system when admin messages are sent to multiple recipients.
{{% /callout %}}

## 51 - Import securities from GTNet for import positions {#JOB51}
This task is created when the query is started in the [GTNet security import]({{% relref "/basedata/gtnetsecurityimport" %}}) through the context menu **Import securities from GTNet**; at most one task waits for the same import list. For every position without a linked security it queries the reachable GTNet instances, chooses the best match and with it links an existing security or creates a new one. If the query is started from the transaction import, the matching import positions of the transactions receive the security as well, recognised by the ISIN or the ticker symbol. For every newly created security the task immediately loads the historical price data, from the instance with the longest price history; if no instance delivers prices, the connector of the security is used. Newly created securities count towards the daily limit of the user, linking existing securities does not.
