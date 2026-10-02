---
title: "Maintenance and Discontinuation"
date: 2026-08-31T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
A GTNet instance that disappears without warning causes requests to run into the void at every peer and is sooner or later recorded there as "Offline". Anyone planning maintenance or shutting down operation entirely therefore announces it in advance. The peers act on the announcement automatically and do not even query the instance during the announced period.

### The Two Announcements Compared
There are two announcements for planned unavailability, differing in their duration and in how final they are.

| | Maintenance window | Discontinuation |
|---|---|---|
| Message | "Server is in maintenance mode during time period" | "Server operation will be discontinued as of this date" |
| Fields to enter | "Start" and "End" | "Discontinued as of" |
| Several open at once | Yes, as long as they do not overlap | No, only one |
| Effect on the recipient | The instance is not contacted during the period | "Server Online" is set to "Out of service" |
| Ends by itself | Yes, with "End" | No, the state is final |
| Can be revoked | Before "Start" | Before "Discontinued as of" |

### Announcing a Maintenance Window
A maintenance window is announced in the [Setup GTNet](../) view. Select your own server entry — or no row at all — and choose **"Send GT Net Message"** from the context menu. The message type "Server is in maintenance mode during time period" is available there. The two fields **"Start"** and **"End"** have to be filled in; both must lie in the future and "End" must be after "Start".

Several maintenance windows may be announced at the same time, as long as they do not overlap. A window that reaches into an already announced one is rejected with an error message. This allows a series of recurring maintenance evenings to be published in advance, for example.

The announcement changes nothing on your own instance. It is purely information for the peers; your own server keeps running unchanged until the maintenance actually begins and has to be shut down at the announced time by yourself.

### What Happens During a Maintenance Window
The recipient stores the announced window and evaluates it as soon as it begins. Between "Start" and "End" the instance in question is asked neither for intraday prices nor for historical prices or security metadata, and no messages are sent to it either. The online status check also leaves the instance untouched during this period, so that a planned shutdown is not incorrectly recorded as "Offline".

After "End" the instance is used entirely normally again. No intervention is required and no all-clear has to be sent; the window expires by itself.

Which windows a peer has announced is shown in the server overview: the expanded row contains the **"Maintenance windows"** area with the "Start" and "End" of every reported window. Windows that have already expired remain visible there until the corresponding message is deleted.

### Announcing the Discontinuation
If an instance is being shut down permanently, this is announced with the message "Server operation will be discontinued as of this date". The field **"Discontinued as of"** has to be filled in and must lie in the future. Only one discontinuation can be open at a time; while one is announced, the message type is no longer offered for selection.

Until the announced date everything stays unchanged: the peers continue to use the instance as a data supplier and send messages to it. The advance notice therefore costs no data exchange, it gives the peers time to look for other sources.

### What Happens From the Announced Date
From the date "Discontinued as of" the recipient sets the **"Server Online"** column of the instance in question to **"Out of service"**. At the same time all information objects of that instance receive the "Status Server" **"Closed"** and "Accept request" **"Closed"**, which drops the instance out of supplier selection.

This state is final. Neither an online status check nor an incoming message from the instance in question lifts it again — not even if the server unexpectedly keeps answering. This prevents a discontinued instance from being silently put back into service by an accidentally successful ping.

The switch is performed by a background task that runs every five hours. It is therefore not made to the minute, but within a few hours of the announced date.

{{< mermaid >}}
stateDiagram-v2
    [*] --> Unknown
    Unknown --> Online: Ping successful
    Online --> Offline: Ping failed
    Offline --> Online: Ping successful
    Online --> OutOfService: Announced date reached
    Offline --> OutOfService: Announced date reached
    OutOfService --> Unknown: Operation discontinuation cancelled

    state OutOfService: Out of service

    note right of OutOfService : Lifted neither by a status check nor by a message
{{< /mermaid >}}

After the discontinuation the entry can be deleted from the context menu of the server overview. Its messages, exchange settings and exchange logs disappear with it. If you would rather keep the instance, for instance because it may go back into operation after all, set "Server Online" to a different value by hand in the edit dialog.

### Revoking an Announcement
Both announcements can be withdrawn. Select the sent announcement in the message list of your own entry and choose **"Reverse"** from the context menu. This sends "Maintenance cancelled" or "Operation discontinuation cancelled" to all peers; they then discard the maintenance window or the pending discontinuation.

The menu entry only appears while the announced moment still lies in the future: for a maintenance window before its "Start", for a discontinuation before the date "Discontinued as of". A maintenance window that is already running is not ended prematurely, it expires with "End".

### Delivery of the Announcements
Announcements go to all peers with which a data exchange has been set up and the handshake completed. They are delivered immediately after being sent; if a peer is unreachable at that moment, a background task repeats the delivery later. A peer whose handshake is only established after the announcement still receives it, provided it is still valid at that point.

A separate delivery attempt is kept for every recipient. A temporary connection failure therefore affects only that counterpart and does not prevent others from receiving the announcement. If the credentials are lost after the attempt has been queued, it waits for a new handshake. If the counterpart is permanently marked "Out of service", or if the announcement ceases to be valid, delivery stops. These outcomes remain visible with the message instead of looking like an attempt that has not yet been processed.

Administrators can inspect these outcomes under **"Delivery attempts"** in the expanded entry for their own server. The individual statuses and their meaning are described under [Monitoring GTNet](../../monitoring/#message-delivery-attempts).
{{% notice info %}}
Announce a maintenance window early. An instance that is itself unreachable at the time of the announcement only learns of it with one of the next delivery attempts — and does not act on the window until then.
{{% /notice %}}
