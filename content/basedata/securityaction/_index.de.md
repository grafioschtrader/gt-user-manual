---
title: "Wertpapieraktion"
date: 2026-03-08T22:54:47+01:00
draft: false
weight: 65
archetype: "default"
---
Die Wertpapieraktion verwaltet zwei Anwendungsfälle, bei denen GT automatisch zusammengehörige Verkaufs- und Kauftransaktionen erzeugt: **ISIN-Änderungen** und **Wertpapiertransfers**. Beide Szenarien erfordern manuelle Korrekturen an den Beständen, die ohne diese Funktion fehleranfällig und aufwändig wären.

{{% notice style="info" title="Künstliche Transaktionen" %}}
Bei beiden Anwendungsfällen erzeugt das System Verkaufs- und Kauftransaktionen für den Mandanten. Dies ist eine Ausnahme vom Grundsatz, dass nur der Benutzer selbst Transaktionen erstellt, und stellt keine ideale Lösung dar. In der Schweiz haben diese künstlichen Transaktionen jedoch keinen Einfluss auf die Steuerberechnung. Da GT die Steuerberechnung derzeit nur für die Schweiz unterstützt, ist dies in der Praxis kein Problem.
{{% /notice %}}

## Übersicht
Die Wertpapieraktionen werden als **TreeTable** unter **Basisdaten** im Navigationsbaum angezeigt. Der Baum hat zwei Wurzelknoten:
+ **Systemaktionen (ISIN-Änderungen)**: Vom Administrator erstellte, systemweite Aktionen, wenn ein Wertpapier eine neue ISIN erhält. Alle betroffenen Mandanten können die Aktion auf ihre Bestände anwenden.
+ **Eigene Transfers**: Vom Benutzer initiierte Transfers eines Wertpapiers von einem Depot in ein anderes innerhalb desselben Mandanten.

## Spalten der TreeTable
| Spalte | Beschreibung |
|--------|--------------|
| **Name** | Bezeichnung der Aktion bzw. des betroffenen Wertpapiers. |
| **Alte ISIN** | Die bisherige ISIN des Wertpapiers (bei ISIN-Änderungen). |
| **Neue ISIN** | Die neue ISIN des Wertpapiers (bei ISIN-Änderungen). |
| **Aktionsdatum** | Datum, an dem die Aktion wirksam wird. Der Schlusskurs dieses Datums wird für die erzeugten Transaktionen verwendet. |
| **Aus Anzahl** | Splitfaktor-Zähler — gibt an, wie viele alte Anteile umgewandelt werden (z.B. 1 bei einer reinen ISIN-Änderung). |
| **Wird Anzahl** | Splitfaktor-Nenner — gibt an, wie viele neue Anteile daraus entstehen (z.B. 1 bei einer reinen ISIN-Änderung, 10 bei einem 1:10-Split). |
| **Betroffen** | Anzahl der betroffenen Mandanten (bei ISIN-Änderungen). |
| **Angewendet** | Anzahl der Mandanten, die die Aktion bereits angewendet haben. |
| **Status** | Aktueller Status der Aktion für den eingeloggten Benutzer. |
| **Einheiten** | Anzahl der transferierten Einheiten (bei Transfers). |
| **Kurs** | Verwendeter Kurs für die erzeugten Transaktionen. |

## Statuswerte
+ **Nicht angewendet**: Die Aktion wurde noch nicht auf die Bestände des Benutzers angewendet.
+ **Angewendet**: Die Aktion wurde angewendet und die entsprechenden Transaktionen wurden erzeugt.
+ **Rückgängig gemacht**: Die Aktion wurde nach der Anwendung wieder rückgängig gemacht, die erzeugten Transaktionen wurden gelöscht.

## Detailseiten
+ [ISIN-Änderung]({{% ref "/basedata/securityaction/securityisinrename" %}}): Beschreibt den Ablauf bei einer Umbenennung der ISIN eines Wertpapiers.
+ [Wertpapiertransfer]({{% ref "/basedata/securityaction/securitytransfer" %}}): Beschreibt den Transfer eines Wertpapiers zwischen Depots.
