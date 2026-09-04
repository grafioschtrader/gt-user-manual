---
title: "Setup GTNet"
date: 2026-08-31T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
This view manages all known GTNet instances and controls communication with them.

### Setting Up Your Own Server
Before communication with other participants is possible, your own GT server must be registered. It is important that the server is accessible from outside. After registering your own server, its ID is stored as a reference. Subsequently, additional reachable instances can be registered for communication. If the red hint that your own server has to be registered remains after saving, the application could not recognize its own address; how to remedy that is described under [Global Settings GTNet](../globalsettings/) for the parameter "g.gnet.my.entry.id".

### Server Overview
The table shows all known GTNet instances with the following information:
- **Domain**: The base URL of the instance for M2M communication.
- **Time Zone**: The server's time zone, helps estimate operating hours.
- **Server Online**: Shows the current online status of the instance (Online, Offline, Unknown, or Out of service).
- **Restricted Use**: Indicates whether the instance is currently at full capacity and cannot process data requests.
- **Exchange Last Price**: Status of intraday price exchange with this instance.
- **Exchange Entity**: Status of historical data exchange with this instance.
- **Reconnect requested**: Appears as a highlighted icon when this instance has tried in vain to rebuild the connection. See [Restoring a Connection](troubleshooting/).

### Meaning of "Online", "Offline", "Unknown" and "Out of Service" Status
The **Server Online** column has four distinct states that differ fundamentally in what they say about reachability.

**Online** means that a ping message was sent successfully and a valid GTNet response was received. Only a full protocol response — not merely an open TCP connection — counts as Online. This prevents an upstream reverse proxy or DuckDNS placeholder from making an instance appear reachable while the GT application itself is down. If the peer replies to the ping with an HTTP error (for example 502 or 503), the instance is therefore classified as Offline.

**Offline** means the server does not respond or does not respond correctly. No data communication takes place. This status is detected automatically when the ping fails or when the server replies with an HTTP error. It can additionally be signalled proactively by sending a "Server Is Now Offline" message before shutting down.

**Unknown** means the status cannot be verified because the outbound handshake with the peer is not yet complete — the token required for outbound requests is missing. In this state the instance has already been created, for example through an incoming first-contact message, but a reliable ping request can only be issued once the handshake has been confirmed by both sides. Such entries remain visible with the status "Unknown" so that no stale "Online" flag from an early handshake phase lingers.

**Out of service** means the instance has discontinued its operation as announced and the announced date has been reached. It is no longer contacted, and the state is lifted neither by a status check nor by an incoming message from that instance. How this comes about and how the entry can be removed afterwards is described under [Maintenance and Discontinuation](availability/).

### Difference Between "Restricted Use" and "Offline"
The **Restricted Use** state is independent of the Online/Offline status and applies only to servers whose connection is actually usable.

**Restricted Use** means the server is reachable but cannot process data requests due to high load. In this state, no requests for intraday prices or historical prices are sent to this server. However, status messages such as "Server is in maintenance mode during time period", "Server operation will be discontinued as of this date", or "Settings updated" continue to be transmitted. The server can receive incoming messages and respond to them. This setting can be enabled via the user interface for your own server to reduce load during capacity constraints.

### Refreshing the Status of an Instance Immediately
The online status is updated by the **"Track online status"** background task (task ID 20) once, roughly 30 seconds after the server starts. If you do not want to wait for the next startup — for instance after a freshly completed handshake or when you suspect a peer has gone down — the server overview context menu provides the **"Check status now"** entry. It immediately sends a ping to the currently selected instance and updates the corresponding row in the table with the result. The entry is only active for administrators and is disabled for your own server entry.
{{% notice info %}}
Peers without a completed outbound handshake are set to "Unknown" when checked on demand, because no authenticated ping message can be sent without a valid outbound token.
{{% /notice %}}

### Re-admitting a counterpart
If a counterpart loses its credentials — because its database was rebuilt or an older backup was restored, for instance — its renewed **"First contact"** is refused here for as long as a handshake with it is on record. Nor can it get in touch any other way, because an administrative message requires the very connection it is being denied. So that this does not go unnoticed, the icon appears on its row in the **Reconnect requested** column.

The context menu item **"Allow new handshake"**, or a click on that icon, discards the credentials shared with this counterpart, so that its next contact is accepted again. Message history, exchange settings and granted permissions are all kept. The full procedure across both sides is described under [Restoring a Connection](troubleshooting/).

### Different Behavior of Open Server and Push-Open Server
The behavior of these two server types when sharing price data differs fundamentally. To enable active exchange of useful price data, instances should preferably focus on push-open servers. The open server concept is more intended for special cases.
{{% notice info %}}
This price exchange should not lead to neglecting the management of connectors for individual instruments. Connectors remain the primary data source.
{{% /notice %}}

#### Fundamental Differences Between Server Types
| Property | Open Server | Push-Open Server |
|----------|-------------|------------------|
| Data Storage | Directly on local instruments | Separate price data pool |
| Independence | Requires local instruments | Can operate without local instruments |
| Exchange | Can be asymmetric | Always bidirectional |
| Ideal for | Special cases, few partners | Central exchange hub, many partners |

#### Instrument Identification
- **Securities**: ISIN + currency code
- **Currency pairs**: Base currency + quote currency

#### Supplier Selection and Load Balancing
The order in which servers are queried differs between server types:

**Push-Open Servers** use a simple priority-based selection:
- Lower priority values (consumerUsage) are queried first.
- Servers with the same priority are **randomly** selected to distribute load evenly.

**Open Servers** use an optimized score-based selection to maximize successful data exchanges:
1. **Primary**: **Score** = Coverage × Success Rate (higher is better)
   - *Coverage*: Number of requested instruments this supplier supports (from GTNetSupplierDetail)
   - *Success Rate*: Ratio of successfully updated entities from the last 30 days (default 1.0 if no history)
2. **Secondary**: Priority (consumerUsage, lower is better)
3. **Tertiary**: Random shuffle for servers with equal score and priority

This optimization ensures that Open servers with better instrument coverage and higher historical success rates are queried first, reducing unnecessary requests to servers unlikely to provide useful data.

{{% notice note %}}
**Summary of Communication Scenarios:**
- **Push-Open ↔ Push-Open**: Both use and update the shared price pool. Bidirectional exchange.
- **Open → Push-Open**: The open server receives prices from the push-open server's pool and updates its local instruments.
- **Open ↔ Open**: Both update their local instruments directly. Asymmetric exchange possible.
{{% /notice %}}

### Types of Data Exchange
GTNet supports two types of price data for exchange. The detailed workflows are described on separate pages:
- **[Last Price Exchange](../exchange/lastprice/)**: Current day prices for securities and currency pairs. Triggered via the watchlist.
- **[Historical Price Exchange](../exchange/historicalprice/)**: Daily closing prices for the past. Enables filling data gaps.

### Server Configuration
Various settings can be configured for each server:
- **Share Server List**: Determines whether your own server list may be shared with third parties to promote network discovery.
- **Daily query limit**: The number of requests a single peer may direct at your own instance per UTC day. The value applies to each peer individually and not to all of them together; an empty field means unlimited. See [Query Limits](../requestlimits/).
- **Allow Server Creation**: Determines whether unknown servers may be automatically added to the list during the first handshake.

### Limiting the requests of a peer
GTNet limits what a peer may ask for in two places: the **Daily query limit** restricts the number of its requests per UTC day, and the **Query limit** of each exchange kind restricts the number of instruments in a single request. Only the second counts as a violation and, if ignored repeatedly, leads to the peer being refused until an administrator intervenes. Both limits, their effect and what the administrator gets to see of them are described under [Query Limits](../requestlimits/).

### Export and Import of GTNet Data
When migrating a database from one server to another, the GTNet identity (authentication tokens, peer connections, message history) is overwritten. To restore the existing GTNet state on the new server, GTNet provides an export and import function.

The context menu of the server overview (not row-dependent) offers the menu item **"Export GTNet data"**. This function generates a SQL file named `gtnet_export.sql` containing all relevant GTNet tables as DELETE and INSERT statements. The export includes the core configuration: connections, tokens, message history, exchange settings, and exchange logs.

To restore the data, use the same context menu and select **"Import GTNet data"**. A file upload dialog opens where the previously exported `.sql` file can be selected. After a successful import, the system automatically schedules two background tasks to run approximately five minutes later: a configuration synchronization with connected peers and a server status check. Derived data such as the instrument pool, supplier details, and price data pool are not included in the export but are automatically rebuilt by these background tasks. This keeps the export file compact and ensures that the derived data matches the actual state of the new server.

{{% notice note %}}
Export and import are available to administrators only.
{{% /notice %}}

If an instance was moved or its database rebuilt without this export, the credentials of its counterparts are lost and the affected connections can no longer be used. They can be restored afterwards; the procedure is described under [Restoring a Connection](troubleshooting/).

### Expanded Row of an Instance
When a row of the server overview is expanded, four collapsed areas appear which are opened individually as needed: **"Entity Configuration"** with the exchange settings per information object, **"GT Message"** with the message history, **"Maintenance windows"** with the maintenance times announced by this instance, and – for administrators only – **"Delivery attempts"** with the outcome of outgoing background messages. Each area states the number of entries it holds in its title, so it is apparent without opening where there is anything to see.

Messages are displayed in a hierarchy, with related messages grouped together; each message shows direction (sent/received), message type, and status. The delivery attempts add the columns "Message timestamp", "Message code", "Target domain", "Attempt status", "Try count", "Last attempt", "Delivered at", and "Last error" for each target. How to interpret these details when diagnosing a problem is described under [Monitoring GTNet](../monitoring/#message-delivery-attempts).

### Message Categories
GTNet uses a message-based communication protocol where each message belongs to one of three categories:

**Requests** are messages that require a response from the recipient. The sender sees a "Response Expected" status until the recipient responds. Requests include "First contact", "Token refresh", "Query this server list", and "Request for data exchange".

**Responses** are reactions to incoming requests. Depending on the request, the response can be an acceptance or a rejection. For example, a "First contact" is answered with either "Contact successful" or "Contact rejected".

**Announcements** are one-way messages that do not require a response. These include "Server is now offline", "Settings updated", and the maintenance and operation discontinuation messages. Announcements are sent to all connected peers simultaneously depending on their type.

### Peer Connection Lifecycle
The relationship between two GTNet instances goes through different states. The following diagram shows the typical lifecycle:
{{< mermaid >}}
stateDiagram-v2
    [*] --> NoConnection
    NoConnection --> HandshakePending: First contact
    HandshakePending --> Connected: Contact successful
    HandshakePending --> NoConnection: Contact rejected
    Connected --> DataExchangeActive: Data request accepted
    DataExchangeActive --> Connected: Revoke data exchange
    Connected --> NoConnection: Allow new handshake
    DataExchangeActive --> NoConnection: Allow new handshake

    state NoConnection: No Connection
    state HandshakePending: Handshake Pending
    state DataExchangeActive: Data Exchange Active

    note right of Connected : Token refresh possible
    note right of DataExchangeActive : Token refresh possible
{{< /mermaid >}}

After the first contact and a successful response, authentication tokens are exchanged and the connection is established. From this point on, a data exchange can be requested. If the request is accepted, the automatic exchange of price data begins. The data exchange can be terminated at any time by sending a "Revoke data exchange" message. Tokens can be refreshed at any time while a connection exists. From any connected state, **"Allow new handshake"** also leads back to "No Connection"; that is the way to re-admit a counterpart which has lost its credentials.

### Available Messages by State
Which messages can be sent via the context menu depends on the current state of the relationship with the selected peer.

When **no connection** exists (no handshake performed), only "First contact" is available as a directed message.

With an **established connection**, the following messages can be sent: "Token refresh", "Query this server list", and "Admin message". Additionally, "Request for data exchange" is available if the target server accepts requests and no active exchange exists yet. Conversely, "Revoke data exchange" appears only when at least one active exchange already exists. If the server list was previously shared, this permission can be revoked with "My server list may no longer be queried".

For **broadcast messages** (to all connected peers simultaneously), the following options are available: "Server is now offline" is always available. "Server is in maintenance mode during time period" is likewise always available, because several maintenance windows may be announced at the same time; only a window that overlaps an already announced one is rejected. "Server operation will be discontinued as of this date", by contrast, appears only if no open discontinuation exists. Both announcements are described under [Maintenance and Discontinuation](availability/).

### Request-Response Pairs
The following diagram shows the possible responses to each request:
{{< mermaid >}}
flowchart LR
    A["First contact"] -->|Successful| B["Connection established"]
    A -->|Rejected| C["No connection"]
    D["Token refresh"] -->|Accepted| E["Token updated"]
    D -->|Rejected| F["Previous token remains"]
    G["Query server list"] -->|Allowed| H["List shared"]
    G -->|Not allowed| I["Not shared"]
    J["Request data exchange"] -->|Accepted| K["Exchange active"]
    J -->|Rejected| L["No exchange"]
{{< /mermaid >}}

### Reversible Announcements
Both announcements of planned unavailability can be revoked after being sent. Select the sent announcement in the message list of your own entry and choose **"Reverse"** from the context menu; "Maintenance cancelled" or "Operation discontinuation cancelled" is then sent. The menu entry only appears while the announced moment still lies in the future. This informs the connected peers that the announced state no longer applies. See [Maintenance and Discontinuation](availability/).

### Automatic Answers
Without configured rules, every incoming request requires manual approval by the administrator. To simplify operations, rule-based responses can be set up in the [Automatic Answer](../autoanswer/) view. These rules use conditional expressions that evaluate variables such as time of day, timezone difference, or number of existing connections. If no rule is triggered, the request remains pending for manual review.

### Sending Messages
Messages can be sent to the selected instance via the context menu. The available options depend on the connection status and previous messages, as described above under "Available Messages by State". For requests that require a response, the status "Response Expected" is displayed. Administrative free-text messages are managed via the separate [Admin Messages](msgadmin/) tab, which provides its own conversation management with visibility control.
