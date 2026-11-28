---
title: "ISIN change"
date: 2026-10-21T12:00:00+02:00
lastmod: 2026-10-21T12:00:00+02:00
draft: false
weight: 5
archetype: "default"
---
When a security receives a new ISIN — for example due to a corporate merger, restructuring, or regulatory change — all holdings must be transferred from the old to the new security. GT supports this process as a system-wide action: an administrator creates the ISIN change once, and each affected tenant can apply it to their own holdings.

## Workflow overview
The following diagram shows the typical workflow of an ISIN change — from discovery by the user to application on their own holdings:

{{< mermaid >}}
sequenceDiagram
    participant U as User
    participant S as System (GT)
    participant A as Administrator

    U->>S: Discovers ISIN change<br/>(e.g. via watchlist)
    U->>A: Reports ISIN change<br/>via internal mail
    A->>S: Creates ISIN change<br/>(System Actions)
    S->>S: References existing or creates<br/>new security, counts affected tenants
    S->>S: Looks up a new security at the data providers,<br/>chooses the connectors and loads the prices
    S->>U: Notification<br/>via internal mail
    U->>S: Applies ISIN change
    S->>S: Reassigns transactions from action<br/>date onwards to new security
    S->>S: Generates sell (old ISIN)<br/>and buy (new ISIN)
    S-->>U: Status: Applied
{{< /mermaid >}}

## Administrator workflow
The administrator creates an ISIN change in the [Security action]({{% ref "/basedata/securityaction" %}}) TreeTable by **right-clicking** on the root node **System Actions (ISIN Changes)** → **Create ISIN change**.

### Dialog fields
| Field | Required | Description |
|-------|----------|-------------|
| **Old security** | Yes | The existing security whose ISIN is changing. Selected via the search button. |
| **New ISIN** | Yes | The new ISIN that the security will receive. If a security with this ISIN already exists, it is referenced; otherwise GT automatically creates a new security. |
| **Action date** | Yes | Date on which the change takes effect. The closing price of this date is used for the generated transactions. |
| **From units** | No | Split factor numerator. Default value 1. For a pure ISIN change without a stock split, this remains at 1. |
| **To units** | No | Split factor denominator. Default value 1. For a simultaneous stock split of e.g. 1:10, enter 10 here. |
| **Note** | No | Optional note about the ISIN change. |

After saving, GT references the existing security or creates a new one with the specified ISIN, counts the affected tenants, and sends them an internal mail notification about the available ISIN change. When GT has to create a new security, it first looks it up at the data providers; saving may therefore take up to a minute.
### How GT creates the new security
If no security with the new ISIN exists yet, GT runs the same [lookup at the data providers]({{% relref "/watchlistinstrument/instrument/securityderived/security/securitylookup" %}}) that is available via **Search** in the edit dialog of a security. It looks up the new ISIN on the stock exchange, in the currency and with the asset class of the old security; its ticker serves as an additional search term. Unlike in the edit dialog, GT chooses the listing and the connectors itself. This is necessary because the URL extensions of many connectors contain the ISIN, and those of the old security would deliver no data or wrong data for the new ISIN.

GT proceeds the same way for each of the four connectors **History connector**, **Intraday connector**, **Dividend connector** and **Split connector**:
- GT keeps the connector of the old security if its trial request for the new ISIN succeeded, and sets the URL extension the lookup determined for the new ISIN.
- Otherwise GT chooses the first connector whose trial request succeeded.
- In both cases a history connector only qualifies if it delivers prices from the action date on, because this date becomes **Trading from** of the new security.
- If no suitable connector is found, the setting of the old security is kept for this connector.

{{< mermaid >}}
graph TD
    A[Connector of the old security] --> B{Trial request with new<br/>URL extension successful?}
    B -->|Yes| C[Keep connector,<br/>set new URL extension]
    B -->|No| D{Other connector with<br/>successful trial request?}
    D -->|Yes| E[Choose the first such connector]
    D -->|No| F[Keep the setting of<br/>the old security]
{{< /mermaid >}}

If the data providers state that the instrument distributes nothing, it receives no dividend connector. If the lookup returns several listings, GT prefers the one on which the history connector of the old security works. Name, **Ticker/symbol**, **Denomination**, **Distribution Frequency**, **Issuer country**, **Stock exchange hyperlink** and **Product hyperlink** come from the data found; if a value is missing, GT takes that of the old security. Currency, asset class, stock exchange and leverage factor always match the old security, and **Trading from** is set to the action date.

If the lookup does not find the new ISIN, for example because the data providers do not carry it yet, GT copies the old security. The name then receives the suffix " (new ISIN)", and all four connectors are copied with their URL extensions. Notes and watchlist assignments are never copied.

In both cases GT saves the new security the same way the edit dialog does: the connectors are checked, and historical prices, dividends and splits are loaded in the background. If this check fails, the ISIN change is not saved. In that case, create the new security yourself first; the ISIN change then references it.

{{% callout style="warning" title="Verify connector settings" %}}
The trial requests are a snapshot, and a copy of the old security takes over its settings unchanged. After the ISIN change is created, verify that the new security actually has historical price data and that the connectors deliver correct data, especially if its name ends with " (new ISIN)". Adjust the settings of the new security if necessary.
{{% /callout %}}

{{% callout style="info" title="Performance watchlist" %}}
The new instrument is not automatically added to the watchlist designated as performance. To add it, open the corresponding watchlist and use the **Add existing instrument** search dialog with the **With active holding** checkbox activated.
{{% /callout %}}

### Deleting an ISIN change
The administrator can remove an ISIN change via **right-click** → **Delete ISIN change**. This is only possible as long as no tenant has applied the action.

## User workflow

### Applying an ISIN change
Once an administrator has created an ISIN change, every affected tenant sees the action in their TreeTable with the status **Not applied**. To apply the change: **right-click** on the corresponding row → **Apply ISIN change**.

GT then performs the following steps:
1. All existing transactions of the tenant on or after the action date that reference the old security are **reassigned to the new security**. This includes dividends, buy/sell transactions, and any other operations recorded after the ISIN change took effect.
2. The closing price of the old security on the action date is retrieved.
3. For each affected securities account, a **sell transaction** for the old security and a **buy transaction** for the new security are generated based on the remaining pre-action-date holdings.
4. If split factors are set (From units / To units not equal to 1:1), the unit count and price on the buy side are adjusted accordingly.
5. The status changes to **Applied**.

#### ISIN change already carried out
Before booking, GT checks for every securities account whether the ISIN change has already been carried out there. This is the case when, at most ten days before or after the action date, the whole position of the old security was sold and the new security was bought in the same securities account. The purchased units must match the split ratio; a deviation of less than one unit is accepted, because banks settle fractions in cash. For such a securities account GT books no second pair, and a message states the number of these accounts.

A pair you entered yourself stays unchanged. A pair with the comment "System-Created" that GT created earlier and that no longer belongs to any application is taken over by the new application. GT prices it again: the sale receives the closing price of its day in the share basis traded at that time, the purchase the same amount. Older program versions had booked such pairs with the split-adjusted price.

{{% callout style="warning" title="Prerequisite" %}}
A closing price of the old security must exist for the action date as soon as GT books a pair. A pair that is taken over needs the closing price on its own date. If the price is missing, the action cannot be applied. If the change had already been carried out in every securities account, no price is needed.
{{% /callout %}}

### Reversing an ISIN change
An already applied ISIN change can be reversed via **right-click** → **Reverse ISIN change**. This deletes all automatically generated sell and buy transactions, including a pair that was taken over. A pair you entered yourself remains. The status changes to **Reversed**. The action can then be applied again.

## Practical examples
Inverse and leveraged ETFs lose value over time due to the daily reset mechanism and so-called volatility decay. When the price falls too far, issuers perform a reverse split to maintain minimum exchange listing prices. In the USA, a reverse split requires a new CUSIP number and thus also a new ISIN, as the security structure changes substantially. Such ISIN changes with a simultaneous split are a typical use case for the GT function.

### Examples: ProShares inverse ETFs
| Name | Reverse split | Effective date | Old ISIN | New ISIN |
|------|---------------|----------------|----------|----------|
| ProShares Short QQQ (PSQ) | 1:5 | 2024-04-10 | US74347B7148 | US74349Y8378 |
| ProShares Short S&P500 (SH) | 1:4 | 2024-11-07 | US74347B4251 | US74349Y7537 |

### Recording in GT
For the **ProShares Short QQQ** example, the administrator would record the ISIN change as follows: select the instrument with ISIN US74347B7148 as the **Old security**, enter US74349Y8378 as the **New ISIN**, and set the **Action date** to 2024-04-10. Since a reverse split of 1:5 occurred simultaneously, **From units** is set to 1 and **To units** to 5. When applied, GT then generates the corresponding sell and buy transactions with adjusted unit counts.

{{% callout style="tip" title="Act early" %}}
ISIN changes caused by reverse splits are often noticed late by users — for example, when historical price data is no longer updated or portfolio reports show implausible values. The longer the change goes unnoticed, the more transactions have been recorded under the old ISIN that would then need to be corrected manually. GT's ISIN change function automatically reassigns all affected transactions to the new security, saving a laborious manual correction process.
{{% /callout %}}
