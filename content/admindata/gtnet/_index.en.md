---
title: "GTNet"
date: 2026-01-20T22:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
GTNet (Grafioschtrader Network) is a decentralized peer-to-peer system that enables multiple Grafioschtrader instances to exchange financial data. Each installation can simultaneously act as a data provider and data consumer in the network.

### Concept and Functionality
GTNet connects different Grafioschtrader instances with each other and enables the mutual exchange of price data. Each instance can decide for itself whether it provides data, receives data, or does both simultaneously. The exchange is automated via secure machine-to-machine communication (M2M).

### Types of Data Exchange
GTNet supports three types of data for exchange:
- **Intraday Prices**: Current day prices for securities and currency pairs with OHLCV data (Open, High, Low, Close, Volume).
- **Historical Prices**: Daily closing prices for the past.
- **Security Metadata**: Master data of securities such as ISIN, name, asset class, exchange information and connector settings. This enables the import of security configurations from other GTNet instances.

### Connection Setup and Authentication
The connection between two GTNet instances is established via a handshake procedure. During the first contact, authentication tokens are exchanged, which are used for further secure communication. Each instance can decide whether it automatically accepts unknown servers or only allows predefined servers.

### Messages and Status Notifications
GTNet uses a message-based communication protocol for various purposes:
- **Handshake Messages**: For establishing and confirming connections between instances.
- **Status Messages**: For communicating changes such as online/offline status, maintenance mode, or capacity utilization.
- **Data Requests**: For requesting and approving the exchange of specific data types.

### Request Limits and Load Management
To protect servers from overload, daily request limits can be defined. These apply both to incoming requests from other instances and to outgoing requests to other servers. Additionally, an instance can be marked as "busy", whereby only status messages are communicated.

---

## Getting Started with GTNet
This section provides a step-by-step guide to set up your GT instance for participation in the GTNet network.

### Prerequisites
To participate in GTNet, your own Grafioschtrader server must be accessible from outside via HTTPS. Ensure that your server is properly configured for machine-to-machine (M2M) communication. If you encounter issues with external connectivity, consult the [Problem Solving Guide](https://github.com/grafioschtrader/grafioschtrader/wiki/Problem-solving) in the wiki.

### Step 1: Enable GTNet in Global Settings
First, activate GTNet functionality via the [Global Settings GTNet](globalsettings/):
- Set **gt.gtnet.use** to a value greater than 0 to enable GTNet
- Optionally enable **gt.gtnet.use.log** to track data exchanges

### Step 2: Register Your Own Instance
Before communicating with other participants, you must register your own server in the [GTNet Setup](setup/) view:
1. Open the GTNet Setup view
2. Create an entry for your own server with your domain URL
3. After saving, the system stores your instance ID in the global setting **gt.gtnet.my.entry.id**

### Step 3: Add Remote Instances
Add one or more remote GTNet instances to establish communication:
1. In the [GTNet Setup](setup/) view, add entries for remote servers
2. **Recommendation**: Prefer **PUSH_OPEN** servers, as they maintain an active data pool and enable bidirectional exchange without requiring local instruments
3. Configure the daily request limit and other settings as needed

### Step 4: Establish Initial Connection
Request data exchange directly, which automatically handles the handshake process:
1. Select the remote server entry
2. Use the context menu to send a **Request for Data Exchange** message
3. The system automatically performs the handshake and token exchange if this is the first contact
4. Wait for the response (acceptance or rejection)
5. Once accepted, the data exchange is activated and authentication tokens are stored for future communication

### Step 5: Configure Automatic Answers (Recommended)
To avoid manually responding to every incoming request, configure automatic response rules in the [Automatic Answer](autoanswer/) view:
1. Define rules for different message types (e.g., first contact, data exchange requests)
2. Set conditions using variables like time of day, daily request count, or domain patterns
3. Specify whether to accept or reject requests automatically
4. Configure waiting times after rejections to prevent repeated requests

{{% notice tip %}}
Without automatic answer rules, every incoming request requires manual approval by the administrator. Setting up appropriate rules significantly reduces administrative overhead.
{{% /notice %}}

### Step 6: Configure Securities for Exchange
Define which instruments should participate in data exchange:
1. Open the [Exchange Price Data](exchange/) view
2. For each security or currency pair, configure the four exchange options:
   - Receive intraday prices
   - Receive historical prices
   - Send intraday prices
   - Send historical prices
3. Save your changes

---

## Where GTNet Is Used
GTNet integrates into various parts of the application, both automatically by the system and through user actions.

### System-Triggered Usage
{{< mermaid >}}
graph LR;
    A[Application Start] -->|Broadcast| B[Server Online Message]
    C[Application Shutdown] -->|Broadcast| D[Server Offline Message]
    E[Settings Changed] -->|Broadcast| F[Settings Updated Message]
{{< /mermaid >}}

- **Application Startup**: When the GT server starts with GTNet enabled, an "online" status is automatically broadcast to all connected peers
- **Application Shutdown**: During graceful shutdown, an "offline" status is broadcast to inform peers
- **Settings Changes**: Configuration changes are automatically synchronized to connected instances

### User-Triggered Usage
- **Watchlist Update**: When updating a watchlist, instruments configured for GTNet exchange can receive intraday prices from connected peers. See [Last Price Exchange](setup/lastprice/) for details.
- **Historical Data Loading**: When loading historical price data, GTNet can fill gaps from peer instances. See [Historical Price Exchange](setup/historicalprice/) for details.
- **Security Creation**: When creating new securities, the [GTNet Security Search](../../watchlistinstrument/instrument/securityderived/#gt-net-security-search) allows importing security metadata and connector settings from other GTNet instances.

---

## Monitoring GTNet
GTNet provides several ways to monitor the status and activity of data exchanges.

### Static Overview: Exchange Price Data
The [Exchange Price Data](exchange/) view provides a static overview of which instruments are configured for exchange and which OPEN instances can supply data:
- View all securities and currency pairs with their exchange settings
- Expand rows to see supplier details (which peers can provide data for each instrument)
- Useful for understanding the current configuration state

### Dynamic View: Exchange Log
The [Exchange Log](exchangelog/) view offers a dynamic, time-based view of actual exchange activity:
- See statistics for both intraday and historical price exchanges
- Track supplier and consumer statistics per connected instance
- View aggregated data by day, week, month, and year
- Requires logging to be enabled via **gt.gtnet.use.log** in [Global Settings](globalsettings/)

### Background Tasks
Several background tasks handle GTNet operations automatically. These can be monitored in the [Task Data Change Monitor](../taskdatachangemonitor/):

| Task ID | Name | Description |
|---------|------|-------------|
| 20 | Track online status | Checks and updates the online/busy status of all configured GTNet servers |
| 22 | Aggregate exchange logs | Summarizes exchange log entries into daily, weekly, monthly, and yearly periods |
| 23 | Synchronize configurations | Syncs exchange configurations with connected peers and updates supplier details |
| 24 | Broadcast settings changes | Sends settings updates to all connected peers when local settings change |
| 25 | Deliver future messages | Handles scheduled messages like maintenance announcements |

For detailed descriptions of each task, see [Background Tasks](../taskdatachangemonitor/taskdescription/).

---

## Setup Pages
The detailed setup is done via the following subpages:
