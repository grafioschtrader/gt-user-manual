---
title: "Endpoints"
date: 2026-02-27T12:00:00+01:00
draft: false
weight: 20
archetype: "default"
---
Each endpoint defines how a specific type of price data is fetched from the provider and interpreted. Up to four endpoints can be configured per connector, where each combination of feed type and instrument type may only be used once:
- **EOD History — Security**: Historical end-of-day quotes for securities
- **EOD History — Currency pair**: Historical end-of-day quotes for currency pairs
- **Intraday — Security**: Current prices for securities
- **Intraday — Currency pair**: Current prices for currency pairs
## Endpoint settings
- **Feed / Instrument type**: Combined selection of feed type and instrument type. Already used combinations are automatically hidden. When editing an existing endpoint, this field is locked.
- **URL template**: The URL for fetching data. Placeholders are automatically replaced (see below).
- **HTTP method**: HTTP method for the request (default: `GET`).
- **Response format**: `JSON`, `CSV`, or `HTML` — determines how the API response is parsed.
- **Number format**: Format for decimal numbers: `US` (period), `GERMAN` (comma), `SWISS` (apostrophe), or `PLAIN` (no thousands separator).
- **Date format type**: How dates are encoded in the response: `Unix seconds`, `Unix milliseconds`, `ISO_DATE`, `ISO_DATE_TIME`, or `PATTERN` (custom).
- **Date format pattern**: Custom date format pattern (only for type `PATTERN`), e.g. `yyyy-MM-dd`.
- **Endpoint options**: Optional processing flags for the endpoint (see [Endpoint options](#endpoint-options) below).
## URL template placeholders
- **`{ticker}`**: Ticker symbol or URL extension of the instrument
- **`{fromDate}`**: Start date (formatted according to the date format setting)
- **`{toDate}`**: End date (formatted according to the date format setting)
- **`{fromUnix}`**: Start date as Unix timestamp (seconds)
- **`{toUnix}`**: End date as Unix timestamp (seconds)
- **`{fromUnixMs}`**: Start date as Unix timestamp (milliseconds)
- **`{toUnixMs}`**: End date as Unix timestamp (milliseconds)
- **`{fromDay}`**: Day of the start date (1–31)
- **`{fromMonth}`**: Month of the start date (1–12)
- **`{fromYear}`**: Year of the start date (e.g. 2026)
- **`{toDay}`**: Day of the end date (1–31)
- **`{toMonth}`**: Month of the end date (1–12)
- **`{toYear}`**: Year of the end date (e.g. 2026)
- **`{fromCurrency}`**: ISO currency code of the base currency (e.g. `USD`)
- **`{toCurrency}`**: ISO currency code of the target currency (e.g. `CHF`)
- **`{limit}`**: Maximum number of data points per request (from the **Max data points** endpoint setting)
- **`{apiKey}`**: API key (from the connector API key table)
**Example** URL template for a JSON API provider:
```
https://api.example.com/v1/history?symbol={ticker}&from={fromDate}&to={toDate}&apikey={apiKey}
```
## Format-specific settings
Depending on the chosen **response format**, different settings become relevant:
### JSON settings
- **Data structure**: `Array of objects` (most common format), `Parallel arrays`, or `Single object` (for intraday).
- **Data path**: Path to the data array in the JSON response (e.g. `results` or `data.quotes`).
- **Status path**: Optional path to a status field for error detection.
- **Status OK value**: Expected value in the status field for a successful response.
### CSV settings
- **CSV delimiter**: Separator between columns (default: comma).
- **Skip header lines**: Number of header lines to skip when parsing (default: 1).
### HTML settings
- **CSS selector**: JSoup CSS selector for the relevant HTML element.
- **Extract mode**: `Regex groups` (extract values using regular expressions), `Split positions` (split text by delimiter), or `Multi selector` (separate CSS selector per field).
- **Text cleanup**: Optional regular expression for cleaning extracted text.
- **Extract regex**: Regular expression with capture groups (only for mode Regex groups).
- **Split delimiter**: Delimiter for splitting text (only for mode Split positions).
## Ticker settings
Some data providers use ticker formats that differ from the GT standard. The ticker settings allow customization.
- **Ticker build strategy**: `URL_EXTEND` (the URL extension field of the security is used) or `CURRENCY_PAIR` (currency pair is composed from base and target currency).
- **Currency pair separator**: Optional character between the two currency codes (e.g. `/` for `EUR/USD`).
- **Currency pair suffix**: Optional suffix after the currency pair (e.g. `=X`).
- **Ticker uppercase**: Whether the ticker should be converted to uppercase.
## Pagination settings
- **Max data points**: Maximum number of data points per query.
- **Pagination enabled**: Whether pagination is enabled for large data sets.
## Endpoint options
Endpoint options are bitmask flags that enable special processing for specific feed types.
- **`SKIP_WEEKEND_DATA`** (Historical endpoints): Filters out data points that fall on Saturday or Sunday. Useful when a data provider incorrectly delivers weekend rows.
- **`REMOVE_DUPLICATE_DATES`** (Historical endpoints): Removes duplicate date entries from the response. Only the first entry per date is kept. Useful when a data provider returns overlapping data across pagination batches or duplicate rows.
