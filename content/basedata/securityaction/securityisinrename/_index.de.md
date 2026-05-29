---
title: "ISIN-Änderung"
date: 2026-03-10T22:54:47+01:00
lastmod: 2026-03-10T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
Wenn ein Wertpapier eine neue ISIN erhält — etwa durch eine Unternehmensfusion, Umstrukturierung oder regulatorische Änderung — müssen alle Bestände vom alten auf das neue Wertpapier übertragen werden. GT unterstützt diesen Vorgang als systemweite Aktion: Ein Administrator erstellt die ISIN-Änderung einmalig, und jeder betroffene Mandant kann sie auf seine eigenen Bestände anwenden.

## Ablauf im Überblick
Das folgende Diagramm zeigt den typischen Ablauf einer ISIN-Änderung — von der Entdeckung durch den Benutzer bis zur Anwendung auf die eigenen Bestände:

{{< mermaid >}}
sequenceDiagram
    participant B as Benutzer
    participant S as System (GT)
    participant A as Administrator

    B->>S: Entdeckt ISIN-Änderung<br/>(z.B. über Watchlist)
    B->>A: Meldet ISIN-Änderung<br/>per interner Mail
    A->>S: Erstellt ISIN-Änderung<br/>(Systemaktionen)
    S->>S: Referenziert bestehendes oder erstellt<br/>neues Wertpapier, zählt betroffene Mandanten
    S->>S: Übernimmt Connector-Einstellungen und<br/>lädt historische Kursdaten
    S->>B: Benachrichtigung<br/>per interner Mail
    B->>S: Wendet ISIN-Änderung an
    S->>S: Ordnet Transaktionen ab Aktionsdatum<br/>dem neuen Wertpapier zu
    S->>S: Erzeugt Verkauf (alte ISIN)<br/>und Kauf (neue ISIN)
    S-->>B: Status: Angewendet
{{< /mermaid >}}

## Administrator-Ablauf
Der Administrator erstellt eine ISIN-Änderung in der [Wertpapieraktion]({{% ref "/basedata/securityaction" %}})-TreeTable über **Rechtsklick** auf den Wurzelknoten **Systemaktionen (ISIN-Änderungen)** → **ISIN-Änderung erstellen**.

### Dialogfelder
| Feld | Pflicht | Beschreibung |
|------|---------|--------------|
| **Altes Wertpapier** | Ja | Das bestehende Wertpapier, dessen ISIN sich ändert. Wird über die Suchschaltfläche ausgewählt. |
| **Neue ISIN** | Ja | Die neue ISIN, die das Wertpapier erhält. Existiert bereits ein Wertpapier mit dieser ISIN, wird es referenziert; andernfalls erstellt GT automatisch ein neues Wertpapier. |
| **Aktionsdatum** | Ja | Datum, an dem die Änderung wirksam wird. Der Schlusskurs dieses Datums wird für die erzeugten Transaktionen herangezogen. |
| **Aus Anzahl** | Nein | Splitfaktor-Zähler. Standardwert 1. Bei einer reinen ISIN-Änderung ohne Aktiensplit bleibt dieser Wert auf 1. |
| **Wird Anzahl** | Nein | Splitfaktor-Nenner. Standardwert 1. Bei einem gleichzeitigen Aktiensplit von z.B. 1:10 wird hier 10 eingetragen. |
| **Bemerkung** | Nein | Optionale Notiz zur ISIN-Änderung. |

Nach dem Speichern referenziert GT das bestehende Wertpapier oder erstellt ein neues mit der angegebenen ISIN, zählt die betroffenen Mandanten und sendet ihnen eine interne Mailbenachrichtigung über die verfügbare ISIN-Änderung. Wenn GT ein neues Wertpapier erstellt, wird die Datenquellen-Konfiguration vom alten Wertpapier übernommen und die historischen Kursdaten werden automatisch im Hintergrund geladen.

{{% notice style="info" title="Kopierte und nicht kopierte Eigenschaften" %}}
Folgende Eigenschaften werden vom alten Wertpapier **übernommen**: Name (mit Suffix „ (new ISIN)"), Währung, Anlageklasse, Börse, Tickersymbol, Stückelung, Aktiv-bis-Datum, Ausschüttungshäufigkeit, Hebelfaktor, alle drei Konnektoren (Historisch, Intraday, Dividende) mit ihren URL-Erweiterungen, Börsenlink und Produktlink. Das **Aktiv-ab-Datum** wird auf das Aktionsdatum der ISIN-Änderung gesetzt.

**Nicht übernommen** werden: Notiz-/Beschreibungsfelder, historische Kursdaten (werden automatisch im Hintergrund geladen) und mandantenspezifische Watchlist-Zuordnungen.
{{% /notice %}}

{{% notice style="warning" title="Connector-Einstellungen prüfen" %}}
Die vom alten Wertpapier übernommenen Connector-Einstellungen sind für das neue Instrument möglicherweise nicht korrekt. Prüfen Sie nach der Erstellung der ISIN-Änderung, ob das neue Wertpapier tatsächlich historische Kursdaten aufweist und ob die konfigurierten Konnektoren (Historisch, Intraday und Dividende) korrekte Daten liefern. Passen Sie die Connector-Einstellungen des neuen Wertpapiers bei Bedarf an.
{{% /notice %}}

{{% notice style="info" title="Performance-Watchlist" %}}
Das neue Instrument wird nicht automatisch zur als Performance gekennzeichneten Watchlist hinzugefügt. Um es hinzuzufügen, öffnen Sie die entsprechende Watchlist und verwenden Sie den Suchdialog **Bestehendes Instrument hinzufügen** mit aktivierter Checkbox **Mit aktivem Bestand**.
{{% /notice %}}

### Löschen einer ISIN-Änderung
Der Administrator kann eine ISIN-Änderung über **Rechtsklick** → **ISIN-Änderung löschen** wieder entfernen. Dies ist nur möglich, solange kein Mandant die Aktion angewendet hat.

## Benutzer-Ablauf

### ISIN-Änderung anwenden
Hat ein Administrator eine ISIN-Änderung erstellt, sieht jeder betroffene Mandant die Aktion in seiner TreeTable mit dem Status **Nicht angewendet**. Um die Änderung anzuwenden: **Rechtsklick** auf die entsprechende Zeile → **ISIN-Änderung anwenden**.

GT führt dabei folgende Schritte aus:
1. Alle bestehenden Transaktionen des Mandanten, die ab dem Aktionsdatum das alte Wertpapier betreffen, werden **dem neuen Wertpapier zugeordnet**. Dies umfasst Dividenden, Kauf-/Verkaufstransaktionen und alle weiteren Vorgänge, die nach dem Inkrafttreten der ISIN-Änderung erfasst wurden.
2. Der Schlusskurs des alten Wertpapiers am Aktionsdatum wird abgerufen.
3. Für jedes betroffene Depot wird eine **Verkaufstransaktion** für das alte Wertpapier und eine **Kauftransaktion** für das neue Wertpapier erzeugt, basierend auf den verbleibenden Beständen vor dem Aktionsdatum.
4. Sind Splitfaktoren gesetzt (Aus Anzahl / Wird Anzahl ungleich 1:1), werden die Stückzahl und der Kurs auf der Kaufseite entsprechend angepasst.
5. Der Status wechselt auf **Angewendet**.

{{% notice style="warning" title="Voraussetzung" %}}
Für das Aktionsdatum muss ein Schlusskurs des alten Wertpapiers vorhanden sein. Fehlt dieser, kann die Aktion nicht angewendet werden.
{{% /notice %}}

### ISIN-Änderung rückgängig machen
Eine bereits angewendete ISIN-Änderung kann über **Rechtsklick** → **ISIN-Änderung rückgängig machen** rückgängig gemacht werden. Dabei werden alle automatisch erzeugten Verkaufs- und Kauftransaktionen gelöscht. Der Status wechselt auf **Rückgängig gemacht**. Die Aktion kann danach erneut angewendet werden.

## Praxisbeispiele
Inverse und gehebelte ETFs verlieren durch den täglichen Reset-Mechanismus und den sogenannten Volatilitätszerfall langfristig an Wert. Wenn der Kurs zu stark fällt, führen Emittenten einen Reverse Split durch, um die Börsenmindestpreise einzuhalten. In den USA erfordert ein Reverse Split eine neue CUSIP-Nummer und damit auch eine neue ISIN, da sich die Wertpapierstruktur wesentlich ändert. Solche ISIN-Änderungen mit gleichzeitigem Split sind ein typischer Anwendungsfall für die GT-Funktion.

### Beispiele: ProShares Inverse-ETFs
| Name | Reverse Split | Wirksam ab | Alte ISIN | Neue ISIN |
|------|---------------|------------|-----------|-----------|
| ProShares Short QQQ (PSQ) | 1:5 | 10.04.2024 | US74347B7148 | US74349Y8378 |
| ProShares Short S&P500 (SH) | 1:4 | 07.11.2024 | US74347B4251 | US74349Y7537 |

### Erfassung in GT
Für das Beispiel **ProShares Short QQQ** würde der Administrator die ISIN-Änderung wie folgt erfassen: Als **Altes Wertpapier** wird das Instrument mit der ISIN US74347B7148 ausgewählt, als **Neue ISIN** US74349Y8378 eingetragen und das **Aktionsdatum** auf den 10.04.2024 gesetzt. Da gleichzeitig ein Reverse Split von 1:5 stattfand, wird **Aus Anzahl** auf 1 und **Wird Anzahl** auf 5 gesetzt. GT erzeugt dann beim Anwenden die entsprechenden Verkaufs- und Kauftransaktionen mit angepasster Stückzahl.

{{% notice style="tip" title="Frühzeitig handeln" %}}
ISIN-Änderungen durch Reverse Splits werden vom Benutzer oft erst spät bemerkt, beispielsweise wenn historische Kursdaten nicht mehr aktualisiert werden oder Portfolioberichte unplausible Werte zeigen. Je länger die Änderung unbemerkt bleibt, desto mehr Transaktionen wurden unter der alten ISIN erfasst, die dann manuell korrigiert werden müssten. Die ISIN-Änderungsfunktion von GT ordnet alle betroffenen Transaktionen automatisch dem neuen Wertpapier zu und erspart so eine aufwändige manuelle Nachbearbeitung.
{{% /notice %}}
