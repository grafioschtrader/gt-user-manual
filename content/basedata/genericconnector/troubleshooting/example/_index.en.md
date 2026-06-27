---
title: "Connector Examples"
date: 2026-02-27T12:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
These examples show complete generic connector configurations for four data providers. The first three demonstrate different JSON data structures supported by GT. The fourth — Pacific Exchange Rate Service — is a CSV-based currency pair example that uses date component and currency placeholders. The example instrument is noted for each connector.

## Gettex

Gettex (Munich Stock Exchange) provides delayed price data via the LSEG widget API. Authentication is automatic via a SAML token flow — no manual API key entry is needed. Example instrument: Erste Group Bank (RIC `EBKG.GTX`).

### Connector definition

- **Short ID**: `gettex`
- **Domain URL**: `https://lseg-widgets.financial.com/`
- **API key**: Auto-acquired (token config YAML handles SAML login)
- **URL pattern regex**: `^[A-Z0-9]+\.GTX$`
- **Intraday delay**: 900 seconds
- **Rate limit**: Token bucket, 5 requests / 1 second
- **HTTP header**: `jwt: {apiKey}` (JWT token substituted automatically)

### Historical endpoint

- **Data structure**: Array of objects
- **URL template**: `rest/api/timeseries/historical?ric={ticker}&fids=_DATE_END,OPEN_PRC,HIGH_1,LOW_1,CLOSE_PRC,ACVOL_1&samples=D&appendRecentData=all&fromDate={fromDate}&toDate={toDate}`
- **Data path**: `data`
- **Status path / OK value**: `status` / `OK`
- **Date format**: ISO_DATE_TIME
- **Field mappings**: date → `_DATE_END`, open → `OPEN_PRC`, high → `HIGH_1`, low → `LOW_1`, close → `CLOSE_PRC`, volume → `ACVOL_1`

```json
{
  "data": [
    {"_DATE_END": "2026-02-03", "OPEN_PRC": "71.4", "HIGH_1": "71.4",
     "LOW_1": "69.8", "CLOSE_PRC": "71.4", "ACVOL_1": "8"},
    {"_DATE_END": "2026-02-04", "OPEN_PRC": "71.4", "HIGH_1": "71.4",
     "LOW_1": "70.0", "CLOSE_PRC": "71.4", "ACVOL_1": "2"},
    {"_DATE_END": "2026-02-05", "OPEN_PRC": "71.4", "HIGH_1": "71.4",
     "LOW_1": "70.0", "CLOSE_PRC": "71.4", "ACVOL_1": "12"}
  ],
  "status": "OK"
}
```

### Intraday endpoint

The intraday endpoint reuses the same URL template as the historical endpoint but with today's date as both `{fromDate}` and `{toDate}`. It uses the same data structure and field mappings except the target fields differ.

- **Data structure**: Array of objects
- **Data path**: `data`
- **Status path / OK value**: `status` / `OK`
- **Field mappings**: last → `CLOSE_PRC`, open → `OPEN_PRC`, high → `HIGH_1`, low → `LOW_1`, volume → `ACVOL_1`

{{% notice info %}}Gettex only returns data during trading hours. Outside hours the `data` array is empty — this is normal behavior, not an error.{{% /notice %}}

```json
{"data": [], "status": "OK", "metadata": {}}
```

### URL extension

The URL extension is the instrument's RIC code, e.g. `EBKG.GTX`. It replaces the `{ticker}` placeholder in the URL template.

---

## OTC-X

OTC-X (Berner Kantonalbank) provides prices for Swiss OTC-traded securities. No API key is required. Example instrument: Rigi Bahnen AG (ISIN `CH0016290014`).

### Connector definition

- **Short ID**: `otcx`
- **Domain URL**: `https://www.otc-x.ch/api/`
- **API key**: Not required
- **URL pattern regex**: `^[A-Z]{2}[A-Z0-9]{9}[0-9]$`

### Historical endpoint

- **Data structure**: Array of objects
- **URL template**: `market/securities/{ticker}/chartData?type=PRICE_HISTORY&from={fromDate}&until={toDate}&interval=DAY`
- **Data path**: `data`
- **Date format**: ISO_DATE
- **Processing options**: Skip weekend data, Remove duplicate dates
- **Field mappings**: date → `x`, close → `values.0`

Only the close price is available. The mapping `values.0` reads the first element of the `values` array in each data object.

```json
{
  "data": [
    {"x": "2026-02-20", "values": [13.25], "metadata": []},
    {"x": "2026-02-23", "values": [13.25], "metadata": []},
    {"x": "2026-02-24", "values": [13.30], "metadata": []}
  ],
  "from": "2026-02-20", "until": "2026-02-28",
  "type": "PRICE_HISTORY"
}
```

### Intraday endpoint

- **Data structure**: Single object (no data path needed)
- **URL template**: `market/securities/{ticker}`
- **Date format**: ISO_DATE_TIME
- **Field mappings**: last → `lastPrice`, low → `bidPricePoint`, high → `askPricePoint`

The bid and ask prices are mapped to low and high because OTC securities have no intraday OHLC data.

```json
{
  "isin": "CH0016290014",
  "name": "Rigi Bahnen AG, Goldau",
  "askPricePoint": 14.0,
  "bidPricePoint": 13.1,
  "lastPrice": 13.15,
  "lastDate": "2026-02-27T14:25:15.427Z",
  "performanceYtd": -0.0259,
  "tradeable": true
}
```

### URL extension

The URL extension is the instrument's ISIN, e.g. `CH0016290014`. It replaces `{ticker}` in the URL template. The regex validates the standard 12-character ISIN format.

---

## SIX Swiss Exchange

SIX Swiss Exchange provides free delayed (15-minute) price data for Swiss securities. No API key is required. Example instrument: SPI index (ISIN `CH0237935652`).

### Connector definition

- **Short ID**: `sixgroup`
- **Domain URL**: `https://www.six-group.com/`
- **API key**: Not required
- **URL pattern regex**: `^([A-Z]{2})([A-Z0-9]{9})([0-9]{1})[A-Za-z]{3}\d$`
- **Intraday delay**: 900 seconds

### Historical endpoint

- **Data structure**: Parallel arrays
- **URL template**: `itf/fqs/delayed/charts.json?netting=1440&columns=Date,Time,Close,Open,Low,High,TotalVolume&fromdate={fromDate}&where=ValorId={ticker}&select=ISIN,ClosingPrice,ClosingPerformance,PreviousClosingPrice,LatestTradeTime,LatestTradeDate`
- **Data path**: `valors.0.data`
- **Date format**: Pattern `yyyyMMdd`
- **Field mappings**: date → `Date`, open → `Open`, high → `High`, low → `Low`, close → `Close`, volume → `TotalVolume`

Each field is a separate array; all arrays share the same length and are correlated by index.

```json
{
  "valors": [{
    "ISIN": "CH0237935652",
    "data": {
      "Date": [20260223, 20260224, 20260225],
      "Close": [166.12, 167.4, 167.68],
      "Open": [166.52, 166.16, 167.88],
      "Low": [165.82, 165.84, 167.3],
      "High": [166.58, 168.16, 168.12],
      "TotalVolume": [58279, 258532, 178954]
    }
  }]
}
```

### Intraday endpoint

- **Data structure**: Column names with row data
- **URL template**: `itf/fqs/delayed/movie.json?where=ValorId={ticker}&select=ValorSymbol,MarketDate,Currency,ClosingPrice,ClosingPerformance,WeekAgoPerformance,MonthAgoPerformance,YearAgoPerformance,YearToDatePerformance,LatestTradeVolume,TotalVolume,PreviousClosingPrice,BidPrice,AskPrice,BidVolume,AskVolume,DailyLowPrice,DailyLowTime,DailyHighPrice,DailyHighTime,YearlyLowPrice,YearlyLowDate,YearlyHighPrice,YearlyHighDate`
- **Column names path**: `colNames`
- **Data path**: `rowData`
- **Field mappings**: last → `ClosingPrice`, changePercentage → `ClosingPerformance`, prevClose → `PreviousClosingPrice`, low → `DailyLowPrice`, high → `DailyHighPrice`

Column headers are in `colNames`, values in positional `rowData` arrays. The mapping `ClosingPrice` resolves to the column index, and the parser reads that position from the row.

```json
{
  "colNames": ["ValorSymbol", "MarketDate", "Currency",
    "ClosingPrice", "ClosingPerformance", "PreviousClosingPrice",
    "DailyLowPrice", "DailyHighPrice"],
  "rowData": [
    ["CHSPI", 20260227, "CHF", 167.7, 0.48, 166.9, 167.12, 168.42]
  ]
}
```

### URL extension

SIX identifies instruments by `{ISIN}{currency}{digit}`, e.g. `CH0237935652CHF4`. The regex in the connector definition validates this format.

---

## Pacific Exchange Rate Service

The Pacific Exchange Rate Service (University of British Columbia) provides free historical exchange rates for currency pairs. No API key is required. This is the first CSV-based and currency pair example. Example currency pair: USD/CHF.

### Connector definition

- **Short ID**: `fxubc_gen`
- **Domain URL**: `https://fx.sauder.ubc.ca/`
- **API key**: Not required

### Historical endpoint

- **Feed / Instrument type**: EOD History — Currency pair
- **Response format**: CSV
- **URL template**: `cgi/fxdata?b={toCurrency}&c={fromCurrency}&rd=&fd={fromDay}&fm={fromMonth}&fy={fromYear}&ld={toDay}&lm={toMonth}&ly={toYear}&y=daily&q=price&f=csv&o=`
- **CSV delimiter**: `,`
- **Skip header lines**: 2
- **Number format**: US
- **Date format type**: PATTERN
- **Date format pattern**: `yyyy/MM/dd`
- **Max data points**: 1040
- **Pagination enabled**: Yes
- **Processing options**: Skip weekend data
- **Field mappings**: date → `1`, close → `3`

The URL uses `{fromCurrency}` and `{toCurrency}` placeholders directly instead of `{ticker}`, and date component placeholders (`{fromDay}`, `{fromMonth}`, `{fromYear}`, `{toDay}`, `{toMonth}`, `{toYear}`) instead of `{fromDate}`/`{toDate}`. The server limits responses to approximately 4 years of data, so pagination with `max_data_points = 1040` handles longer date ranges automatically.

The first line of the response is a title, the second is the column header — both are skipped via **Skip header lines = 2**. The trailing copyright line is ignored because it does not start with a digit. CSV values are quoted, but the parser handles this automatically.

```
"PACIFIC Exchange Rate Service"
"Jul.Day","YYYY/MM/DD","Wdy","USD/CHF"
2461091,"2026/02/19","Thu",1.2896
2461092,"2026/02/20","Fri",1.2896
2461095,"2026/02/23","Mon",1.2915
2461096,"2026/02/24","Tue",1.2922
2461097,"2026/02/25","Wed",1.2934
2461098,"2026/02/26","Thu",1.2916
2461099,"2026/02/27","Fri",1.3008
"(C) 2026 Prof. Werner Antweiler, UBC"
```

Field mapping uses CSV column indices (0-based): column 1 is the date (`YYYY/MM/DD`), column 3 is the exchange rate.

### Currency pair mapping

This connector does not use a URL extension or `{ticker}`. Instead, the currency codes are taken directly from the currency pair entity and substituted into `{fromCurrency}` and `{toCurrency}` in the URL template.

---

## Testing

After saving a connector, verify the endpoints using the test dialog. See the [parent page on testing]({{< relref "../" >}}) for details.
