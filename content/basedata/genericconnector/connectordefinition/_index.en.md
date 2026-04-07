---
title: "Connector definition"
date: 2026-03-13T12:00:00+01:00
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
- **Supported categories**: Multi-select of instrument categories that the connector can deliver. The available categories are: Currency pair, Cryptocurrency, Non-investable indices, Equities, Fixed income, ETF, Mutual fund, Real estate fund, and Issuer risk product. When empty, all categories are assumed to be supported. For example, a cryptocurrency data provider would only select *Cryptocurrency*, while a broad stock exchange portal might select *Equities*, *Fixed income*, *ETF* and *Mutual fund*. This is a preparatory field for future connector filtering and is not yet evaluated at runtime.
- **Geo restrictions**: Defines which countries or stock exchanges a connector covers. This allows modelling geographic scope — for instance, a connector that only delivers data for US exchanges, or one that covers all of Switzerland except bonds on a particular exchange. When empty, the connector is considered globally available. This is a preparatory field and is not yet evaluated at runtime.

  In the edit dialog, this field is split into two tree-select controls:
  - **Geo inclusions**: Select countries (2-letter ISO code, e.g. `US`, `CH`, `DE`) or individual stock exchanges by their MIC code (e.g. `XETR` for Xetra, `XSWX` for SIX Swiss Exchange, `XWAR` for Warsaw). The tree is structured as Country → Stock Exchange, so you can pick an entire country or just specific exchanges within it.
  - **Geo exclusions**: Optionally exclude specific instrument categories from an already included country or exchange. Each exclusion combines a geographic code with a category in the format `GEO:-CATEGORY` — for example, `XSWX:-FIXED_INCOME` means the connector covers SIX Swiss Exchange but *not* for fixed-income instruments. In the tree-select, the parent nodes represent exchanges and the child nodes represent the categories to exclude.

  **Examples:**
  - A connector for US data only: **Geo inclusions** = `US`
  - A connector for Xetra and Frankfurt: **Geo inclusions** = `XETR XFRA`
  - A connector that covers Switzerland but cannot deliver bonds from SIX: **Geo inclusions** = `CH`, **Geo exclusions** = `XSWX:-FIXED_INCOME`
- **Token configuration (YAML)**: Optional YAML block for automatic token acquisition (SAML/SSO flows). See [Token configuration (YAML)](../tokenconfiguration/) for details. When empty, the connector uses classic API key mode.
