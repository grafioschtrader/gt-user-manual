---
title: "Last Price Exchange"
date: 2026-01-05T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GT-Net is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
The exchange of intraday price data (last price) allows sharing and receiving current price data with other GT-Net instances. The process is triggered through the watchlist and follows a defined workflow.

### Trigger: The Watchlist
The intraday price data exchange is triggered via the **watchlist**. When a watchlist is updated, the system checks for each instrument whether it is configured for GT-Net reception:
- Only instruments with the **GT-Net Last Price Receive** option enabled are considered for exchange.
- Instruments without this option are updated exclusively via the configured connector.

### Instrument Identification
Instruments are uniquely identified by:
- **Securities**: ISIN + currency code
- **Currency pairs**: Base currency + quote currency

### Price Exchange Workflow
The exchange follows a multi-step process where different data sources are queried sequentially:

{{< mermaid >}}
flowchart TD
    A[Watchlist Update] --> B{GT-Net enabled?}
    B -->|No| K[Use connectors only]
    B -->|Yes| C[Filter instruments by GT-Net receive]
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
    J -->|No| L
    M --> L
{{< /mermaid >}}

### Detailed Workflow

#### Step 1: Filter by GT-Net Configuration
The system separates watchlist instruments into two groups:
- **GT-Net-enabled instruments**: Instruments with the "GT-Net Last Price Receive" option enabled
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
    participant L as Local Server
    participant P1 as Push-Open #1 (Prio 1)
    participant P2 as Push-Open #2 (Prio 2)

    L->>P1: Instruments + timestamps
    P1-->>L: Newer prices
    Note over L: Update local instruments
    L->>P2: Remaining instruments + timestamps
    P2-->>L: Newer prices
    Note over L: Update local instruments
{{< /mermaid >}}

#### Step 4: Query Open Servers
For instruments that still don't have a current price, open servers are queried. The flow is similar to push-open servers, however:
- Open servers read from their local instruments (no separate pool).
- The exchange can be **asymmetric** (depending on the "Exchange Price Data Securities" configuration).

#### Step 5: Connectors for Remaining Instruments
Instruments that received prices neither from the own pool nor from remote servers are updated via their configured **connectors** (data sources like Yahoo, Finnhub, etc.).

#### Step 6: Update Own Price Pool (Push-Open Servers Only)
If the own server is configured as push-open, prices obtained via connectors are stored in the local price pool. This makes these prices available for future requests from other instances.

### Differences Between Server Types

| Aspect | Push-Open Server | Open Server |
|--------|------------------|-------------|
| Data Storage | Separate price pool | Directly on local instruments |
| Exchange | Always bidirectional | Can be asymmetric |
| Query Order | Queried first | Queried after Push-Open |
| Own Pool | Checked before remote servers | Not available |
| Connector Updates | Stored in pool | Local instruments only |

### Priorities and Load Balancing
The order of server queries is determined by **priority** (consumerUsage):
- Lower priority values are queried first.
- Servers with the same priority are **randomly** selected to distribute load evenly.
- The query stops as soon as all instruments have a current price.

{{% notice info %}}
The price exchange is based on timestamp comparisons. The newer price is always preferred, regardless of which server it comes from.
{{% /notice %}}
