---
title: "Messages at a Glance"
date: 2026-10-27T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
{{% callout style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.40.0" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /callout %}}
This page lists all GTNet messages and describes what they cause on your own instance and on the counterpart. You send messages yourself in the server overview of [Setup GTNet](../) through the context menu item **"Send GT Net Message"**. An incoming request is answered in the "GT Message" area of the expanded row with **"Reply"**, and a sent announcement of unavailability is withdrawn there with **"Reverse"**. Which message the dialog offers depends on the state of the connection and is described in [Available Messages by State](../#available-messages-by-state).

### Messages to a single counterpart
| Message | Type | Effect |
|---|---|---|
| First contact | Request | Both instances exchange their credentials. Only afterwards is further communication possible. |
| Token refresh | Request | Replaces the credentials of both directions with new ones. The replaced token remains valid for a transition period. If the counterpart rejects, the previous token applies again. |
| Query this server list | Request | Asks the counterpart for the instances it knows. If allowed, these instances are added to your own list. |
| My server list may no longer be queried | Announcement | Withdraws the permission previously granted to the counterpart to query your server list. A later query needs a new answer. |
| Request for data exchange | Request | Asks for the exchange of the data types selected in the field **"Data types to exchange"**. Only data types that the counterpart accepts and that are not yet exchanged are offered. |
| Revoke data exchange | Announcement | Ends the exchange of the data types selected in the field **"Data types to exchange"**. Only data types with an active exchange are offered. |
| Admin message | Announcement | Free text to the administrators of the counterpart. See [Admin Messages](../msgadmin/). |

Revoking a data exchange takes effect immediately and not at a later time. Your own instance switches off the exchange of the selected data types, and the counterpart closes these data types on its entry of your instance. Resuming the exchange requires a new "Request for data exchange". A revoke without a selected data type is not sent, because it would end nothing on either side.

### Messages to all counterparts
These announcements go to all connected counterparts at once.
| Message | Effect |
|---|---|
| Server is now offline | The counterparts immediately list your instance as offline. |
| Server is in maintenance mode during time period | Announces a maintenance window during which your instance is not contacted. |
| Server operation will be discontinued as of this date | Announces the end of your instance. From that date on, the counterparts list it as "Out of Service". |
| Maintenance cancelled | Lifts an announced maintenance window. Triggered with **"Reverse"** on the sent announcement. |
| Operation discontinuation cancelled | Lifts an announced discontinuation. Also triggered with **"Reverse"**. |

The details on maintenance and discontinuation are described in [Maintenance and Discontinuation](../availability/).

### Answers to requests
A request is answered either by an administrator with **"Reply"** or by a rule of the [Automatic answer](../../autoanswer/). Until the answer arrives, the requesting side shows the status "Answer expected".
| Request | Acceptance | Rejection |
|---|---|---|
| First contact | Contact successful | Contact rejected |
| Token refresh | Token refresh accepted | Token refresh rejected |
| Query this server list | Allow query of my server list | Do not allow query of my server list |
| Request for data exchange | Data request is accepted | Data request is rejected |

The answer **"Contact rejected - server not in list"** cannot be chosen manually. It is sent automatically when an unknown instance gets in touch and **"Allow server creation"** is not active on your own entry. A rejection can also carry a waiting period during which a new request of the same kind is rejected without further review.

### Feedback from the system
Besides the actual answers, there is feedback an instance sends on its own. It decides nothing about the request and therefore does not appear as an answer in the message list.

**Received and processed** confirms that an announcement or admin message has arrived.

**Accepted, an administrator decides** means that the request has arrived but no rule of the automatic answer matched. The request stays open with "Answer expected" on the requesting side until the administrator of the counterpart has decided. The decision then arrives as a message of its own.

**Not processed** means that the counterpart refused the message, for example because it was invalid or the connection no longer exists. Your own message then counts as not delivered.

**Daily request limit for this server reached** means that the counterpart's quota for the current UTC day is used up. The request was not processed and, as with "Not processed", counts as not delivered. It therefore does not remain as an open request and can be deleted. See [Query Limits](../../requestlimits/).

**Settings updated** is sent by your own instance to all counterparts on its own as soon as the **"Daily query limit"**, the restricted use or the exchange settings of a data type change on your own entry.

### Status check with "Ping"
The status check is not sent through the dialog but through the context menu item **"Check status now"** or automatically by the background task "Track online status". The counterpart's answer determines not only its online status. It also carries what the counterpart publishes about itself and takes this information over into its entry: the time zone (**"Local time +/- equals UTC"**), **"Pass on in server lists"**, **"Daily query limit"** and, per data type, whether it accepts requests and the query limit. A counterpart with which only status checks are exchanged therefore does not keep outdated information.

### Messages of the data exchange
As soon as a data exchange is agreed, the instances communicate with further messages that are never offered in the dialog. Background tasks and the security lookup send them on their own, and the answer comes back immediately. They do not appear in the message list. Their frequency and outcome are shown in the [Exchange Log](../../exchangelog/).
| Area | Messages |
|---|---|
| Intraday prices | "Request intraday prices" with its answer, "Intraday price push" with its acknowledgement and "Request exceeded maximum instrument limit" |
| Historical prices | "Request historical prices" with its answer, "Push historical prices" with its acknowledgement, "Request historical coverage metadata" with its answer and "Request exceeded maximum historical data limit" |
| Synchronisation | "Exchange Sync Request" and "Exchange Sync Response", with which two instances align the exchange configuration of their instruments |
| Security metadata | "Security lookup request" with the answers "Security lookup response", "Security not found" and "Security lookup rejected" as well as "Request metadata for multiple securities" with its answer |

The procedures are described in [Last Price Exchange](../../exchange/lastprice/) and [Historical Price Exchange](../../exchange/historicalprice/).
