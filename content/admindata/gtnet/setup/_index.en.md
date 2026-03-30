---
title: "Setup GTNet"
date: 2026-03-29T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
This view manages all known GTNet instances and controls communication with them.

### Setting Up Your Own Server
Before communication with other participants is possible, your own GT server must be registered. It is important that the server is accessible from outside. After registering your own server, its ID is stored as a reference. Subsequently, additional reachable instances can be registered for communication.

### Server Overview
The table shows all known GTNet instances with the following information:
- **Domain**: The base URL of the instance for M2M communication.
- **Time Zone**: The server's time zone, helps estimate operating hours.
- **Server Online**: Shows the current online status of the instance (Online, Offline, or Unknown).
- **Restricted Use**: Indicates whether the instance is currently at full capacity and cannot process data requests.
- **Exchange Last Price**: Status of intraday price exchange with this instance.
- **Exchange Entity**: Status of historical data exchange with this instance.

### Difference Between "Restricted Use" and "Offline"
These two states differ fundamentally in their impact on communication.

**Restricted Use** means the server is reachable but cannot process data requests due to high load. In this state, no requests for intraday prices or historical prices are sent to this server. However, status messages such as "Maintenance Planned", "Operation Discontinued", or "Settings Updated" continue to be transmitted. The server can receive incoming messages and respond to them. This setting can be enabled via the user interface for your own server to reduce load during capacity constraints.

**Offline** means the server is not reachable. No communication takes place at all. This status is detected automatically when the server does not respond, or can be signaled by sending a "Server Is Now Offline" message before shutting down.

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
- **[Last Price Exchange](lastprice/)**: Current day prices for securities and currency pairs. Triggered via the watchlist.
- **[Historical Price Exchange](historicalprice/)**: Daily closing prices for the past. Enables filling data gaps.

### Server Configuration
Various settings can be configured for each server:
- **Share Server List**: Determines whether your own server list may be shared with third parties to promote network discovery.
- **Daily Request Limit**: Maximum number of data requests this server may make to your own instance per day.
- **Allow Server Creation**: Determines whether unknown servers may be automatically added to the list during the first handshake.

### Export and Import of GTNet Data
When migrating a database from one server to another, the GTNet identity (authentication tokens, peer connections, message history) is overwritten. To restore the existing GTNet state on the new server, GTNet provides an export and import function.

The context menu of the server overview (not row-dependent) offers the menu item **"Export GTNet data"**. This function generates a SQL file named `gtnet_export.sql` containing all relevant GTNet tables as DELETE and INSERT statements. The export includes the core configuration: connections, tokens, message history, exchange settings, and exchange logs.

To restore the data, use the same context menu and select **"Import GTNet data"**. A file upload dialog opens where the previously exported `.sql` file can be selected. After a successful import, the system automatically schedules two background tasks to run approximately five minutes later: a configuration synchronization with connected peers and a server status check. Derived data such as the instrument pool, supplier details, and price data pool are not included in the export but are automatically rebuilt by these background tasks. This keeps the export file compact and ensures that the derived data matches the actual state of the new server.

{{% notice note %}}
Export and import are available to administrators only.
{{% /notice %}}

### Messages
The lower section of the view shows the message history with the selected instance. Messages are displayed in a hierarchy, with related messages grouped together. Each message shows direction (sent/received), message type, and status.

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
    Connected --> Connected: Token refresh
    Connected --> DataExchangeActive: Data request accepted
    DataExchangeActive --> Connected: Revoke data exchange
    DataExchangeActive --> DataExchangeActive: Token refresh

    state NoConnection: No Connection
    state HandshakePending: Handshake Pending
    state DataExchangeActive: Data Exchange Active
{{< /mermaid >}}

After the first contact and a successful response, authentication tokens are exchanged and the connection is established. From this point on, a data exchange can be requested. If the request is accepted, the automatic exchange of price data begins. The data exchange can be terminated at any time by sending a "Revoke data exchange" message. Tokens can be refreshed at any time while a connection exists.

### Available Messages by State
Which messages can be sent via the context menu depends on the current state of the relationship with the selected peer.

When **no connection** exists (no handshake performed), only "First contact" is available as a directed message.

With an **established connection**, the following messages can be sent: "Token refresh", "Query this server list", and "Admin message". Additionally, "Request for data exchange" is available if the target server accepts requests and no active exchange exists yet. Conversely, "Revoke data exchange" appears only when at least one active exchange already exists. If the server list was previously shared, this permission can be revoked with "My server list may no longer be queried".

For **broadcast messages** (to all connected peers simultaneously), the following options are available: "Server is now offline" is always available. "Maintenance mode" appears only if no open maintenance announcement exists. "Operation discontinued" appears only if no open operation discontinuation exists.

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
The announcements "Maintenance mode" and "Operation discontinued" can be revoked after being sent. While such an announcement is open, the context menu offers the corresponding cancellation option instead of a new announcement of the same type: "Maintenance cancelled" or "Operation discontinuation cancelled". This informs the connected peers that the announced state no longer applies.

### Automatic Answers
Without configured rules, every incoming request requires manual approval by the administrator. To simplify operations, rule-based responses can be set up in the [Automatic Answer](../autoanswer/) view. These rules use conditional expressions that evaluate variables such as time of day, timezone difference, or number of existing connections. If no rule is triggered, the request remains pending for manual review.

### Sending Messages
Messages can be sent to the selected instance via the context menu. The available options depend on the connection status and previous messages, as described above under "Available Messages by State". For requests that require a response, the status "Response Expected" is displayed. Administrative free-text messages are managed via the separate [Admin Messages](msgadmin/) tab, which provides its own conversation management with visibility control.
