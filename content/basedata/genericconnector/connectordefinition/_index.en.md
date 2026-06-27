---
title: "Connector definition"
date: 2026-05-27T12:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
The connector definition contains the basic settings of the data provider.
## Connector settings
- **Short ID**: Unique short identifier (2–32 characters). Used internally as the connector ID.
- **Readable name**: Human-readable name displayed in the user interface.
- **Description**: Optional description in German and English.
- **Domain URL**: Base URL of the data provider (with trailing slash).
- **Needs API key**: Whether the provider requires an API key. If yes, the key must be stored in the connector API key table.
- **Supports security**: Whether security quotes are supported.
- **Supports currency**: Whether currency pair quotes are supported.
## Rate limit settings
Many APIs restrict the number of requests per time period. The rate limit configuration ensures that GT respects these limits.
- **Rate limit type**: `None` (no limit), `Token bucket` (fixed request rate), or `Semaphore` (limit concurrent requests).
- **Rate limit requests**: Maximum number of requests per time period (token bucket only).
- **Rate limit period (sec)**: Length of the time window in seconds (token bucket only).
- **Max concurrent**: Maximum number of concurrent requests (semaphore only).
## Advanced settings
- **Intraday delay (sec)**: Minimum interval in seconds between two intraday queries for the same instrument (default: 900 = 15 minutes).
- **URL pattern (Regex)**: Optional regular expression to validate the URL extension for securities.
- **History gap filler**: Whether GT should automatically fill missing trading days in historical data with the previous day's value.
- **GBX divider**: Whether prices should automatically be divided by 100 — required for the London Stock Exchange, where prices are quoted in pence rather than pounds.
- **Supported categories**: Multi-select of instrument categories that the connector can deliver. The available entries are the same ones shown in the dropdown: Currency pair, Cryptocurrency, Non-investable indices, Equities, Fixed income, ETF, Mutual fund, Real estate fund, Issuer risk product, CFD derivative and Pension fund. When empty, all categories are assumed to be supported. For example, a cryptocurrency data provider would only tick *Cryptocurrency*, while a broad stock-exchange portal might tick *Equities*, *Fixed income*, *ETF* and *Mutual fund*. This is a preparatory field for future connector filtering and is not yet evaluated at runtime.
- **Geo inclusions**: Tree-select that lists the countries and stock exchanges configured in GT. Countries appear as parent nodes (shown with their localised name and flag — for example *Switzerland*, *Germany*, *United States*); each country can be expanded to reveal the stock exchanges below it (for example *SIX Swiss Exchange*, *Xetra*, *Frankfurt Stock Exchange*). Tick the entries that this connector covers. Selecting a country and selecting one of its exchanges are independent: ticking *Switzerland* does not also tick *SIX Swiss Exchange*, and vice versa. When empty, the connector is considered globally available. This is a preparatory field and is not yet evaluated at runtime.
- **Geo exclusions**: Tree-select used to mark instrument categories that this connector cannot deliver for a particular geo. The tree shows only the countries and exchanges that are currently ticked in **Geo inclusions** (it is rebuilt automatically whenever the inclusion selection changes). Expand any of those entries and tick the categories — *Equities*, *Fixed income*, *ETF*, *Mutual fund*, etc., the same labels seen in **Supported categories** — that should be excluded for that geo. Leave the tree empty when no exclusions apply.

  **Examples:**
  - A connector that only delivers US data: in **Geo inclusions**, tick *United States*.
  - A connector for the German stock exchanges Xetra and Frankfurt: in **Geo inclusions**, expand *Germany* and tick *Xetra* and *Frankfurt Stock Exchange* (leave the *Germany* node itself unchecked).
  - A connector that covers Switzerland in general but cannot deliver bonds from SIX: in **Geo inclusions**, tick *Switzerland* and also tick *SIX Swiss Exchange*; then in **Geo exclusions**, expand the *SIX Swiss Exchange* node and tick *Fixed income*.
- **Token configuration (YAML)**: Optional YAML block for automatic token acquisition (SAML/SSO flows). See [Token configuration (YAML)](../tokenconfiguration/) for details. When empty, the connector uses classic API key mode.
