---
title: "ISIN-Änderung"
date: 2026-10-21T12:00:00+02:00
lastmod: 2026-10-21T12:00:00+02:00
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
    S->>S: Sucht ein neues Wertpapier bei den Datenanbietern,<br/>wählt die Konnektoren und lädt die Kursdaten
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

Nach dem Speichern referenziert GT das bestehende Wertpapier oder erstellt ein neues mit der angegebenen ISIN, zählt die betroffenen Mandanten und sendet ihnen eine interne Mailbenachrichtigung über die verfügbare ISIN-Änderung. Muss GT ein neues Wertpapier erstellen, sucht es dieses zuerst bei den Datenanbietern; das Speichern kann deshalb bis zu einer Minute dauern.
### Wie GT das neue Wertpapier erstellt
Existiert noch kein Wertpapier mit der neuen ISIN, führt GT dieselbe [Suche bei den Datenanbietern]({{% relref "/watchlistinstrument/instrument/securityderived/security/securitylookup" %}}) aus, die im Bearbeitungsdialog eines Wertpapiers über **Suchen** erreichbar ist. Gesucht wird die neue ISIN am Handelsplatz, in der Währung und mit der Anlageklasse des alten Wertpapiers; dessen Ticker dient als zusätzlicher Suchbegriff. Anders als im Bearbeitungsdialog wählt GT Listing und Konnektoren selbst. Das ist nötig, weil die URL-Erweiterungen vieler Konnektoren die ISIN enthalten und jene des alten Wertpapiers für die neue ISIN keine oder falsche Daten liefern würden.

Für jeden der vier Konnektoren **Historischer Konnektor**, **Intraday Konnektor**, **Dividenden Konnektor** und **Split Konnektor** geht GT gleich vor:
- GT behält den Konnektor des alten Wertpapiers, wenn seine Probeabfrage für die neue ISIN erfolgreich war, und setzt die URL-Erweiterung, die die Suche für die neue ISIN ermittelt hat.
- Andernfalls wählt GT den ersten Konnektor, dessen Probeabfrage erfolgreich war.
- Ein historischer Konnektor kommt in beiden Fällen nur in Frage, wenn er Kurse ab dem Aktionsdatum liefert, denn dieses Datum wird beim neuen Wertpapier zu **Handel ab**.
- Findet sich kein geeigneter Konnektor, bleibt für diesen Konnektor die Einstellung des alten Wertpapiers erhalten.

{{< mermaid >}}
graph TD
    A[Konnektor des alten Wertpapiers] --> B{Probeabfrage mit neuer<br/>URL-Erweiterung erfolgreich?}
    B -->|Ja| C[Konnektor behalten,<br/>neue URL-Erweiterung setzen]
    B -->|Nein| D{Anderer Konnektor mit<br/>erfolgreicher Probeabfrage?}
    D -->|Ja| E[Ersten solchen Konnektor wählen]
    D -->|Nein| F[Einstellung des alten<br/>Wertpapiers übernehmen]
{{< /mermaid >}}

Schüttet das Instrument gemäss den Datenanbietern nichts aus, erhält es keinen Dividenden-Konnektor. Liefert die Suche mehrere Listings, bevorzugt GT jenes, bei dem der historische Konnektor des alten Wertpapiers funktioniert. Name, **Ticker/Symbol**, **Stückelung**, **Ausschüttungshäufigkeit**, **Emittentenland**, **Hyperlink Handelsplatz** und **Produkt Hyperlink** stammen aus den gefundenen Daten; fehlt eine Angabe, übernimmt GT jene des alten Wertpapiers. Währung, Anlageklasse, Handelsplatz und Hebelfaktor entsprechen immer dem alten Wertpapier, und **Handel ab** wird auf das Aktionsdatum gesetzt.

Findet die Suche die neue ISIN nicht, etwa weil die Datenanbieter sie noch nicht führen, kopiert GT das alte Wertpapier. Der Name erhält dann den Zusatz „ (new ISIN)“, und alle vier Konnektoren werden mit ihren URL-Erweiterungen übernommen. Notizen und Watchlist-Zuordnungen werden in keinem Fall übernommen.

In beiden Fällen speichert GT das neue Wertpapier wie der Bearbeitungsdialog: Die Konnektoren werden geprüft, und historische Kurse, Dividenden und Splits werden im Hintergrund geladen. Scheitert diese Prüfung, wird die ISIN-Änderung nicht gespeichert. Erfassen Sie in diesem Fall das neue Wertpapier zuerst selbst; die ISIN-Änderung verweist danach auf dieses.

{{% callout style="warning" title="Connector-Einstellungen prüfen" %}}
Die Probeabfragen sind eine Momentaufnahme, und eine Kopie des alten Wertpapiers übernimmt dessen Einstellungen unverändert. Prüfen Sie nach der Erstellung der ISIN-Änderung, ob das neue Wertpapier tatsächlich historische Kursdaten aufweist und ob die Konnektoren korrekte Daten liefern, besonders wenn der Name auf „ (new ISIN)“ endet. Passen Sie die Einstellungen des neuen Wertpapiers bei Bedarf an.
{{% /callout %}}

{{% callout style="info" title="Performance-Watchlist" %}}
Das neue Instrument wird nicht automatisch zur als Performance gekennzeichneten Watchlist hinzugefügt. Um es hinzuzufügen, öffnen Sie die entsprechende Watchlist und verwenden Sie den Suchdialog **Bestehendes Instrument hinzufügen** mit aktivierter Checkbox **Mit aktivem Bestand**.
{{% /callout %}}

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

#### Bereits durchgeführte ISIN-Änderung
Vor dem Buchen prüft GT für jedes Depot, ob die ISIN-Änderung dort schon durchgeführt wurde. Das ist der Fall, wenn höchstens zehn Tage vor oder nach dem Aktionsdatum der gesamte Bestand des alten Wertpapiers verkauft und das neue Wertpapier im selben Depot gekauft wurde. Die gekaufte Stückzahl muss dem Splitverhältnis entsprechen; eine Abweichung von weniger als einem Stück wird akzeptiert, weil Banken Bruchstücke bar ausgleichen. Für ein solches Depot bucht GT kein zweites Paar, und eine Meldung nennt die Zahl dieser Depots.

Ein Paar, das Sie selbst erfasst haben, bleibt unverändert. Ein Paar mit dem Kommentar «System-Created», das GT früher erzeugt hat und das keiner Anwendung mehr zugeordnet ist, übernimmt GT in die neue Anwendung. Dabei bewertet GT es neu: Der Verkauf erhält den Schlusskurs seines Tages in der damals gehandelten Basis, der Kauf denselben Betrag. Ältere Programmversionen hatten solche Paare mit dem splitbereinigten Kurs gebucht.

{{% callout style="warning" title="Voraussetzung" %}}
Für das Aktionsdatum muss ein Schlusskurs des alten Wertpapiers vorhanden sein, sobald GT ein Paar bucht. Für ein übernommenes Paar braucht es den Schlusskurs an dessen Datum. Fehlt der Kurs, kann die Aktion nicht angewendet werden. War die Änderung in allen Depots bereits durchgeführt, ist kein Kurs nötig.
{{% /callout %}}

### ISIN-Änderung rückgängig machen
Eine bereits angewendete ISIN-Änderung kann über **Rechtsklick** → **ISIN-Änderung rückgängig machen** rückgängig gemacht werden. Dabei werden alle automatisch erzeugten Verkaufs- und Kauftransaktionen gelöscht, auch ein übernommenes Paar. Ein selbst erfasstes Paar bleibt bestehen. Der Status wechselt auf **Rückgängig gemacht**. Die Aktion kann danach erneut angewendet werden.

## Praxisbeispiele
Inverse und gehebelte ETFs verlieren durch den täglichen Reset-Mechanismus und den sogenannten Volatilitätszerfall langfristig an Wert. Wenn der Kurs zu stark fällt, führen Emittenten einen Reverse Split durch, um die Börsenmindestpreise einzuhalten. In den USA erfordert ein Reverse Split eine neue CUSIP-Nummer und damit auch eine neue ISIN, da sich die Wertpapierstruktur wesentlich ändert. Solche ISIN-Änderungen mit gleichzeitigem Split sind ein typischer Anwendungsfall für die GT-Funktion.

### Beispiele: ProShares Inverse-ETFs
| Name | Reverse Split | Wirksam ab | Alte ISIN | Neue ISIN |
|------|---------------|------------|-----------|-----------|
| ProShares Short QQQ (PSQ) | 1:5 | 10.04.2024 | US74347B7148 | US74349Y8378 |
| ProShares Short S&P500 (SH) | 1:4 | 07.11.2024 | US74347B4251 | US74349Y7537 |

### Erfassung in GT
Für das Beispiel **ProShares Short QQQ** würde der Administrator die ISIN-Änderung wie folgt erfassen: Als **Altes Wertpapier** wird das Instrument mit der ISIN US74347B7148 ausgewählt, als **Neue ISIN** US74349Y8378 eingetragen und das **Aktionsdatum** auf den 10.04.2024 gesetzt. Da gleichzeitig ein Reverse Split von 1:5 stattfand, wird **Aus Anzahl** auf 1 und **Wird Anzahl** auf 5 gesetzt. GT erzeugt dann beim Anwenden die entsprechenden Verkaufs- und Kauftransaktionen mit angepasster Stückzahl.

{{% callout style="tip" title="Frühzeitig handeln" %}}
ISIN-Änderungen durch Reverse Splits werden vom Benutzer oft erst spät bemerkt, beispielsweise wenn historische Kursdaten nicht mehr aktualisiert werden oder Portfolioberichte unplausible Werte zeigen. Je länger die Änderung unbemerkt bleibt, desto mehr Transaktionen wurden unter der alten ISIN erfasst, die dann manuell korrigiert werden müssten. Die ISIN-Änderungsfunktion von GT ordnet alle betroffenen Transaktionen automatisch dem neuen Wertpapier zu und erspart so eine aufwändige manuelle Nachbearbeitung.
{{% /callout %}}
