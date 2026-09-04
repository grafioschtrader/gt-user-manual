---
title: "GTNet"
date: 2026-08-31T22:54:47+01:00
draft: false
weight: 35
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
GTNet (Grafioschtrader Network) is a decentralized peer-to-peer system that enables multiple Grafioschtrader instances to exchange financial data. Each installation can simultaneously act as a data provider and data consumer in the network. This page gives an overview of how the network is put together, of the places in the program where it takes effect, and of its limits. The actual setup is described in [Getting Started with GTNet](gettingstarted/), day-to-day operation in [Monitoring GTNet](monitoring/).

### Overview
GTNet connects different Grafioschtrader instances with each other and enables the mutual exchange of price data. Each instance can decide for itself whether it provides data, receives data, or does both simultaneously. There is no central authority administering the network: an instance knows only those counterparts its administrator has entered or admitted.

The exchange is automated via machine-to-machine communication (M2M). Once two instances have agreed, it runs without any further action by a user; all that is visible in the program is the result, namely additional prices and prefilled security master data.

{{< mermaid >}}
graph LR;
    ME["Own instance"]
    P1["Counterpart, mode Open"]
    P2["Counterpart, mode Push Open"]
    POOL[("Shared price data pool")]
    ME <-->|Intraday prices| P1
    ME <-->|Historical prices| P2
    ME -->|Request for security metadata| P1
    P2 --- POOL
{{< /mermaid >}}

The settings for GTNet carry two prefixes which mirror this structure. Settings starting with **g.gnet** concern the network itself, that is connections, messages and logging. Settings starting with **gt.gtnet** concern what is traded over that network, that is the price data of Grafioschtrader. All of them are described under [Global Settings GTNet](globalsettings/).

### The Two Roles of an Instance
Supplier and consumer are not properties of an installation but of a single exchange. The same instance can supply prices to one counterpart and obtain them from another, and it can supply one kind of data to a counterpart while obtaining another kind from that same counterpart. Both directions may be active at the same time.

Which role applies to an individual instrument is decided by its four exchange options: "Receive intraday price" and "Receive historical price data" make the instance a consumer, "Send intraday prices" and "Send historical price data" make it a supplier. These options are maintained under [Exchange price data](exchange/).

### Types of Data Exchange
GTNet supports three types of data for exchange:
- **Intraday Prices**: Current day prices for securities and currency pairs with OHLCV data (Open, High, Low, Close, Volume).
- **Historical Prices**: Daily closing prices for the past.
- **Security Metadata**: Master data of securities such as ISIN, name, asset class, exchange information and connector settings. This enables the import of security configurations from other GTNet instances.

The two price types are agreed as a running exchange and are then retrieved again and again. Security metadata, in contrast, is queried only when somebody wants to create a security; there is no standing agreement for it.

### How an Instance Takes Part in the Network
For each kind of data an instance decides separately to what extent it serves others. The chosen mode is communicated to the counterparts so they know whether a request is worth making at all.

| Mode | Meaning |
|------|---------|
| Closed | Nothing is supplied for this kind of data. Requests are refused. |
| Open | Requests are answered as far as the instance's own instruments can provide the data asked for. |
| Push Open | Requests are answered, and in addition prices a counterpart sends of its own accord are accepted. These end up in a shared price data pool. |

An instance in "Push Open" mode can therefore also supply prices for instruments it does not hold itself: it passes on what its pool contains. It is the preferred counterpart because it holds more and fresher data. This mode is not available for security metadata, because master data is not pushed but queried case by case. How the modes differ in operation, in particular the order in which suppliers are asked, is described under [Setup GTNet](setup/).

### Where GTNet Takes Effect in Grafioschtrader
GTNet is not a feature you call up but an additional data source that steps in at several places in the program.

{{< mermaid >}}
graph TD;
    W["Update watchlist"] --> LP["Intraday prices from the network"]
    H["Load historical price data"] --> HP["Closing prices and filling of gaps"]
    K["Connector stops delivering"] --> FB["GTNet as substitute source"]
    I["Create a single instrument"] --> MD["Security metadata and connector settings"]
    B["Create securities in bulk"] --> MD
    T["Transaction import with unknown security"] --> MD
{{< /mermaid >}}

#### User-Triggered Usage
- **Watchlist Update**: When updating a watchlist, instruments configured for GTNet exchange can receive intraday prices from connected counterparts. See [Last Price Exchange](exchange/lastprice/) for details.
- **Historical Data Loading**: When loading historical price data, GTNet can fill gaps from other instances. See [Historical Price Exchange](exchange/historicalprice/) for details.
- **Failed connector**: Once an instrument's retry counter reaches the connector limit, GTNet steps in as a substitute source without the failure being hidden as a result. See [Retry Counter and GTNet Fallback](quoteretry/).
- **Security Creation**: When creating a new security, the [GTNet Security Search](../../watchlistinstrument/instrument/securityderived/#gtnet-security-search) allows importing security metadata and connector settings from other GTNet instances.
- **Creating securities in bulk**: The [GTNet Security Import](../../basedata/gtnetsecurityimport/) creates the master data for a whole list of securities in one pass and records what could not be matched in the process.
- **Transaction import**: If an import contains a security that does not exist here yet, it can be created from the network via [Security import for transactions](../../tenantportfolio/securityaccounts/transactionimport/securityimportfortransaction/) instead of being entered by hand.

#### System-Triggered Usage
{{< mermaid >}}
graph LR;
    A[Application Start] -->|Broadcast| B[Server Online Message]
    C[Application Shutdown] -->|Broadcast| D[Server Offline Message]
    E[Settings Changed] -->|Broadcast| F[Settings Updated Message]
{{< /mermaid >}}

- **Application Startup**: When the GT server starts with GTNet enabled, an "online" status is automatically broadcast to all connected counterparts
- **Application Shutdown**: During graceful shutdown, an "offline" status is broadcast to inform the counterparts
- **Settings Changes**: Configuration changes are automatically synchronized to connected instances

### Connection Setup and Authentication
The connection between two GTNet instances is established via a handshake procedure. During the first contact, authentication tokens are exchanged, which are used for further secure communication. Each instance can decide whether it automatically accepts unknown servers or only allows predefined servers.

The tokens can be renewed later. Because the two sides do not adopt the new token at the same moment, the token being replaced stays valid for a limited time. If the answer to a renewal is lost on the way, the connection therefore does not break; the next attempt puts it right again on its own.

### Approving the Data Exchange
A completed handshake only means that two instances know each other and are allowed to talk. It does not yet entitle either of them to price data. For intraday prices and historical prices a data request must also have been made for that kind of data and approved by the other side. Without that approval the instance being asked refuses the request, even when it offers the kind of data in principle.

Security metadata is exempt from this. It is queried individually when needed rather than agreed as an ongoing exchange; here your own setting of whether such requests are accepted is enough.

If a data request is refused, or an existing approval is withdrawn, the exchange ends in the same state on both sides. Both instances then show the kind of data as no longer agreed, and taking it up again requires a new data request.

{{% notice style="note" title="Existing connections" %}}
Peers with which price data has already been exchanged keep their approval automatically when the version changes. Only those who so far had merely a handshake but never an exchange have to make a data request.
{{% /notice %}}

### Messages and Status Notifications
GTNet uses a message-based communication protocol for various purposes:
- **Handshake Messages**: For establishing and confirming connections between instances.
- **Status Messages**: For communicating changes such as online/offline status, maintenance mode, or capacity utilization. When a peer signs off while shutting down, it is recorded as offline straight away; previously it stayed entered as online until the next send failed.
- **Data Requests**: For requesting and approving the exchange of specific data types.

### Who May Do What
GTNet concerns the whole instance rather than an individual tenant. Only a user with administrator rights may therefore change anything about it: creating, editing or deleting peers, sending and answering messages, deleting messages, and maintaining the rules for automatic answers. Every other logged-in user may read the GTNet views, that is the list of peers with their messages, the announced maintenance windows and the exchange log.

What a user is not allowed to do is not offered to them either: the corresponding menu entries and dialogs do not appear at all rather than being refused on save. The exchange options of an individual instrument under [Exchange price data](exchange/) are the one exception. They belong to the instrument and follow its ordinary editing rights: an administrator and a «Privileged user» may change every instrument, every other user only the instruments they created themselves.

Two areas stay reserved for administrators even for reading. The [Automatic Message](autoanswer/) reveals the terms on which this instance admits a peer, and the export of the GTNet data hands out the entire holdings at once. Admin messages marked «Admin only» likewise remain visible to administrators in every view, including where the messages of an individual peer are expanded.

A conversation marked «Admin only» stays that way for every message in it. A reply cannot widen it — neither one written
here nor one that arrives from the peer, which has no way of knowing how the thread it answers is classified. The
classification is decided by the message that started the conversation.

A message that is still waiting for an answer cannot be deleted, and neither can the peer it belongs to, until the
answer has arrived or the request has been rejected. This holds for every kind of request, including the ones the
instances exchange among themselves such as the token refresh and the synchronisation of the exchange settings.

### Query Limits and Load Management
To protect servers from overload, GTNet knows two limits. The **Daily query limit** determines how many requests a single peer may make per UTC day; it works in both directions, because your own instance stops asking of its own accord once the allowance a peer has published is used up. The **Query limit** restricts, per exchange kind, how many instruments a single request may cover. Both are described under [Query Limits](requestlimits/). Additionally, an instance can be marked as "busy", whereby only status messages are communicated.

### Limits: the exchanged data is not verified
{{% notice style="warning" title="Received prices are not verified" %}}
GTNet checks **who** is talking to this instance, but not **whether what is delivered is correct**. Exchange data only with counterparts you trust.
{{% /notice %}}
All of GTNet's protective measures are directed against the wrong sender and against overload: handshake and tokens make sure that only admitted instances are listened to at all, the admission rules decide who is taken in, and the query limits prevent a counterpart from swamping your instance with requests. None of these measures says anything about whether a delivered price is correct.

A received price is neither compared against your own connectors nor checked for plausibility. Nor does it carry a signature or a checksum by which it could later be established whether it was altered on the way.

A stored price also does not record which instance it came from. If it is passed on through a second instance, the original source can no longer be made out. Neither is there a network-wide reputation from which the reliability of a foreign instance could be read; every instance judges its counterparts only from its own experience.

The success rate in the [Exchange Log](exchangelog/) merely measures whether anything was delivered, not whether it was delivered correctly. The same applies to the order in which suppliers are asked: it favours the counterpart that has answered reliably so far, not the one with the better prices.

The instance able to check the least is, of all things, the one that passes on the most. An instance that mainly maintains the shared price data pool holds no security of its own for those prices, and therefore no connector setting to compare them against. Such an instance passes on prices it cannot verify itself.

Three recommendations follow from this. Take in only counterparts whose operator you can judge. Keep a working connector as the primary source for every instrument; GTNet is meant as a supplement, not as a replacement. And check conspicuous prices yourself, for example in the price data view of an instrument, where individual values can also be corrected.

An automatic verification of the exchanged data is planned but not yet implemented.

## Setup Pages
The detailed setup is done via the following subpages:
