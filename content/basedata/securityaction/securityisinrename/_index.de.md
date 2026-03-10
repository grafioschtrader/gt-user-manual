---
title: "ISIN-Änderung"
date: 2026-03-08T22:54:47+01:00
lastmod: 2026-03-08T22:54:47+01:00
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
    S->>B: Benachrichtigung<br/>per interner Mail
    B->>S: Wendet ISIN-Änderung an
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

Nach dem Speichern referenziert GT das bestehende Wertpapier oder erstellt ein neues mit der angegebenen ISIN, zählt die betroffenen Mandanten und sendet ihnen eine interne Mailbenachrichtigung über die verfügbare ISIN-Änderung.

### Löschen einer ISIN-Änderung
Der Administrator kann eine ISIN-Änderung über **Rechtsklick** → **ISIN-Änderung löschen** wieder entfernen. Dies ist nur möglich, solange kein Mandant die Aktion angewendet hat.

## Benutzer-Ablauf

### ISIN-Änderung anwenden
Hat ein Administrator eine ISIN-Änderung erstellt, sieht jeder betroffene Mandant die Aktion in seiner TreeTable mit dem Status **Nicht angewendet**. Um die Änderung anzuwenden: **Rechtsklick** auf die entsprechende Zeile → **ISIN-Änderung anwenden**.

GT führt dabei folgende Schritte aus:
1. Alle Transaktionen des Mandanten, die das alte Wertpapier betreffen, werden ermittelt.
2. Der Schlusskurs des alten Wertpapiers am Aktionsdatum wird abgerufen.
3. Für jedes betroffene Depot wird eine **Verkaufstransaktion** für das alte Wertpapier und eine **Kauftransaktion** für das neue Wertpapier erzeugt.
4. Sind Splitfaktoren gesetzt (Aus Anzahl / Wird Anzahl ungleich 1:1), werden die Stückzahl und der Kurs auf der Kaufseite entsprechend angepasst.
5. Der Status wechselt auf **Angewendet**.

{{% notice style="warning" title="Voraussetzung" %}}
Für das Aktionsdatum muss ein Schlusskurs des alten Wertpapiers vorhanden sein. Fehlt dieser, kann die Aktion nicht angewendet werden.
{{% /notice %}}

### ISIN-Änderung rückgängig machen
Eine bereits angewendete ISIN-Änderung kann über **Rechtsklick** → **ISIN-Änderung rückgängig machen** rückgängig gemacht werden. Dabei werden alle automatisch erzeugten Verkaufs- und Kauftransaktionen gelöscht. Der Status wechselt auf **Rückgängig gemacht**. Die Aktion kann danach erneut angewendet werden.
