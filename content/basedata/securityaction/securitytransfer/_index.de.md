---
title: "Wertpapiertransfer"
date: 2026-11-20T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
Ein Wertpapiertransfer verschiebt ein Wertpapier von einem Depot in ein anderes innerhalb desselben Mandanten. GT erzeugt dabei automatisch eine Verkaufstransaktion im Quelldepot und eine Kauftransaktion im Zieldepot zum Schlusskurs des Transferdatums. Da jede Wertpapiertransaktion ein Bankkonto referenziert, fliesst der Verkaufserlös auf ein Bankkonto im Quellportfolio, während der Kaufbetrag von einem Bankkonto im Zielportfolio abgebucht wird. Ein ebenfalls erzeugter Kontoübertrag zwischen diesen beiden Bankkonten gleicht das aus, sodass die Kontostände nach dem Transfer dieselben sind wie vorher. Diese Funktion ist nützlich, wenn Sie beispielsweise den Broker wechseln oder Bestände zwischen Depots umschichten möchten.

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
    QB->>ZB: Kontoübertrag<br/>(Einheiten × Kurs)
    ZB->>ZD: Kauftransaktion<br/>(Belastung: Einheiten × Kurs)
    Note over QD,ZD: Wertpapier wechselt<br/>vom Quell- ins Zieldepot
{{< /mermaid >}}

{{% callout style="info" title="Kontobewegung" %}}
Verkauf und Kauf referenzieren jeweils ein Bankkonto im eigenen Portfolio. Der Kontoübertrag bringt den Verkaufserlös vom Bankkonto des Quellportfolios auf das Bankkonto des Zielportfolios, das den Kauf bezahlt. Haben die beiden Bankkonten unterschiedliche Währungen, rechnet GT den Übertrag mit dem Wechselkurs des Transferdatums um.
{{% /callout %}}

## Transfer erstellen
Ein Wertpapiertransfer wird direkt aus der Wertpapiertabelle eines Depots heraus initiiert. Klicken Sie mit der **rechten Maustaste** auf die Zeile des gewünschten Wertpapiers → **Wertpapier transferieren**. Weitere Informationen zur Wertpapiertabelle finden Sie unter [Depots]({{% ref "/tenantportfolio/securityaccounts" %}}).

{{% callout style="info" title="Verfügbarkeit" %}}
Der Menüpunkt **Wertpapier transferieren** ist nur verfügbar, wenn das Wertpapier offene Positionen hat (Stückzahl grösser als 0) und kein Marginprodukt ist.
{{% /callout %}}

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
3. Ein **Kontoübertrag** über denselben Betrag bringt den Erlös vom Bankkonto des Quellportfolios auf ein Bankkonto des Zielportfolios.
4. Im Zieldepot wird eine **Kauftransaktion** über dieselbe Stückzahl und denselben Kurs erzeugt. Der Betrag wird diesem Bankkonto im Zielportfolio belastet. GT wählt in beiden Portfolios bevorzugt ein Bankkonto in der Währung des Wertpapiers.
5. Alle erzeugten Transaktionen werden mit dem Transfer verknüpft.
6. Der Transfer erscheint in der [Wertpapieraktion]({{% ref "/basedata/securityaction" %}})-TreeTable unter dem Knoten **Eigene Transfers**.

{{% callout style="warning" title="Voraussetzung" %}}
Für das Transferdatum muss ein Schlusskurs des Wertpapiers vorhanden sein. Fehlt dieser, kann der Transfer nicht durchgeführt werden.
{{% /callout %}}

## Transfer rückgängig machen
Ein durchgeführter Transfer kann in der [Wertpapieraktion]({{% ref "/basedata/securityaction" %}})-TreeTable rückgängig gemacht werden. Navigieren Sie zum Knoten **Eigene Transfers**, klicken Sie mit der **rechten Maustaste** auf den entsprechenden Transfer → **Transfer rückgängig machen**. Dabei werden alle automatisch erzeugten Transaktionen (Verkauf, Kontoübertrag und Kauf) sowie der Transfereintrag selbst gelöscht.

Eine Rückgängigmachung ist nur möglich, wenn im Zieldepot nach dem Transferdatum keine Transaktionen auf dem transferierten Wertpapier erfasst wurden. Existieren solche Transaktionen (z. B. Dividenden oder weitere Käufe), ist der Menüpunkt **Transfer rückgängig machen** deaktiviert. Um den Transfer dennoch rückgängig zu machen, müssen zuerst die späteren Transaktionen im Zieldepot gelöscht werden.
