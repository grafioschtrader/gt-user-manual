---
title: "Handelsplattform Plan"
date: 2026-09-24T12:00:00+02:00
draft: false
weight: 15
archetype: "default"
---
Ein **Depot** hat einen **Handelsplattform Plan** aus folgenden Gründen:
+ Er steuert die korrekte Zuordnung der **Importvorlagen** zu einem Depot.
+ Es kann ein **Gebührenmodell** definiert werden, um Transaktionskosten für hypothetische Trades abzuschätzen. So kann GT bei der Simulation der **hypothetischen Glattstellung** von Positionen **realistische Ergebnisse** liefern.
+ Er liefert das **Händlerland** für die Steuerschätzung der [historischen Wiederholung]({{% ref "/algoalert/historicalrun" %}}).

Im folgenden Entity-Relationship-Diagramm sind die Beziehungen dargestellt:

{{< mermaid >}}
erDiagram
    Depot }o--|| Handelsplattform-Plan : hat
    Handelsplattform-Plan }o--o| Vorlagengruppe : hat
    Vorlagengruppe ||--o{ Importvorlage :  hat
{{< /mermaid >}}

## Händlerland
Das **Händlerland** ist das Land des Effektenhändlers, über den die Depots dieses Plans handeln. Es ist nicht der Wohnsitz des Anlegers: Ein Anleger in der Schweiz, der bei einem ausländischen Broker handelt, wählt das Land des Brokers. Das Feld erscheint nur, wenn auf dieser GT-Instanz der regelbasierte Handel eingeschaltet ist. Es wirkt ausschliesslich in einer historischen Wiederholung, bei der **Simulationssteuermodelle anwenden** eingeschaltet ist; echte Transaktionen bleiben unberührt.

In der Realität fällt die Schweizer Umsatzabgabe (Stempelsteuer) nur an, wenn ein Schweizer Effektenhändler am Geschäft beteiligt ist. Das mitgelieferte [Simulationssteuermodell]({{% ref "/algoalert/historicalrun/taxmodel" %}}) der Schweiz bildet dies ab: Ist das Händlerland die Schweiz, belastet die Wiederholung bei jedem Kauf und Verkauf den Kundenanteil der Abgabe, nämlich 0,075 % für schweizerische und 0,15 % für ausländische Wertpapiere. Bei einem ausländischen Händler entfällt die Abgabe. Die Quellensteuern auf Dividenden und Zinsen hängen dagegen vom Emittentenland ab und nicht vom Händler; ein ausländischer Broker ändert daran nichts.

Bei einem Schweizer Händler muss im Depot auch **Steuerbefreiter Anleger** auf nein oder ja stehen, sonst kann das Modell die Abgabe nicht berechnen. Bleibt das Händlerland leer, schätzt die Wiederholung keine Stempelsteuer und kennzeichnet jedes Geschäft als unvollständig geschätzt. Der Wohnsitz des Anlegers fliesst nicht in die Schätzung ein. Deshalb fehlt die Befreiung ausländischer Obligationen, die für eine im Ausland ansässige Vertragspartei gilt.

## Gebührenmodell
Das Gebührenmodell ermöglicht es, **Transaktionskostenregeln** als YAML auf einem Handelsplattform Plan zu definieren. Jede Regel besteht aus einer **Bedingung** und einem **Gebührenausdruck**, beide in **EvalEx-Ausdrücken** formuliert. Das Gebührenmodell wird in einem **eigenen Dialog** bearbeitet, der über das Kontextmenü der Handelsplattform-Plan-Tabelle geöffnet wird: Rechtsklick auf einen Handelsplattform Plan → **Gebührenmodell bearbeiten...**. Der Dialog enthält einen **Monaco-Editor** mit schemabasierter Autovervollständigung und Hover-Dokumentation für das YAML.

Die gemeinsame Bedienung beschreibt [YAML bearbeiten und prüfen]({{% ref "/intro/userinterface#yaml-editor" %}}). **Prüfen** kontrolliert das Dokument und seine Ausdrücke, ohne es zu speichern. Bei Depotgebühren passen die Vorschläge zum jeweiligen Berechnungsfeld: Für einen Gebührenbetrag stehen andere Variablen zur Verfügung als für die Bewertung einzelner Positionen oder die Auswahl anrechenbarer Geschäfte.

{{% notice style="info" title="Depotspezifische Überschreibung" %}}
Einzelne [Depots]({{% ref "/tenantportfolio/securityaccounts" %}}) können ein eigenes Gebührenmodell-YAML definieren, das dieses Plan-Modell überschreibt. Der gleiche Gebührenmodell-Editor kann auch über das Kontextmenü des Depots im Navigationsbaum geöffnet werden → **Gebührenmodell bearbeiten...**. Wenn ein Depot ein eigenes Gebührenmodell hat, hat dieses Vorrang vor dem Modell dieses Handelsplattform Plans. Courtagen und Depotgebühren werden gemeinsam ersetzt, die [Aufschläge bei der Währungsumrechnung](#aufschläge-bei-der-währungsumrechnung) werden dagegen getrennt geerbt.
{{% /notice %}}

Die [historische Wiederholung]({{% ref "/algoalert/historicalrun/fees" %}}) verwendet die konfigurierten Courtagen, Depotgebührenmodelle und Aufschläge bei der Währungsumrechnung. Der Courtagen-Test unten bewertet jeweils ein einzelnes Geschäft.

Regeln werden **von oben nach unten** ausgewertet — die erste Regel, deren Bedingung `true` ergibt, bestimmt die Transaktionskosten. Verwenden Sie `"true"` als Bedingung für eine Auffangregel.

Es gibt zwei sich gegenseitig ausschliessende YAML-Formate auf der obersten Ebene:

### Flache Regeln (ohne Datumseinschränkung)
Verwenden Sie das `rules`-Array auf oberster Ebene, wenn sich das Gebührenmodell zeitlich nicht ändert. Beispiel für einen Schweizer Broker:

```yaml
rules:
  - name: "Schweizer ETF Pauschale"
    condition: 'instrument == "ETF" && mic == "XSWX"'
    expression: "9.0"
  - name: "US-Aktien"
    condition: 'assetclass == "EQUITIES" && currency == "USD"'
    expression: "MAX(15.0, tradeValue * 0.0025)"
  - name: "Standard"
    condition: "true"
    expression: "MAX(20.0, tradeValue * 0.003)"
```

### Zeitbasierte Perioden mit verschachtelten Regeln
Verwenden Sie das `periods`-Array auf oberster Ebene, wenn sich das Gebührenmodell im Laufe der Zeit ändert. Jede Periode hat ein **validFrom**-Datum, ein optionales **validTo**-Datum und ein eigenes `rules`-Array. Die erste Periode, die zum **Transaktionsdatum** passt, wird verwendet. Fehlt `validTo`, gilt die Periode unbefristet.

```yaml
periods:
  - validFrom: "2023-01-01"
    validTo: "2024-12-31"
    rules:
      - name: "Alte Pauschale"
        condition: "true"
        expression: "25.0"
  - validFrom: "2025-01-01"
    rules:
      - name: "Neue ETF-Gebühr"
        condition: 'instrument == "ETF"'
        expression: "5.0"
      - name: "Neue Standardgebühr"
        condition: "true"
        expression: "MAX(10.0, tradeValue * 0.002)"
```

{{% notice style="info" title="Gegenseitig ausschliessend" %}}
Ein Gebührenmodell-YAML muss entweder `rules` oder `periods` auf der obersten Ebene enthalten, nie beides. Ein Handelsplattform Plan braucht immer eines davon. Nur das eigene Dokument eines Depots darf allein aus einem Abschnitt `fx` bestehen; es behält dann die Courtagen und Depotgebühren des Plans.
{{% /notice %}}

### Freie Trades und Währungsumtausch
Manche Broker verzichten auf die Courtage für eine begrenzte Anzahl Trades, beispielsweise für den ersten Trade jedes Quartals oder für den ersten Auftrag je ETF und Monat. Regeln können sich deshalb auf die Anzahl **früherer Trades** desselben Depots beziehen: `tradesInMonth`, `tradesInQuarter` und `tradesInYear` zählen die früheren Käufe und Verkäufe im Kalendermonat, Kalenderquartal oder Kalenderjahr des Trades, `securityTradesInMonth` nur jene desselben Wertpapiers im Kalendermonat. Der zu bewertende Trade selbst wird nie mitgezählt. Gezählt wird je Depot; ein Freikontingent, das ein Broker über mehrere Depots derselben Bankbeziehung gewährt, lässt sich nicht abbilden.

Eine Gebühr für den Währungsumtausch fällt meist nur an, wenn der Trade über ein Geldkonto in einer anderen Währung abgewickelt wird. `settlementCurrency` enthält die Währung des abwickelnden Geldkontos und kann mit der Währung des Instruments `currency` verglichen werden.

```yaml
rules:
  - name: "Ein freier Trade pro Quartal bis 5000"
    condition: "tradesInQuarter == 0 && tradeValue <= 5000"
    expression: "0"
  - name: "Erster Auftrag je ETF und Monat ab 1000"
    condition: 'instrument == "ETF" && securityTradesInMonth == 0 && tradeValue >= 1000'
    expression: "0"
  - name: "Courtage und Umtauschgebühr"
    condition: "true"
    expression: "tradeValue * (0.005 + IF(settlementCurrency != currency, 0.0095, 0))"
```

Der Gebührenmodellvergleich und der historische Simulationslauf ermitteln diese Werte aus den gebuchten Transaktionen des Depots. In der Simulation zählen auch die Trades vor dem Simulationsbeginn im selben Jahr, und nur tatsächlich gebuchte Trades verbrauchen ein Freikontingent.

Eine Umtauschgebühr in einer Courtagenregel ist ein Betrag in der Währung des Instruments. Eine prozentuale Marge im Wechselkurs gehört dagegen in die getrennten [Aufschläge bei der Währungsumrechnung](#aufschläge-bei-der-währungsumrechnung). Bilden Sie dieselbe Belastung nicht an beiden Stellen ab: Belastet eine Courtagenregel den Umtausch bereits über `settlementCurrency`, ergänzen Sie für diese Umrechnungen einen ausdrücklichen Währungstarif von null.

### Depotgebühren und Courtagenguthaben
Ein Gebührenmodell kann auch wiederkehrende Depotgebühren beschreiben. Ergänzen Sie dazu den Abschnitt `custody` auf oberster Ebene neben den bestehenden Courtagenregeln `rules` oder `periods`; ersetzen Sie die Courtagenregeln nicht durch das folgende Beispiel. Depotgebührenperioden haben eigene Datumsangaben, weil ein Broker seinen Depotgebührentarif ändern kann, ohne die Courtagen zu ändern. Beide Datumsangaben gelten einschliesslich; lassen Sie `validTo` für eine unbefristete Periode weg. Überlappende Perioden werden abgewiesen.

Der historische Simulationslauf verwendet das Modell des jeweiligen Depots. Ein depotspezifisches Dokument mit Courtagenregeln ersetzt die Courtagen und Depotgebühren des Handelsplattform Plans gemeinsam. Kopieren Sie beide Abschnitte, wenn Sie eine Überschreibung anlegen. Der Abschnitt `fx` gehört nicht dazu: Ohne eigenen Abschnitt `fx` behält das Depot die Aufschläge bei der Währungsumrechnung des Plans. Das Speichern eines Modells erzeugt keine Belastungen auf einem realen Konto.

#### Geprüfte Bedingungen und ausdrückliche Annahmen
Verwenden Sie `VERIFIED` für belegte Vertragsbedingungen, `ASSUMED` für eine bewusst gewählte Simulationskonvention und `UNRESOLVED` für unvollständige Bedingungen. Geben Sie immer `source` an; angenommene und ungeklärte Perioden erfordern zusätzlich eine erklärende `note`. Eine ungeklärte Periode, eine fehlende historische Periode oder fehlende Bewertungsdaten stoppen die Simulation. Ein fehlender Abschnitt `custody` bedeutet **nicht modellierte Depotgebühren**, nicht bestätigte Gebührenfreiheit. Für ein gebührenfreies Depot verwenden Sie eine datierte, ausführbare Periode mit `amount: "0"` und ohne Minimum.

Die mitgelieferten Brokervorlagen unterscheiden die heute veröffentlichten Bedingungen von historischen Belegen. Die Befreiung von Depotgebühren für Saxo-Privatkunden gilt ab dem 1. Februar 2025 und darf nicht rückwirkend auf Strateo angewendet werden. Der mitgelieferte ausführbare Tarif von PostFinance beginnt am 1. Januar 2026. Ältere Belastungen allein belegen nicht alle früheren Vertragsbedingungen. Die Migros-Broschüre vom Juni 2025 sah eine jährliche Abrechnung vor; die aktuelle Broschüre sieht eine vierteljährliche Abrechnung mit monatlicher Berechnung vor. Der genaue Beobachtungstag muss noch bestätigt werden. Die Vorlagen für Swissquote und Cornèrtrader lassen ungeklärte Einzelheiten offen, statt stillschweigend Bewertungs- oder Abrechnungstage festzulegen. Bei Migros ist zu prüfen, welche Anlagen unter die Befreiung für Vorsorgefonds fallen.

#### Vorausbelastung mit verfallenden Courtagenguthaben
Dieses Beispiel setzt ausschliesslich berechtigte Online-Wertschriftengeschäfte voraus. Nur offline handelbare Instrumente müssen ausdrücklich ausgeschlossen werden, beispielsweise über die ISIN im Berechtigungsausdruck. Die vierteljährliche Belastung von CHF 18 bleibt eine Depotgebühr. Das Guthaben von CHF 18 reduziert spätere berechtigte Courtagen im selben Quartal und bezahlt nie Transaktionssteuern. Nicht genutzte Guthaben verfallen; sie sind kein Bargeld und erhöhen den Portfoliowert nicht. Abrechnungen von Kapitalmassnahmen verwenden diese Guthaben nicht.

```yaml
custody:
  periods:
    - validFrom: "2026-01-01"
      status: ASSUMED
      source: "https://www.postfinance.ch/content/dam/pfch/doc/480_499/482_22_en.pdf"
      note: "Dieses Depot verwendet nur berechtigte Online-Wertschriftengeschäfte."
      currency: CHF
      valuation: NONE
      billingMonths: 3
      collection: START
      billingDay: 0
      dayCount: ACTUAL_ACTUAL
      amount: "18"
      creditAmount: 18
      creditEligibility: 'instrument != "FOREX" && instrument != "CFD"'
```

#### Bewertung und Abrechnung sind getrennt
`valuation` wählt keine Bewertung (`NONE`), tägliche Schlusswerte (`DAILY`), eine monatliche Beobachtung (`MONTHLY`) oder eine Beobachtung am Ende der Abrechnungsperiode (`PERIOD_END`). Bei monatlicher Bewertung ist `observationDay` erforderlich: 0 bedeutet Monatsende, sonst ein Tag von 1 bis 31; ein kürzerer Monat verwendet seinen letzten Tag. Bewertet werden die Wertschriften dieses Depots, umgerechnet in die Gebührenwährung, ohne Bargeld. Es werden nur historische Kurse am oder vor dem Beobachtungstag verwendet.

`amount` ist bei `NONE` die Gebühr je Rechnung. Bei den anderen Arten ist es ein **jährlicher** Gebührenausdruck: Monatliche Beobachtungen tragen einen Zwölftel bei, Beobachtungen am Periodenende den entsprechenden Jahresanteil, und tägliche Beobachtungen verwenden `dayCount`. Unterstützt werden die tatsächlichen Tage geteilt durch 365, 360 oder die tatsächliche Anzahl Tage des Jahres. `assetValue` ist der gebührenpflichtige Wert, `accountValue` der Wert aller Wertschriften des Depots. Optionale `valueRules` berechnen, erste Übereinstimmung zuerst, den gebührenpflichtigen Wert jeder Position aus `positionValue`, Instrumenttyp, Anlageklasse, ISIN, Währung und Börse (`mic`). Damit lassen sich Befreiungen oder Rabatte abbilden.

`billingMonths` ist 1, 3, 6 oder 12; die Abrechnungsperioden folgen immer dem Kalenderjahr, eine halbjährliche Abrechnung erfolgt also Ende Juni und Ende Dezember. `collection` ist `START` oder `END`; eine Vorausbelastung erfordert `NONE`. `billingDay: 0` verwendet die Kalendergrenze. Ein anderer Tag wählt diesen Tag im ersten oder letzten Monat der Abrechnungsperiode. Dies ist eine ausdrückliche Konvention und muss mit den Bedingungen des Brokers übereinstimmen. Courtagenguthaben erfordern eine Vorausbelastung an der Kalendergrenze. Eine belastete Tarifänderung innerhalb einer Abrechnungsperiode muss mit einem ausdrücklichen Depotmodell und passenden Abrechnungsperioden abgebildet werden.

`minimum` und `maximum` sind Beträge **je Rechnung**; `annualCap` begrenzt den im Kalenderjahr abgerechneten Betrag vor Mehrwertsteuer. `vatRate` ist ein Anteil, beispielsweise 0.081. Übernehmen Sie ein jährliches Minimum nie direkt als vierteljährliches Minimum. Die Simulation führt aufgelaufene, unbezahlte Gebühren als Verpflichtung. Die Abrechnung reduziert das Bargeld und löst diese Verpflichtung auf, damit die Performance nicht doppelt belastet wird. Endet eine Simulation vor der nächsten Rechnung, ist die aufgelaufene Verpflichtung trotzdem enthalten. Beim Schliessen eines Depots werden aufgelaufene Gebühren eines nachschüssigen Modells abgerechnet; vorausbezahlte Gebühren werden nicht erstattet.

Eine Simulationseröffnung an einer Kalendergrenze kann ohne Abgrenzungen beginnen. Andernfalls geben Sie den Eröffnungszustand ein, wie unter [Gebühren in der Wiederholung]({{% ref "/algoalert/historicalrun/fees" %}}) beschrieben. Die Simulation hält Modelle und Eröffnungsbestände bei der Übermittlung fest. Das Protokoll zeigt abgerechnete Gebühren, Mehrwertsteuer, massgebende Periode und verbrauchte Guthaben. Depots ohne Abschnitt `custody` bleiben ausdrücklich nicht modelliert. Das Panel **Gebührenschätzung testen** schätzt **nur Courtagen**; es berechnet keine Depotgebühren und verbraucht keine Guthaben. Für die Aufschläge bei der Währungsumrechnung gibt es das eigene Panel **Währungsumrechnung**.

#### Gebühren je Börse
Manche Broker verlangen für jede Börse, an der Positionen gehalten oder Trades ausgeführt werden, eine jährliche Gebühr. Im Ausdruck `amount` einer bewerteten Abrechnung enthält `exchangeCount` die Anzahl verschiedener Börsen der Abrechnungsperiode: Börsen der in die Periode übernommenen Positionen, der bei einer Beobachtung gehaltenen Positionen und der in der Periode gebuchten Trades. `positionCount` enthält die Anzahl Positionen der Beobachtung. Die optionale `exchangeCondition` bestimmt, welche Börsen gezählt werden, beispielsweise um Börsen ohne Gebühr auszuschliessen; sie kann `mic`, Instrumenttyp, Anlageklasse, ISIN und Währung verwenden. Ohne diese Angabe zählt jede Börse.

```yaml
custody:
  periods:
    - validFrom: "2026-01-01"
      status: ASSUMED
      source: "Gebührenverzeichnis des Brokers"
      note: "Gebühr je Börse und Kalenderjahr, höchstens 0.25 Prozent des Depotwerts."
      currency: EUR
      valuation: PERIOD_END
      billingMonths: 12
      collection: END
      billingDay: 0
      dayCount: ACTUAL_ACTUAL
      amount: "MIN(2.5 * exchangeCount, 0.0025 * accountValue)"
      exchangeCondition: 'mic != "XAMS"'
```

Die Simulation belastet diese Gebühr am Ende der Abrechnungsperiode, nicht beim ersten Trade an einer Börse. Eine Obergrenze im Verhältnis zum Depotwert verwendet den Wert bei der Beobachtung, nicht den höchsten Wert des Jahres.

### Aufschläge bei der Währungsumrechnung
Viele Broker belasten einen Währungsumtausch nicht als eigene Gebühr, sondern rechnen zu einem etwas schlechteren Kurs als dem Marktkurs um. Der Abschnitt `fx` auf oberster Ebene beschreibt diese Marge als **prozentualen Aufschlag** auf den Tagesschluss-Mittelkurs des Währungspaars. Wer die erste Währung des Paars kauft, bezahlt den Aufschlag zusätzlich; wer sie verkauft, erhält entsprechend weniger. Ergänzen Sie den Abschnitt `fx` neben den Courtagenregeln; das eigene Dokument eines Depots darf auch nur den Abschnitt `fx` enthalten.

Der Aufschlag wird an drei Stellen verwendet: Die [historische Wiederholung]({{% ref "/algoalert/historicalrun/fees#aufschläge-bei-der-währungsumrechnung" %}}) rechnet damit um, die [beobachtete Wechselkursabweichung]({{% ref "/reportportfolio/transactioncosts#beobachtete-wechselkursabweichung-vom-tagesschlusskurs" %}}) im Bericht Transaktionskosten vergleicht ihn mit den erfassten Wechselkursen, und das Testpanel **Währungsumrechnung** unten berechnet ihn für eine einzelne Umrechnung. Echte Transaktionen verändert er nie.

Wie die Courtagen enthält der Abschnitt `fx` entweder flache `rules` oder datierte `periods`, nie beides. Jede Periode braucht `validFrom`, `status` und `source`; `validTo` ist optional und gilt einschliesslich. `status` ist `VERIFIED` für einen belegten Tarif oder `ASSUMED` für eine bewusst gewählte Annahme, die zusätzlich eine erklärende `note` erfordert. Perioden dürfen sich nicht überlappen. Innerhalb der gültigen Periode bestimmt die erste Regel, deren Bedingung zutrifft, den Aufschlag. Ihr Ausdruck liefert den Aufschlag **in Prozent**: `1.5` bedeutet 1,5 %. Zulässig sind nur Werte von null bis unter 5 %; ein grösseres oder negatives Ergebnis gilt als ungültiger Tarif. Fixe Umrechnungsgebühren und Mindestgebühren lassen sich mit diesem prozentualen Modell nicht abbilden.

Die optionalen `currencyClasses` fassen Währungen unter frei gewählten Namen zusammen, beispielsweise `MAJOR` oder `EMERGING`. Jede Währung darf nur einer Klasse angehören. Eine Währung ohne Klasse hat eine leere Klasse. Die optionale `amountCurrency` ist die Währung, in der Betragsstufen abgestuft sind; sie ist erforderlich, sobald eine Regel `tierAmount` verwendet.

| Variable | Typ | Beschreibung |
|----------|-----|--------------|
| `payCurrency` | String | ISO-Code der abgegebenen Währung |
| `receiveCurrency` | String | ISO-Code der erhaltenen Währung |
| `kind` | String | `"TRADE"` für einen Kauf oder Verkauf, `"INCOME"` für eine Dividenden- oder Zinszahlung, `"TRANSFER"` für einen Kontoübertrag |
| `amount` | Numerisch | Abgegebener Betrag in der abgegebenen Währung, zum Mittelkurs vor dem Aufschlag bewertet; ein Kauf enthält Courtage, Steuern und Marchzinsen, ein Verkauf oder Ertrag verwendet den Nettoerlös |
| `tierAmount` | Numerisch | `amount` umgerechnet in `amountCurrency` |
| `payClass` / `receiveClass` | String | Klasse der abgegebenen oder erhaltenen Währung aus `currencyClasses` |
| `mic` | String | MIC-Code der Börse des Wertpapiers; leer bei einem Kontoübertrag |

Beispiel mit Währungsklassen:
```yaml
fx:
  currencyClasses:
    MAJOR: [USD, EUR, JPY, GBP, CHF, AUD, CAD, NZD]
    MINOR: [NOK, SEK, DKK, SGD, HUF, PLN, CZK, CNH]
  periods:
    - validFrom: "1900-01-01"
      status: ASSUMED
      source: "Devisenseite des Brokers"
      note: "Klassenmatrix beobachtet 2026-09, auf die ganze Historie angewendet."
      rules:
        - name: "Major/Major"
          condition: "payClass == 'MAJOR' && receiveClass == 'MAJOR'"
          expression: "0.95"
        - name: "Alle übrigen Umrechnungen"
          condition: "true"
          expression: "1.5"
```
Beispiel mit Betragsstufen in Franken, unabhängig von den tatsächlich umgerechneten Währungen:
```yaml
fx:
  amountCurrency: CHF
  rules:
    - name: "Bis CHF 50'000"
      condition: "tierAmount <= 50000"
      expression: "1.5"
    - name: "Über CHF 50'000"
      condition: "true"
      expression: "1.0"
```
Eine ausdrückliche Regel mit null Prozent bedeutet, dass eine Umrechnung kostenlos ist. Das ist etwas anderes als eine Umrechnung, die keine Regel abdeckt. GT unterscheidet deshalb folgende Fälle: **Kein Währungstarif konfiguriert**, **Kein Währungstarif für dieses Datum**, **Keine Währungsregel für diese Umrechnung**, **Historischer Kurs für die Tarifwährung fehlt** (eine Stufe in einer dritten Währung braucht deren Schlusskurs am Tag) und **Ungültiger Währungstarif oder ungültige Anfrage**. Ein ungültiger Tarif wird nie als Aufschlag von null behandelt.

Die mitgelieferten Handelsplattform Pläne für Swissquote, Migros Bank, PostFinance und Saxo enthalten einen Währungstarif, als Annahme mit Quelle gekennzeichnet. Er wurde nur ergänzt, wo das Gebührenmodell des Plans noch unverändert war; ein selbst bearbeiteter Plan behält sein Dokument.

#### Testpanel Währungsumrechnung
Unter **Gebührenschätzung testen** enthält der Dialog des Gebührenmodells das aufklappbare Panel **Währungsumrechnung**. Es berechnet den Aufschlag einer einzelnen Umrechnung aus dem aktuellen, noch nicht gespeicherten YAML-Text. Geben Sie **Abgegebene Währung**, **Erhaltene Währung**, **Art der Umrechnung** (Wertpapierhandel, Ertrag oder Kontoübertrag), den **Betrag** in der abgegebenen Währung, das **Datum** und optional die **MIC** ein und klicken Sie auf **Währungsaufschlag testen**. Das Ergebnis zeigt, welcher der obigen Fälle zutrifft. Bei einer zutreffenden Regel erscheinen zusätzlich **Aufschlag (%)**, **Gültig ab** der Periode, **Zutreffende Regel** und **Status** (Verifiziert oder Angenommen). Im Dialog eines Depots verwendet der Test das ungespeicherte Depotdokument zusammen mit dem Währungstarif des Plans, genau so, wie er geerbt würde. Erst **Speichern** legt das Dokument ab.

### JSON-Schema
Das YAML muss dem folgenden JSON-Schema entsprechen. Es kann auch als Kontext für ein LLM verwendet werden, um gültiges Gebührenmodell-YAML zu generieren.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Fee Model Configuration",
  "description": "Trading commissions with optional independently dated custody charges, expiring trading credits and percentage FX conversion markups.",
  "type": "object",
  "oneOf": [
    {
      "required": [
        "rules"
      ],
      "properties": {
        "rules": {
          "type": "array",
          "description": "Fee rules (no date restriction). Evaluated top-to-bottom; first matching condition wins.",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FeeRule"
          }
        },
        "custody": {
          "$ref": "#/$defs/Custody"
        },
        "fx": {
          "$ref": "#/$defs/Fx"
        }
      },
      "additionalProperties": false
    },
    {
      "required": [
        "periods"
      ],
      "properties": {
        "periods": {
          "type": "array",
          "description": "Time-based fee periods. Each period has a date range and its own rules array. First period matching the transaction date is used.",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FeeModelPeriod"
          }
        },
        "custody": {
          "$ref": "#/$defs/Custody"
        },
        "fx": {
          "$ref": "#/$defs/Fx"
        }
      },
      "additionalProperties": false
    },
    {
      "required": [
        "fx"
      ],
      "properties": {
        "fx": {
          "$ref": "#/$defs/Fx"
        }
      },
      "additionalProperties": false
    }
  ],
  "$defs": {
    "FeeRule": {
      "type": "object",
      "required": [
        "name",
        "condition",
        "expression"
      ],
      "properties": {
        "name": {
          "type": "string",
          "description": "Human-readable rule name (e.g., 'Swiss stocks - Premium tier')"
        },
        "condition": {
          "type": "string",
          "description": "EvalEx boolean expression. Variables: tradeValue (trade amount), units (share count), instrument (string: DIRECT_INVESTMENT, ETF, MUTUAL_FUND, PENSION_FUNDS, CFD, FOREX, ISSUER_RISK_PRODUCT, NON_INVESTABLE_INDICES), assetclass (string: EQUITIES, FIXED_INCOME, MONEY_MARKET, COMMODITIES, REAL_ESTATE, MULTI_ASSET, CONVERTIBLE_BOND, CREDIT_DERIVATIVE, CURRENCY_PAIR), mic (MIC code, e.g. XSWX, XNYS), currency (ISO code), fixedAssets (portfolio value), tradeDirection (0=buy, 1=sell), settlementCurrency (ISO code of the settling cash account), tradesInMonth / tradesInQuarter / tradesInYear (earlier trades of the account in the calendar period), securityTradesInMonth (earlier trades of this security in the calendar month). Legacy numeric aliases: specInvestInstrument, categoryType. Use 'true' for a catch-all default rule."
        },
        "expression": {
          "type": "string",
          "description": "EvalEx numeric expression for fee calculation. Supports MAX(), MIN(), IF(), ABS(), ROUND(), arithmetic, comparisons. Example: 'MAX(9.0, tradeValue * 0.001)'"
        }
      }
    },
    "FeeModelPeriod": {
      "type": "object",
      "required": [
        "validFrom",
        "rules"
      ],
      "properties": {
        "validFrom": {
          "type": "string",
          "format": "date",
          "description": "Start date (inclusive) in YYYY-MM-DD format"
        },
        "validTo": {
          "type": "string",
          "format": "date",
          "description": "End date (inclusive) in YYYY-MM-DD format. Omit for open-ended (until further notice)."
        },
        "rules": {
          "type": "array",
          "description": "Fee rules for this period",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FeeRule"
          }
        }
      }
    },
    "CustodyPeriod": {
      "type": "object",
      "required": [
        "validFrom",
        "status",
        "source"
      ],
      "properties": {
        "validFrom": {
          "type": "string",
          "format": "date"
        },
        "validTo": {
          "type": "string",
          "format": "date"
        },
        "status": {
          "enum": [
            "VERIFIED",
            "ASSUMED",
            "UNRESOLVED"
          ]
        },
        "source": {
          "type": "string",
          "minLength": 1
        },
        "note": {
          "type": "string"
        },
        "currency": {
          "type": "string",
          "pattern": "^[A-Z]{3}$"
        },
        "valuation": {
          "enum": [
            "NONE",
            "DAILY",
            "MONTHLY",
            "PERIOD_END"
          ]
        },
        "observationDay": {
          "type": "integer",
          "minimum": 0,
          "maximum": 31,
          "description": "Monthly valuation day; 0 means month end."
        },
        "billingMonths": {
          "enum": [
            1,
            3,
            6,
            12
          ],
          "description": "Billing period length in months, aligned to the calendar year."
        },
        "collection": {
          "enum": [
            "START",
            "END"
          ]
        },
        "billingDay": {
          "type": "integer",
          "minimum": 0,
          "maximum": 31
        },
        "dayCount": {
          "enum": [
            "ACTUAL_365",
            "ACTUAL_360",
            "ACTUAL_ACTUAL"
          ]
        },
        "closingPolicy": {
          "enum": [
            "ACCRUED",
            "FULL_PERIOD",
            "PRORATED"
          ],
          "description": "Required for an arrears account closing mid-period. ACCRUED keeps daily/monthly observations; FULL_PERIOD assesses fixed/period-end charges in full; PRORATED scales fixed/period-end fees and bill limits by elapsed calendar days."
        },
        "amount": {
          "type": "string",
          "description": "EvalEx annual fee, or amount per bill for NONE. Variables: assetValue (weighted custody value), accountValue (all securities), exchangeCount (distinct exchanges held or traded in the billing period, filtered by exchangeCondition), positionCount (observed positions). No cash. Example per-exchange fee: 'MIN(2.5 * exchangeCount, 0.0025 * accountValue)'."
        },
        "valueRules": {
          "type": "array",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FeeRule"
          },
          "description": "First match returns chargeable position value. Variables: positionValue, accountValue, instrument, assetclass, isin, currency, mic."
        },
        "exchangeCondition": {
          "type": "string",
          "description": "EvalEx boolean deciding which exchanges exchangeCount counts. Variables: mic, instrument, assetclass, isin, currency. Omitted means every exchange counts. Example: 'mic != \"XETR\" && mic != \"XAMS\"'."
        },
        "creditEligibility": {
          "type": "string",
          "description": "Eligible online commission: instrument, assetclass, isin, currency, mic, tradeDirection. Excludes taxes and corporate actions."
        },
        "minimum": {
          "type": "number",
          "minimum": 0
        },
        "maximum": {
          "type": "number",
          "minimum": 0
        },
        "annualCap": {
          "type": "number",
          "minimum": 0
        },
        "vatRate": {
          "type": "number",
          "minimum": 0
        },
        "creditAmount": {
          "type": "number",
          "minimum": 0
        }
      },
      "additionalProperties": false,
      "allOf": [
        {
          "if": {
            "properties": {
              "status": {
                "enum": [
                  "VERIFIED",
                  "ASSUMED"
                ]
              }
            }
          },
          "then": {
            "required": [
              "currency",
              "valuation",
              "billingMonths",
              "collection",
              "billingDay",
              "dayCount",
              "amount"
            ]
          }
        }
      ]
    },
    "Custody": {
      "type": "object",
      "required": [
        "periods"
      ],
      "properties": {
        "periods": {
          "type": "array",
          "minItems": 1,
          "maxItems": 100,
          "items": {
            "$ref": "#/$defs/CustodyPeriod"
          }
        }
      },
      "additionalProperties": false
    },
    "Fx": {
      "type": "object",
      "properties": {
        "amountCurrency": {
          "type": "string",
          "pattern": "^[A-Z]{3}$"
        },
        "currencyClasses": {
          "type": "object",
          "additionalProperties": {
            "type": "array",
            "items": {
              "type": "string",
              "pattern": "^[A-Z]{3}$"
            }
          }
        },
        "rules": {
          "type": "array",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FeeRule"
          }
        },
        "periods": {
          "type": "array",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FxPeriod"
          }
        }
      },
      "oneOf": [
        {
          "required": [
            "rules"
          ],
          "not": {
            "required": [
              "periods"
            ]
          }
        },
        {
          "required": [
            "periods"
          ],
          "not": {
            "required": [
              "rules"
            ]
          }
        }
      ],
      "additionalProperties": false
    },
    "FxPeriod": {
      "type": "object",
      "required": [
        "validFrom",
        "status",
        "source",
        "rules"
      ],
      "properties": {
        "validFrom": {
          "type": "string",
          "format": "date"
        },
        "validTo": {
          "type": "string",
          "format": "date"
        },
        "status": {
          "enum": [
            "VERIFIED",
            "ASSUMED"
          ]
        },
        "source": {
          "type": "string",
          "minLength": 1
        },
        "note": {
          "type": "string"
        },
        "rules": {
          "type": "array",
          "minItems": 1,
          "items": {
            "$ref": "#/$defs/FeeRule"
          }
        }
      },
      "additionalProperties": false
    }
  }
}
```

### Variablenreferenz
Die folgenden Variablen stehen in Bedingungen und Ausdrücken zur Verfügung:

| Variable | Typ | Beschreibung |
|----------|-----|--------------|
| `tradeValue` | Numerisch | Gesamtbetrag des Trades (Preis × Anzahl) |
| `units` | Numerisch | Anzahl der Anteile/Stücke |
| `instrument` | String | Finanzinstrumenttyp, z.B. `"DIRECT_INVESTMENT"`, `"ETF"`, `"MUTUAL_FUND"`, `"PENSION_FUNDS"`, `"CFD"`, `"FOREX"`, `"ISSUER_RISK_PRODUCT"` |
| `assetclass` | String | Anlageklasse, z.B. `"EQUITIES"`, `"FIXED_INCOME"`, `"MONEY_MARKET"`, `"COMMODITIES"`, `"REAL_ESTATE"`, `"MULTI_ASSET"`, `"CONVERTIBLE_BOND"`, `"CREDIT_DERIVATIVE"`, `"CURRENCY_PAIR"` |
| `mic` | String | MIC-Code der Börse, z.B. `"XSWX"`, `"XNYS"` |
| `currency` | String | ISO-Währungscode, z.B. `"CHF"`, `"USD"` |
| `fixedAssets` | Numerisch | Gesamtwert des Portfolios |
| `tradeDirection` | Numerisch | `0` = Kauf, `1` = Verkauf |
| `settlementCurrency` | String | ISO-Währungscode des abwickelnden Geldkontos; leer, wenn unbekannt |
| `tradesInMonth` | Numerisch | Frühere Trades des Depots im Kalendermonat des Trades |
| `tradesInQuarter` | Numerisch | Frühere Trades des Depots im Kalenderquartal des Trades |
| `tradesInYear` | Numerisch | Frühere Trades des Depots im Kalenderjahr des Trades |
| `securityTradesInMonth` | Numerisch | Frühere Trades desselben Wertpapiers im Kalendermonat des Trades |
| `transactionDate` | Datum | Relevant für das Periodenformat — bestimmt, welche Periode gilt |

Zusätzlich stehen die numerischen Legacy-Variablen `specInvestInstrument` und `categoryType` als ordinale Byte-Werte der Instrument- bzw. Anlageklassen-Enums zur Verfügung.

### EvalEx-Funktionen
Bedingungen und Ausdrücke unterstützen die Standard-**EvalEx**-Funktionsbibliothek. Häufig verwendete Funktionen sind:
+ `MAX(a, b)` und `MIN(a, b)` — gibt den grösseren bzw. kleineren Wert zurück
+ `IF(bedingung, dannWert, sonstWert)` — bedingter Ausdruck
+ `ABS(wert)` — Absolutwert
+ `ROUND(wert, dezimalstellen)` — Rundung
+ Arithmetische Operatoren: `+`, `-`, `*`, `/`
+ Vergleiche: `==`, `!=`, `>`, `<`, `>=`, `<=`
+ String-Gleichheit: `instrument == "ETF"`

### Gebührenschätzung testen
Im Gebührenmodell-Dialog befindet sich unterhalb des YAML-Editors ein einklappbares Panel **Gebührenschätzung testen**. Damit können Sie überprüfen, ob Ihre Regeln die erwarteten Kosten liefern. Die Testparameter verwenden Dropdown-Auswahlfelder, wo immer möglich, so dass Sie z.B. eine Börse oder Währung bequem aus einer Liste wählen können.

| Feld | Beschreibung |
|------|-------------|
| **Handelswert** | Gesamtbetrag des Trades (Preis × Anzahl) |
| **Anzahl Position** | Anzahl der gehandelten Anteile/Stücke |
| **MIC** | Börse als Dropdown (z.B. «Euronext Lisbon - XLIS») |
| **Währung** | Handelswährung als Dropdown (z.B. CHF, USD) |
| **Depotwert** | Gesamtwert des Portfolios/Depots |
| **Finanzinstrument** | Instrumenttyp als Dropdown (z.B. ETF, Direktanlage, Investmentfonds) |
| **Anlageklasse** | Anlageklasse als Dropdown (z.B. Aktien, Obligationen, Rohstoffe) |
| **Handelsrichtung** | Dropdown mit Kaufen / Verkaufen |
| **Abwicklungswährung** | Währung des abwickelnden Geldkontos als Dropdown; leer lassen, wenn das Modell sie nicht verwendet |
| **Frühere Trades im Monat** | Anzahl früherer Trades im Kalendermonat; leer zählt als 0 |
| **Frühere Trades im Quartal** | Anzahl früherer Trades im Kalenderquartal; leer zählt als 0 |
| **Frühere Trades im Jahr** | Anzahl früherer Trades im Kalenderjahr; leer zählt als 0 |
| **Frühere Trades dieses Wertpapiers im Monat** | Anzahl früherer Trades desselben Wertpapiers im Kalendermonat; leer zählt als 0 |
| **Transaktionsdatum** | Datumswähler — relevant für das Periodenformat |

1. Füllen Sie die gewünschten Testparameter aus.
2. Klicken Sie auf **Gebührenschätzung testen**. GT prüft den aktuellen YAML-Text und berechnet damit die Schätzung, ohne das Modell zu speichern. Das gilt sowohl für Handelsplattform-Pläne als auch für depotspezifische Modelle.
3. Das Ergebnis zeigt die **geschätzten Kosten** und den **Namen der zutreffenden Regel** an, oder eine Fehlermeldung, falls keine Regel zutraf.
4. Erst **Speichern** übernimmt Ihre Änderungen dauerhaft. Das gilt unabhängig davon, ob Sie zuvor eine Schätzung getestet haben. Schliessen Sie den Dialog ohne Speichern, bleibt das bisher gespeicherte Modell bestehen.

{{% notice style="info" title="Vergleich mit realen Transaktionskosten" %}}
Während das Testpanel oben einzelne hypothetische Transaktionen validiert, vergleicht der [Gebührenmodellvergleich]({{% ref "/reportportfolio/transactioncosts" %}}) im Transaktionskosten-Report die geschätzten Kosten des Gebührenmodells mit den tatsächlich erfassten Kosten aller Kauf-/Verkaufstransaktionen eines Depots. Er liefert statistische Kennzahlen (mittlerer absoluter Fehler, mittlerer relativer Fehler, RMSE), um die Gesamtgenauigkeit des Gebührenmodells zu bewerten. Der Vergleich geht die Transaktionen in Buchungsreihenfolge durch, zählt die früheren Trades für die Variablen der Freikontingente und übernimmt die Abwicklungswährung aus dem Geldkonto jeder Transaktion.
{{% /notice %}}
