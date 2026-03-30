---
title: "Last Price Exchange"
date: 2026-02-04T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
The exchange of intraday price data (last price) allows sharing and receiving current price data with other GTNet instances. The process is triggered through the watchlist and follows a defined workflow.

### Trigger: The Watchlist
The intraday price data exchange is triggered via the **watchlist**. When a watchlist is updated, the system checks for each instrument whether it is configured for GTNet reception:
- Only instruments with the **GTNet Last Price Receive** option enabled are considered for exchange.
- Instruments without this option are updated exclusively via the configured connector.

### Instrument Identification
Instruments are uniquely identified by:
- **Securities**: ISIN + currency code
- **Currency pairs**: Base currency + quote currency

### Price Exchange Workflow
The exchange follows a multi-step process where different data sources are queried sequentially:

{{< mermaid >}}
flowchart TD
    A[Watchlist Update] --> B{GTNet enabled?}
    B -->|No| K[Use connectors only]
    B -->|Yes| C[Filter instruments by GTNet receive]
    C --> D{Own server Push-Open?}
    D -->|Yes| E[Query own price pool]
    E --> F{All prices found?}
    F -->|Yes| L[Done]
    F -->|No| G[Query Push-Open servers]
    D -->|No| G
    G --> H[Query Open servers]
    H --> I[Connectors for remaining instruments]
    I --> J{Own server Push-Open?}
    J -->|Yes| M[Update own price pool]
    J -->|No| N{Own server Open?}
    M --> L
    N -->|Yes| O[Async: Push updated prices<br/>to Push-Open servers]
    N -->|No| L
    O --> L
{{< /mermaid >}}

### Detailed Workflow

#### Step 1: Filter by GTNet Configuration
The system separates watchlist instruments into two groups:
- **GTNet-enabled instruments**: Instruments with the "GTNet Last Price Receive" option enabled
- **Connector-only instruments**: All other instruments

#### Step 2: Query Own Price Pool (Push-Open Servers Only)
If the own server is configured as **Push-Open**, the local price pool is queried first. This enables fast responses without network communication if prices are already available in the pool.

#### Step 3: Query Push-Open Servers
For instruments not yet filled, all configured push-open servers are queried:
- Servers are sorted by **priority**.
- Servers with the same priority are **randomly** selected (load balancing).
- Each request contains:
  - List of instruments (ISIN/currency or currency pair)
  - Timestamp of the last known price for each instrument
- The response contains only prices that are **newer** than those specified in the request.

{{< mermaid >}}
sequenceDiagram
    autonumber
    participant L as Local Server (Open)
    participant P as Push-Open Server
    participant O as Open Server
    participant C as Connector

    L->>P: Request: Prices for Instrument X, Y, Z + timestamps
    Note over P: Has newer prices for X and Y
    P-->>L: Response: Prices for X and Y
    Note over L: X and Y updated,<br/>Z still open.<br/>Remember timestamp from P.
    L->>O: Request: Price for Instrument Z + timestamp
    Note over O: Has no newer price
    O-->>L: Response: No newer data
    Note over L: Z still not filled
    L->>C: Fallback: Fetch price for Z
    C-->>L: Current price for Z
    Note over L: Z updated.<br/>Send result to frontend.
    rect rgb(230, 245, 255)
    Note over L,P: Asynchronous Push-Back (Open servers only)
    L-)P: Push: Newer prices for Z
    Note over P: Price pool updated
    P--)L: Acknowledgement
    end
{{< /mermaid >}}

#### Step 4: Query Open Servers
For instruments that still don't have a current price, open servers are then queried. These are sorted by a **score** calculated from coverage multiplied by success rate. Servers with higher scores are queried first. With equal scores, servers are sorted by priority (lower first) and then randomly. The success rate is calculated from exchange logs of the last 30 days (default: 1.0 if no history exists). Open servers:
- Read from their local instruments (no separate pool).
- The request is filtered: Only instruments that the respective server supports are sent.
- The exchange can be **asymmetric** (depending on the "Exchange Price Data Securities" configuration).

#### Step 5: Connectors for Remaining Instruments
Instruments that received prices neither from the own pool nor from remote servers are updated via their configured **connectors** (data sources like Yahoo, Finnhub, etc.).

#### Step 6: Update Own Price Pool (Push-Open Servers Only)
If the own server is configured as push-open, prices obtained via connectors are stored in the local price pool. This makes these prices available for future requests from other instances.

#### Step 7: Async Push-Back to Push-Open Servers (Open Servers Only)
If the own server is configured as **Open** (not Push-Open), it sends updated prices **asynchronously** to all previously contacted Push-Open servers after completing the exchange. This process:
- Runs in the background and does not block the response to the frontend.
- Sends only prices that are **newer** than those originally received from the Push-Open server.
- Allows Push-Open servers to enrich their price pool with data they don't have themselves.
- Automatically creates new instrument entries in the Push-Open server's price pool if they don't exist yet.

{{% notice tip %}}
This bidirectional data flow ensures that Push-Open servers can benefit from the connectors of Open servers. An Open server that receives prices via its connector automatically shares them with the Push-Open servers it previously requested data from.
{{% /notice %}}

### Differences Between Server Types

| Aspect | Push-Open Server | Open Server |
|--------|------------------|-------------|
| Data Storage | Separate price pool | Directly on local instruments |
| Exchange | Always bidirectional | Can be asymmetric |
| Query Order | Queried first | Queried after Push-Open |
| Sorting | Priority + randomization | Score (Coverage × Success Rate) |
| Instrument Filtering | All GTNet instruments | Only supported instruments |
| Own Pool | Checked before remote servers | Not available |
| Connector Updates | Stored in pool | Local instruments only |
| Push-Back | Receives prices from Open servers | Sends async prices to contacted Push-Open servers |

### Priorities and Load Balancing
The order of server queries differs depending on server type:

**Push-Open servers** are sorted by **priority** (consumerUsage):
- Lower priority values are queried first.
- Servers with the same priority are **randomly** selected to distribute load evenly.

**Open servers** are sorted by **score**:
- Score = Coverage × Success Rate (from the last 30 days)
- Higher score values are queried first.
- With equal scores, servers are sorted by priority (lower first) and then randomly.

The query stops as soon as all instruments have a current price.

{{% notice info %}}
The price exchange is based on timestamp comparisons. The newer price is always preferred, regardless of which server it comes from.
{{% /notice %}}
