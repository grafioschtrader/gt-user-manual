---
title: "Wertpapiertransfer"
date: 2026-03-09T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
Ein Wertpapiertransfer verschiebt ein Wertpapier von einem Depot in ein anderes innerhalb desselben Mandanten. GT erzeugt dabei automatisch eine Verkaufstransaktion im Quelldepot und eine Kauftransaktion im Zieldepot zum Schlusskurs des Transferdatums. Da jede Wertpapiertransaktion ein Bankkonto referenziert, fliesst der Verkaufserlös auf ein Bankkonto im Quellportfolio, während der Kaufbetrag von einem Bankkonto im Zielportfolio abgebucht wird. Diese Funktion ist nützlich, wenn Sie beispielsweise den Broker wechseln oder Bestände zwischen Depots umschichten möchten.

Das folgende Diagramm zeigt den Ablauf eines Wertpapiertransfers und die dabei betroffenen Konten:

{{< mermaid >}}
sequenceDiagram
    participant B as Benutzer
    participant QD as Quelldepot
    participant QB as Bankkonto<br/>(Quellportfolio)
    participant ZD as Zieldepot
    participant ZB as Bankkonto<br/>(Zielportfolio)

    B->>QD: Rechtsklick auf Wertpapier<br/>→ Wertpapier transferieren
    Note over QD,ZB: GT ermittelt Schlusskurs am Transferdatum
    QD->>QB: Verkaufstransaktion<br/>(Gutschrift: Einheiten × Kurs)
    ZB->>ZD: Kauftransaktion<br/>(Belastung: Einheiten × Kurs)
    Note over QD,ZD: Wertpapier wechselt<br/>vom Quell- ins Zieldepot
{{< /mermaid >}}

{{% notice style="info" title="Kontobewegung" %}}
Da Verkauf und Kauf jeweils ein eigenes Bankkonto im jeweiligen Portfolio referenzieren, entsteht im Quellportfolio eine Gutschrift und im Zielportfolio eine Belastung. Falls gewünscht, kann dieser Saldo anschliessend manuell über einen Kontotransfer zwischen den Bankkonten ausgeglichen werden.
{{% /notice %}}

## Transfer erstellen
Ein Wertpapiertransfer wird direkt aus der Wertpapiertabelle eines Depots heraus initiiert. Klicken Sie mit der **rechten Maustaste** auf die Zeile des gewünschten Wertpapiers → **Wertpapier transferieren**. Weitere Informationen zur Wertpapiertabelle finden Sie unter [Depots]({{% ref "/tenantportfolio/securityaccounts" %}}).

{{% notice style="info" title="Verfügbarkeit" %}}
Der Menüpunkt **Wertpapier transferieren** ist nur verfügbar, wenn das Wertpapier offene Positionen hat (Stückzahl grösser als 0) und kein Marginprodukt ist.
{{% /notice %}}

### Dialogfelder
| Feld | Bearbeitbar | Beschreibung |
|------|-------------|--------------|
| **Wertpapier** | Nein | Name des zu transferierenden Wertpapiers (schreibgeschützt). |
| **Quelldepot** | Nein | Das Depot, aus dem das Wertpapier transferiert wird (schreibgeschützt). |
| **Zielportfolio** | Ja | Dropdown mit den verfügbaren Portfolios. Es werden nur Portfolios angezeigt, die mindestens ein geeignetes Zieldepot enthalten. |
| **Zieldepot** | Ja | Dropdown mit den Depots des gewählten Portfolios. Das Quelldepot ist ausgeschlossen. Die Auswahl aktualisiert sich automatisch bei Änderung des Zielportfolios. |
| **Transferdatum** | Ja | Datum, an dem der Transfer durchgeführt wird. Der Schlusskurs dieses Datums wird für die erzeugten Transaktionen verwendet. Das Transferdatum muss **nach** der letzten vorhandenen Transaktion auf diesem Wertpapier im Quelldepot liegen. Wird ein früheres Datum eingegeben, lehnt GT den Transfer ab und zeigt das Datum der letzten Transaktion an. |
| **Bemerkung** | Ja | Optionale Notiz zum Transfer. |

### Was passiert beim Transfer
Nach dem Speichern führt GT folgende Schritte aus:
1. Der Schlusskurs des Wertpapiers am Transferdatum wird abgerufen.
2. Im Quelldepot wird eine **Verkaufstransaktion** über alle gehaltenen Einheiten zum Schlusskurs erzeugt. Der Erlös wird einem Bankkonto im Quellportfolio gutgeschrieben.
3. Im Zieldepot wird eine **Kauftransaktion** über dieselbe Stückzahl und denselben Kurs erzeugt. Der Betrag wird einem Bankkonto im Zielportfolio belastet. GT wählt bevorzugt ein Bankkonto in der Währung des Wertpapiers.
4. Beide Transaktionen werden miteinander verknüpft.
5. Der Transfer erscheint in der [Wertpapieraktion]({{% ref "/basedata/securityaction" %}})-TreeTable unter dem Knoten **Eigene Transfers**.

{{% notice style="warning" title="Voraussetzung" %}}
Für das Transferdatum muss ein Schlusskurs des Wertpapiers vorhanden sein. Fehlt dieser, kann der Transfer nicht durchgeführt werden.
{{% /notice %}}

## Transfer rückgängig machen
Ein durchgeführter Transfer kann in der [Wertpapieraktion]({{% ref "/basedata/securityaction" %}})-TreeTable rückgängig gemacht werden. Navigieren Sie zum Knoten **Eigene Transfers**, klicken Sie mit der **rechten Maustaste** auf den entsprechenden Transfer → **Transfer rückgängig machen**. Dabei werden beide automatisch erzeugten Transaktionen (Verkauf und Kauf) sowie der Transfereintrag selbst gelöscht.

Eine Rückgängigmachung ist nur möglich, wenn im Zieldepot nach dem Transferdatum keine Transaktionen auf dem transferierten Wertpapier erfasst wurden. Existieren solche Transaktionen (z. B. Dividenden oder weitere Käufe), ist der Menüpunkt **Transfer rückgängig machen** deaktiviert. Um den Transfer dennoch rückgängig zu machen, müssen zuerst die späteren Transaktionen im Zieldepot gelöscht werden.
