---
title: "Connector-Beispiele"
date: 2026-02-27T12:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
Diese Beispiele zeigen vollständige Konfigurationen generischer Connectors für vier Datenanbieter. Die ersten drei demonstrieren verschiedene von GT unterstützte JSON-Datenstrukturen. Der vierte — Pacific Exchange Rate Service — ist ein CSV-basiertes Währungspaar-Beispiel, das Datumskomponenten- und Währungs-Platzhalter verwendet. Das Beispielinstrument ist jeweils angegeben.

## Gettex

Gettex (Börse München) liefert verzögerte Kursdaten über die LSEG-Widget-API. Die Authentifizierung erfolgt automatisch über einen SAML-Token-Ablauf — eine manuelle API-Schlüssel-Eingabe ist nicht nötig. Beispielinstrument: Erste Group Bank (RIC `EBKG.GTX`).

### Connector-Definition

- **Kurz-ID**: `gettex`
- **Domänen-URL**: `https://lseg-widgets.financial.com/`
- **API-Schlüssel**: Automatisch bezogen (Token-Konfiguration YAML steuert SAML-Login)
- **URL-Muster-Regex**: `^[A-Z0-9]+\.GTX$`
- **Intraday-Verzögerung**: 900 Sekunden
- **Rate-Limit**: Token-Bucket, 5 Anfragen / 1 Sekunde
- **HTTP-Header**: `jwt: {apiKey}` (JWT-Token wird automatisch eingesetzt)

### Historischer Endpunkt

- **Datenstruktur**: Array von Objekten
- **URL-Vorlage**: `rest/api/timeseries/historical?ric={ticker}&fids=_DATE_END,OPEN_PRC,HIGH_1,LOW_1,CLOSE_PRC,ACVOL_1&samples=D&appendRecentData=all&fromDate={fromDate}&toDate={toDate}`
- **Datenpfad**: `data`
- **Statuspfad / OK-Wert**: `status` / `OK`
- **Datumsformat**: ISO_DATE_TIME
- **Feldzuordnungen**: date → `_DATE_END`, open → `OPEN_PRC`, high → `HIGH_1`, low → `LOW_1`, close → `CLOSE_PRC`, volume → `ACVOL_1`

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

### Intraday-Endpunkt

Der Intraday-Endpunkt verwendet dieselbe URL-Vorlage wie der historische Endpunkt, jedoch mit dem heutigen Datum als `{fromDate}` und `{toDate}`. Die Datenstruktur und Feldzuordnungen sind identisch, nur die Zielfelder unterscheiden sich.

- **Datenstruktur**: Array von Objekten
- **Datenpfad**: `data`
- **Statuspfad / OK-Wert**: `status` / `OK`
- **Feldzuordnungen**: last → `CLOSE_PRC`, open → `OPEN_PRC`, high → `HIGH_1`, low → `LOW_1`, volume → `ACVOL_1`

{{% notice info %}}Gettex liefert nur während der Handelszeiten Daten. Ausserhalb der Handelszeiten ist das `data`-Array leer — dies ist normales Verhalten, kein Fehler.{{% /notice %}}

```json
{"data": [], "status": "OK", "metadata": {}}
```

### URL-Erweiterung

Die URL-Erweiterung ist der RIC-Code des Instruments, z.B. `EBKG.GTX`. Er ersetzt den `{ticker}`-Platzhalter in der URL-Vorlage.

---

## OTC-X

OTC-X (Berner Kantonalbank) liefert Kurse für Schweizer OTC-gehandelte Wertpapiere. Es ist kein API-Schlüssel erforderlich. Beispielinstrument: Rigi Bahnen AG (ISIN `CH0016290014`).

### Connector-Definition

- **Kurz-ID**: `otcx`
- **Domänen-URL**: `https://www.otc-x.ch/api/`
- **API-Schlüssel**: Nicht erforderlich
- **URL-Muster-Regex**: `^[A-Z]{2}[A-Z0-9]{9}[0-9]$`

### Historischer Endpunkt

- **Datenstruktur**: Array von Objekten
- **URL-Vorlage**: `market/securities/{ticker}/chartData?type=PRICE_HISTORY&from={fromDate}&until={toDate}&interval=DAY`
- **Datenpfad**: `data`
- **Datumsformat**: ISO_DATE
- **Verarbeitungsoptionen**: Wochenenddaten überspringen, Doppelte Daten entfernen
- **Feldzuordnungen**: date → `x`, close → `values.0`

Es ist nur der Schlusskurs verfügbar. Die Zuordnung `values.0` liest das erste Element des `values`-Arrays in jedem Datenobjekt.

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

### Intraday-Endpunkt

- **Datenstruktur**: Einzelnes Objekt (kein Datenpfad nötig)
- **URL-Vorlage**: `market/securities/{ticker}`
- **Datumsformat**: ISO_DATE_TIME
- **Feldzuordnungen**: last → `lastPrice`, low → `bidPricePoint`, high → `askPricePoint`

Die Geld- und Briefkurse werden auf low und high abgebildet, da OTC-Wertpapiere keine Intraday-OHLC-Daten liefern.

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

### URL-Erweiterung

Die URL-Erweiterung ist die ISIN des Instruments, z.B. `CH0016290014`. Sie ersetzt `{ticker}` in der URL-Vorlage. Der reguläre Ausdruck validiert das standardmässige 12-stellige ISIN-Format.

---

## SIX Swiss Exchange

SIX Swiss Exchange bietet kostenlose verzögerte (15 Minuten) Kursdaten für Schweizer Wertpapiere an. Es ist kein API-Schlüssel erforderlich. Beispielinstrument: SPI-Index (ISIN `CH0237935652`).

### Connector-Definition

- **Kurz-ID**: `sixgroup`
- **Domänen-URL**: `https://www.six-group.com/`
- **API-Schlüssel**: Nicht erforderlich
- **URL-Muster-Regex**: `^([A-Z]{2})([A-Z0-9]{9})([0-9]{1})[A-Za-z]{3}\d$`
- **Intraday-Verzögerung**: 900 Sekunden

### Historischer Endpunkt

- **Datenstruktur**: Parallele Arrays
- **URL-Vorlage**: `itf/fqs/delayed/charts.json?netting=1440&columns=Date,Time,Close,Open,Low,High,TotalVolume&fromdate={fromDate}&where=ValorId={ticker}&select=ISIN,ClosingPrice,ClosingPerformance,PreviousClosingPrice,LatestTradeTime,LatestTradeDate`
- **Datenpfad**: `valors.0.data`
- **Datumsformat**: Muster `yyyyMMdd`
- **Feldzuordnungen**: date → `Date`, open → `Open`, high → `High`, low → `Low`, close → `Close`, volume → `TotalVolume`

Jedes Feld ist ein separates Array; alle Arrays haben dieselbe Länge und sind über den Index korreliert.

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

### Intraday-Endpunkt

- **Datenstruktur**: Spaltennamen mit Zeilendaten
- **URL-Vorlage**: `itf/fqs/delayed/movie.json?where=ValorId={ticker}&select=ValorSymbol,MarketDate,Currency,ClosingPrice,ClosingPerformance,WeekAgoPerformance,MonthAgoPerformance,YearAgoPerformance,YearToDatePerformance,LatestTradeVolume,TotalVolume,PreviousClosingPrice,BidPrice,AskPrice,BidVolume,AskVolume,DailyLowPrice,DailyLowTime,DailyHighPrice,DailyHighTime,YearlyLowPrice,YearlyLowDate,YearlyHighPrice,YearlyHighDate`
- **Spaltennamenpfad**: `colNames`
- **Datenpfad**: `rowData`
- **Feldzuordnungen**: last → `ClosingPrice`, changePercentage → `ClosingPerformance`, prevClose → `PreviousClosingPrice`, low → `DailyLowPrice`, high → `DailyHighPrice`

Spaltenüberschriften stehen in `colNames`, Werte in positionalen `rowData`-Arrays. Die Zuordnung `ClosingPrice` wird auf den Spaltenindex aufgelöst, und der Parser liest diese Position aus der Zeile.

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

### URL-Erweiterung

SIX identifiziert Instrumente anhand von `{ISIN}{Währung}{Ziffer}`, z.B. `CH0237935652CHF4`. Der reguläre Ausdruck in der Connector-Definition validiert dieses Format.

---

## Pacific Exchange Rate Service

Der Pacific Exchange Rate Service (University of British Columbia) bietet kostenlose historische Wechselkurse für Währungspaare an. Es ist kein API-Schlüssel erforderlich. Dies ist das erste CSV-basierte Währungspaar-Beispiel. Beispiel-Währungspaar: USD/CHF.

### Connector-Definition

- **Kurz-ID**: `fxubc_gen`
- **Domänen-URL**: `https://fx.sauder.ubc.ca/`
- **API-Schlüssel**: Nicht erforderlich

### Historischer Endpunkt

- **Feed / Instrumenttyp**: EOD-Verlauf — Währungspaar
- **Antwortformat**: CSV
- **URL-Vorlage**: `cgi/fxdata?b={toCurrency}&c={fromCurrency}&rd=&fd={fromDay}&fm={fromMonth}&fy={fromYear}&ld={toDay}&lm={toMonth}&ly={toYear}&y=daily&q=price&f=csv&o=`
- **CSV-Trennzeichen**: `,`
- **Kopfzeilen überspringen**: 2
- **Zahlenformat**: US
- **Datumsformat-Typ**: PATTERN
- **Datumsformat-Muster**: `yyyy/MM/dd`
- **Max. Datenpunkte**: 1040
- **Paginierung aktiviert**: Ja
- **Verarbeitungsoptionen**: Wochenenddaten überspringen
- **Feldzuordnungen**: date → `1`, close → `3`

Die URL verwendet `{fromCurrency}`- und `{toCurrency}`-Platzhalter direkt anstelle von `{ticker}`, sowie Datumskomponenten-Platzhalter (`{fromDay}`, `{fromMonth}`, `{fromYear}`, `{toDay}`, `{toMonth}`, `{toYear}`) anstelle von `{fromDate}`/`{toDate}`. Der Server begrenzt Antworten auf ungefähr 4 Jahre Daten, daher werden längere Zeiträume automatisch über Paginierung mit `max_data_points = 1040` abgedeckt.

Die erste Zeile der Antwort ist ein Titel, die zweite die Spaltenüberschrift — beide werden über **Kopfzeilen überspringen = 2** übersprungen. Die abschliessende Copyright-Zeile wird ignoriert, da sie nicht mit einer Ziffer beginnt. CSV-Werte sind in Anführungszeichen eingeschlossen, der Parser behandelt dies automatisch.

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

Die Feldzuordnung verwendet CSV-Spaltenindizes (0-basiert): Spalte 1 ist das Datum (`YYYY/MM/DD`), Spalte 3 ist der Wechselkurs.

### Währungspaar-Zuordnung

Dieser Connector verwendet keine URL-Erweiterung oder `{ticker}`. Stattdessen werden die Währungscodes direkt aus dem Währungspaar-Objekt entnommen und in `{fromCurrency}` und `{toCurrency}` in der URL-Vorlage eingesetzt.

---

## Testen

Nach dem Speichern eines Connectors können Sie die Endpunkte über den Testdialog überprüfen. Weitere Informationen finden Sie auf der [übergeordneten Seite zum Testen]({{< relref "../" >}}).
