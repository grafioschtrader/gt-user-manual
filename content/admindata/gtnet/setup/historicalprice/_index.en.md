---
title: "Historical Price Exchange"
date: 2026-01-05T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
The exchange of historical price data allows sharing and receiving past closing prices with other GT-Net instances. This exchange is integrated into the regular historical quote loading process and works automatically for instruments that have the **gtNetHistoricalRecv** flag enabled.

### Enabling GT-Net for Historical Prices
To receive historical prices via GT-Net for a specific security or currency pair, the **gtNetHistoricalRecv** flag must be enabled on that instrument. This can be configured in the instrument's settings.

### Request Structure
When requesting historical prices, the following information is transmitted:
- **Instrument identification**: ISIN + currency (securities) or currency pair
- **Date range**: From-date (the last locally available price + 1 day) to to-date

### Integration with Regular Historyquote Loading
The GT-Net historical price exchange is seamlessly integrated into the regular `catchAllUpSecuritycurrencyHistoryquote` workflow:

{{< mermaid >}}
flowchart TD
    A[Start historical quote loading] --> B{GT-Net enabled?}
    B -->|No| C[Connector fallback for all]
    B -->|Yes| D[Filter by gtNetHistoricalRecv]
    D --> E[Query Push-Open servers]
    E --> F[Query Open servers]
    F --> G[Connector fallback for remaining]
    G --> H{Want-to-receive markers?}
    H -->|Yes| I[Push connector data back]
    H -->|No| J[Done]
    I --> J
{{< /mermaid >}}

### Detailed Workflow

#### Step 1: Filter by gtNetHistoricalRecv
Only instruments with the `gtNetHistoricalRecv` flag enabled are candidates for GT-Net exchange. Other instruments skip directly to connector-based loading.

#### Step 2: Query Push-Open Servers
Push-Open servers are queried first by priority (with randomization within the same priority level):
- The request contains the instrument list with the desired date range.
- The response contains OHLCV data (open, high, low, close, volume) for each available day.
- Instruments that receive data are marked as "filled".

#### Step 3: Query Open Servers
For instruments not yet filled, Open servers are queried. These can additionally return "want to receive" markers.

#### Step 4: Connector Fallback
Instruments not filled by GT-Net fall back to the regular connector-based loading (Yahoo, Finnhub, etc.). This includes:
- Instruments without `gtNetHistoricalRecv` enabled
- Instruments that were not filled by any GT-Net server

#### Step 5: Push Connector Data Back
After connectors have loaded data, this data is pushed back to GT-Net servers that expressed "want to receive" for those instruments.

### Data Storage
The storage of received historical prices depends on whether the instrument exists locally:

| Instrument Type | Storage Location |
|-----------------|------------------|
| Local instrument (exists in own database) | `historyquote` table |
| Foreign instrument (only in GT-Net pool) | `gt_net_historyquote` table |

{{< mermaid >}}
flowchart LR
    A[Received prices] --> B{Instrument local?}
    B -->|Yes| C[historyquote table]
    B -->|No| D[gt_net_historyquote table]
{{< /mermaid >}}

### The "Want to Receive" Mechanism
A special feature of historical price data exchange is the bidirectional data exchange via the "want to receive" mechanism.

#### How It Works
When an Open server cannot provide data for a requested instrument but wants to receive data for this instrument, it sends back a "want to receive" marker:
- The marker contains the date from which data is desired.
- The requesting server checks if it has local data for this instrument (including data loaded via connectors).
- If yes, it proactively sends this data to the interested server.

{{< mermaid >}}
sequenceDiagram
    autonumber
    participant A as Requesting Server
    participant B as Open Server
    participant C as Connector

    A->>B: Request: Historical prices for Instrument X
    Note over B: Has no data,<br/>but wants to receive
    B-->>A: Response: Want to receive from Date Y
    Note over A: Instrument not filled
    A->>C: Fallback: Fetch from connector
    C-->>A: Historical prices
    Note over A: Stores data locally
    A->>B: Push: Historical prices from Date Y
    Note over B: Stores received data
{{< /mermaid >}}

#### Benefits of the Mechanism
- **Bidirectional exchange**: Both servers benefit from the data exchange.
- **Connector data sharing**: Data fetched from connectors can be shared with the GT-Net community.
- **Automatic completion**: Servers can automatically fill data gaps.
- **Efficiency**: Data exchange occurs specifically for needed time periods.

### Differences from Intraday Exchange

| Aspect | Historical Prices | Intraday Prices |
|--------|-------------------|-----------------|
| Integration | Part of regular historyquote loading | Part of watchlist update |
| Enable Flag | gtNetHistoricalRecv | gtNetLastpriceRecv |
| Data Scope | OHLCV for each day | Current price with timestamp |
| Storage (foreign) | gt_net_historyquote | gt_net_lastprice |
| Want to Receive | Yes | No |
| Connector Data Push-Back | Yes | No |
| Retroactive Data | Yes | No |

### Data Quality
When storing received data, duplicates are avoided:
- Before storing, the system checks if an entry already exists for the date.
- Only new records are stored.
- Existing data is not overwritten.

{{% notice note %}}
The historical price data exchange complements the intraday exchange. While the intraday exchange is optimized for current prices, the historical exchange enables filling data gaps from the past - both from GT-Net servers and by sharing connector-fetched data with other instances.
{{% /notice %}}
