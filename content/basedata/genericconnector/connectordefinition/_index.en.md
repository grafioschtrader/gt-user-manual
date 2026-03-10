---
title: "Connector definition"
date: 2026-02-26T12:00:00+01:00
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
- **Token configuration (YAML)**: Optional YAML block for automatic token acquisition (SAML/SSO flows). See [Token configuration (YAML)](../tokenconfiguration/) for details. When empty, the connector uses classic API key mode.
