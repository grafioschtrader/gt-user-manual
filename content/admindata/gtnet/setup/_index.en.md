---
title: "Setup GTNet"
date: 2026-01-05T22:54:47+01:00
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
- **Usage Restricted**: Indicates whether the instance is currently only accepting limited requests.
- **Exchange Last Price**: Status of intraday price exchange with this instance.
- **Exchange Entity**: Status of historical data exchange with this instance.

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

### Messages
The lower section of the view shows the message history with the selected instance. Messages are displayed in a hierarchy, with related messages grouped together. Each message shows direction (sent/received), message type, and status.

### Message Types
GTNet uses various message types for different purposes:
- **First Contact**: Initial handshake to establish connection.
- **Contact Successful/Rejected**: Response to a contact request.
- **Query This Server List**: Request to share known servers.
- **Request for Data Exchange**: Request for price data exchange.
- **Server Is Now Offline/Online**: Status change of availability.
- **Server Is Busy**: Notification of limited capacity.
- **Settings Updated**: Notification that server settings (Daily Request Limit, Accept Request, Server State, Max Limit) have changed.
- **Maintenance Planned**: Announcement of a planned maintenance window.
- **Operation Discontinued**: Announcement of permanent service discontinuation.

### Sending Messages
Messages can be sent to the selected instance via the context menu. Depending on the connection status and previous messages, different options are available. For requests that require a response, the status "Response Expected" is displayed.
