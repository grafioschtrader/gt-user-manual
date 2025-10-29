---
title: "Periodenertrag"
date: 2025-10-28T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---

Der **Periodenertrag-Report** bietet eine detaillierte Analyse der Performance eines Portfolios oder Mandanten über einen definierten Zeitraum. Der Report ermöglicht es, die Wertentwicklung tages-, wochen- oder monatsweise zu verfolgen und die verschiedenen Einflussfaktoren auf die Gesamtperformance zu identifizieren.

## Aufruf des Reports

Der Periodenertrag-Report kann auf zwei Ebenen aufgerufen werden:

1. **Auf Mandantenebene**: Über das Hauptmenü → **Depots und Konten** → **Portfolios** → **Periodenertrag**
   - Analysiert die Performance aller Portfolios des Mandanten zusammen
   - Werte werden in der Mandantenwährung dargestellt

2. **Auf Portfolioebene**: Innerhalb eines einzelnen Portfolios → **Periodenertrag**
   - Analysiert nur das ausgewählte Portfolio
   - Werte werden in der Portfoliowährung dargestellt

## Eingabeparameter

Vor der Berechnung des Reports müssen folgende Parameter festgelegt werden:

### Datum von
Das Startdatum der Analyseperiode (inklusiv). Wichtig: Das Datum bezieht sich auf den Börsenschluss, d.h. der Ertrag des Starttages ist **nicht** in der Berechnung enthalten.

**Einschränkungen:**
- Muss ein gültiger Handelstag sein (kein Wochenende, Feiertag oder Tag mit fehlenden Kursen)
- Darf nicht vor dem ersten Handelstag mit verfügbaren Daten liegen

### Datum bis
Das Enddatum der Analyseperiode (inklusiv). Das Datum bezieht sich auf den Börsenschluss.

**Einschränkungen:**
- Muss ein gültiger Handelstag sein
- Muss nach dem Startdatum liegen
- Darf nicht nach dem letzten verfügbaren Handelstag liegen

### Periodenaufteilung
Bestimmt, wie die Daten im Report aggregiert werden:

- **Woche**: Zeigt die Performance nach Wochentagen (Montag-Freitag) an
  - Nur verfügbar, wenn der Zeitraum maximal die konfigurierte Anzahl von Wochen umfasst
  - Ideal für detaillierte kurzfristige Analysen

- **Jahr/Monat**: Zeigt die Performance nach Monaten an
  - Nur verfügbar, wenn der Zeitraum mindestens die konfigurierte Anzahl von Monaten umfasst
  - Ideal für längerfristige Übersichten

Die Verfügbarkeit der Optionen wird dynamisch basierend auf dem gewählten Zeitraum angepasst.

## Berichtsinhalt

Der Periodenertrag-Report besteht aus mehreren Hauptbereichen:

### 1. Zusammenfassung (Periodenvergleich)

Dieser Bereich zeigt drei Spalten:

- **Erster Tag**: Alle Werte zum Startdatum der Periode
- **Letzter Tag**: Alle Werte zum Enddatum der Periode
- **Differenz**: Die Veränderung zwischen Start und Ende

**Dargestellte Metriken:**

| Spalte | Beschreibung |
|--------|-------------|
| **Datum** | Das jeweilige Datum |
| **Dividende real** | Realisierte Dividendenerträge (kumulativ) |
| **Gebühren real** | Realisierte Gebühren für Depotführung und Transaktionen (kumulativ) |
| **Zins Konto real** | Realisierte Zinserträge auf Cashkonten (kumulativ) |
| **Saldo Kauf/Verkauf** | Nettobetrag aus Wertpapierkäufen und -verkäufen |
| **Kassabestand** | Gesamter Kassenbestand über alle Konten |
| **Externe Übertragung** | Ein- und Auszahlungen |
| **Wertpapiere** | Marktwert aller Wertpapierbestände |
| **Realisierter Gewinn Margin-Positionen** | Realisierte Gewinne/Verluste aus geschlossenen Margin-Positionen |
| **Wertpapierrisiko** | Marktwert offener Positionen (unrealisiert) |
| **Gewinn** | Gesamtgewinn/-verlust aus Wertpapieren |
| **Wertpapiere und Margin-Gewinn** | Kombinierter Wert aus Wertpapieren und Margin-Gewinnen |
| **Gesamtgewinn** | Gesamtgewinn inklusive regulärer und Margin-Gewinne |
| **Gesamtbilanz** | Gesamtes Portfoliovermögen (Kasse + Wertpapiere + Margin-Gewinne) |

**Wichtig**: Alle Werte werden in der Hauptwährung (MC = Main Currency) dargestellt - entweder Mandanten- oder Portfoliowährung, je nach Aufrufkontext.

### 2. Detaillierte Periodenfenster (Tabellenansicht)

Dieser Bereich zeigt die täglichen Veränderungen strukturiert nach Perioden (Wochen oder Monate):

**Bei wöchentlicher Aufteilung:**
- Jede Zeile repräsentiert eine Woche
- Spalten zeigen die Tage Montag bis Freitag
- Am Ende: Summenspalte für die Woche

**Bei monatlicher Aufteilung:**
- Jede Zeile repräsentiert ein Jahr
- Spalten zeigen die 12 Monate
- Am Ende: Summenspalte für das Jahr

**Pro Zelle werden folgende Informationen angezeigt:**

| Wert | Beschreibung |
|------|-------------|
| **Einlage** | Externe Übertragungen an diesem Tag (positiv = Einzahlung, negativ = Auszahlung) |
| **Gewinn** | Täglicher Gewinn/Verlust aus allen Quellen |
| **Kassabestand** | Veränderung des Kassenbestands |
| **Wertpapiere** | Veränderung des Wertpapierwerts |
| **Gesamtbilanz** | Tägliche Veränderung der Gesamtbilanz |
| **Fehlende Tage** | Anzahl aufeinanderfolgender Tage mit fehlenden Daten vor diesem Tag |

**Farbcodierung in der Tabelle:**
- 🟩 **Grün**: Regulärer Handelstag mit Daten
- 🟨 **Gelb**: Feiertag (keine Handelsaktivität)
- 🟥 **Rot**: Handelstag mit fehlenden historischen Kursdaten
- ⬜ **Grau**: Wochenende oder nicht relevanter Tag

### 3. Spaltensummen

Am Ende jeder Spalte (Wochentag oder Monat) wird die Summe der Gewinne für alle entsprechenden Tage/Monate angezeigt. Dies ermöglicht es, Muster zu erkennen (z.B. "Ist Montag ein schlechter Tag für mein Portfolio?").

### 4. Grafische Darstellung

Der Report bietet die Möglichkeit, die Performance als Chart anzuzeigen:

**Aufruf**: Button "Grafik anzeigen" im Menü

**Dargestellte Linien:**
- **Externe Übertragung Diff**: Kumulierte Ein-/Auszahlungen
- **Gewinn Diff**: Kumulierte Gewinne/Verluste
- **Kassabestand Diff**: Veränderung des Kassenbestands
- **Wertpapiere Diff**: Veränderung des Wertpapierwerts
- **Gesamtbilanz**: Gesamtvermögensentwicklung

Der Chart verwendet Plotly und bietet interaktive Funktionen wie Zoom, Hover-Details und Range-Selektor.

## Fehlende Tagesendkurse

Ein wichtiger Aspekt des Periodenertrag-Reports ist die Behandlung fehlender Kursdaten. Fehlende Kursdaten entstehen, wenn an einem Handelstag keine historischen Kurse für gehaltene Wertpapiere verfügbar sind. Dies kann verschiedene Ursachen haben: Der Kursanbieter hat keine Daten geliefert, es sind technische Probleme beim Datenabruf aufgetreten, oder das Wertpapier wurde an diesem Tag tatsächlich nicht gehandelt.

Die Auswirkungen auf den Report sind erheblich. Fehlende Tage werden im Periodenkalender rot markiert, und die Performance kann an diesen Tagen nicht berechnet werden. Das System zählt die aufeinanderfolgenden fehlenden Tage und zeigt diese im Feld "Fehlende Tage" an. Bei der Datumsauswahl werden Tage mit fehlenden Kursen automatisch als ungültige Handelstage markiert, sodass diese nicht als Start- oder Enddatum gewählt werden können.

{{% notice warning %}}
Ohne vollständige historische Kursdaten ist eine genaue Performance-Berechnung nicht möglich. Es wird dringend empfohlen, fehlende Kurse vor der Analyse zu beheben, um aussagekräftige Ergebnisse zu erhalten.
{{% /notice %}}

### Übersicht und Interaktion mit fehlenden Kursen

Der Report bietet eine separate Ansicht zur Analyse und Behebung fehlender Kursdaten. Diese kann über das Menü unter "Fehlende Tagesendkurse" aufgerufen werden und zeigt zwei miteinander verbundene Bereiche: Einen Jahreskalender im oberen Bereich und eine Wertpapier-Tabelle im unteren Bereich.

Der **Jahreskalender** zeigt alle Handelstage des ausgewählten Jahres in einer übersichtlichen Darstellung. Die Farbcodierung macht sofort ersichtlich, wo Probleme vorliegen: Grün markierte Tage signalisieren, dass alle Kurse verfügbar sind. Rot markierte Tage zeigen an, dass mindestens ein Wertpapier fehlende Kurse aufweist. Gelb markierte Tage sind Tage mit fehlenden Kursen, die vom Benutzer selektiert wurden. Wenn Sie auf einen rot markierten Tag klicken, reagiert die Wertpapier-Tabelle sofort und zeigt nur noch die Wertpapiere an, die an diesem spezifischen Tag fehlende Kurse aufweisen.

Die **Wertpapier-Tabelle** listet alle Wertpapiere auf, die im gewählten Zeitraum fehlende Kurse haben. Neben dem Namen des Wertpapiers wird auch die Anzahl der fehlenden Tage angezeigt. Auch hier funktioniert die Interaktion bidirektional: Wenn Sie in der Tabelle auf ein Wertpapier klicken, werden im Kalender automatisch alle Tage gelb markiert, an denen dieses spezifische Wertpapier fehlende Kurse hat. Diese bidirektionale Interaktion ermöglicht es, systematische Datenlücken schnell zu identifizieren und gezielt zu beheben.

{{% notice tip %}}
Die Kombination aus Kalender und Tabelle erlaubt zwei Analyseansätze: Sie können entweder von einem problematischen Tag ausgehen und sehen, welche Wertpapiere betroffen sind, oder Sie können ein problematisches Wertpapier auswählen und sehen, an welchen Tagen Kurse fehlen.
{{% /notice %}}

### Lösung des Problems fehlender Kursdaten

Für die Behebung fehlender historischer Kursdaten stehen mehrere Möglichkeiten zur Verfügung. Die einfachste Methode ist das **manuelle Nachladen** der Kursdaten. Über die Wertpapier-Verwaltung können historische Kurse erneut vom Datenanbieter abgerufen werden. Wählen Sie das betroffene Wertpapier aus und triggern Sie eine Aktualisierung der historischen Daten.

Eine besonders elegante Lösung für einzelne fehlende Tage zwischen vorhandenen Kursen ist das **lineare Befüllen fehlender Kursdaten**. Das System kann Kurslücken durch lineare Interpolation füllen, wobei der fehlende Kurs aus den benachbarten vorhandenen Kursen berechnet wird. Diese Methode eignet sich besonders für einzelne fehlende Tage in ansonsten vollständigen Kursreihen, beispielsweise wenn der Datenanbieter an einem einzelnen Tag einen Ausfall hatte.

{{% notice style="info" title="Lineares Befüllen von Kurslücken" %}}
Für eine detaillierte Anleitung zum linearen Befüllen fehlender Kursdaten siehe [Lineares Befüllen fehlender Kursdaten](/gt-user-manual/de/watchlistinstrument/externaldata/historyquote/pricedata/). Diese Funktion interpoliert fehlende Werte basierend auf den umgebenden Kursen und eignet sich ideal für einzelne Lücken in der Zeitreihe.
{{% /notice %}}

Wenn ein Datenanbieter systematisch Lücken aufweist, kann es sinnvoll sein, zu einem **alternativen Datenanbieter** zu wechseln. GT unterstützt verschiedene Datenanbieter, und oft bietet ein anderer Anbieter eine bessere Abdeckung für bestimmte Märkte oder Wertpapiere. Die Umstellung des Datenanbieters erfolgt in den Einstellungen des jeweiligen Wertpapiers.

Für wenige fehlende Tage, insbesondere bei exotischen Wertpapieren mit schlechter Datenabdeckung, besteht auch die Möglichkeit der **manuellen Kurseingabe**. Diese Option ist zwar zeitaufwändig, kann aber in Einzelfällen die einzige Möglichkeit sein, eine vollständige Kurshistorie zu erreichen.

## Technische Details

### Handelstage und Feiertage

Das System berücksichtigt automatisch:
- **Globale Feiertage**: Weltweit gültige Nicht-Handelstage
- **Börsenspezifische Feiertage**: Feiertage der Börsen, an denen die gehaltenen Wertpapiere gehandelt werden
- **Wochenenden**: Samstag und Sonntag sind immer ausgeschlossen

### Währungskonvertierung

- Alle Wertpapiere und Konten werden automatisch in die Hauptwährung konvertiert
- Die Konvertierung verwendet historische Wechselkurse zum jeweiligen Datum
- Bei Mandantenauswertung: Konvertierung in Mandantenwährung
- Bei Portfolioauswertung: Konvertierung in Portfoliowährung

### Performance-Optimierung

- Das System verwendet einen Cache mit 2-minütiger Gültigkeit für Handelstag-Metadaten
- Datenabfragen werden parallel ausgeführt (CompletableFuture)
- Ergebnisse werden für wiederholte Anfragen innerhalb der Cache-Periode wiederverwendet

## Typische Anwendungsfälle

1. **Performance-Analyse**: Wie hat sich mein Portfolio über das letzte Quartal entwickelt?
2. **Einflussanalyse**: Welche Faktoren (Gewinne, Einzahlungen, Kursentwicklung) haben die Performance beeinflusst?
3. **Muster-Erkennung**: Gibt es bestimmte Wochentage oder Monate mit besonders guter/schlechter Performance?
4. **Datenqualität**: Gibt es systematische Lücken in meinen historischen Kursdaten?
5. **Vergleich**: Wie unterscheidet sich die Performance verschiedener Portfolios?

## Einschränkungen und Hinweise

- Der Report erfordert mindestens eine Transaktion im System
- Für eine aussagekräftige Analyse sollte der Zeitraum mindestens einige Handelstage umfassen
- Fehlende Kursdaten können die Genauigkeit der Berechnungen beeinträchtigen
- Die Auswahl der Periodenaufteilung wird automatisch basierend auf dem Zeitraum eingeschränkt
- Bei sehr großen Zeiträumen (mehrere Jahre) kann die Berechnung einige Sekunden dauern
