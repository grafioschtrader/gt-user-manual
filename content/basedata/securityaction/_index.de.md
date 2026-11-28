---
title: "Wertpapieraktion"
date: 2026-11-20T22:54:47+01:00
draft: false
weight: 65
archetype: "default"
---
Die Wertpapieraktion verwaltet drei Anwendungsfälle, bei denen GT automatisch zusammengehörige Verkaufs- und Kauftransaktionen erzeugt: **ISIN-Änderungen**, **Abspaltungen** und **Wertpapiertransfers**. Alle drei erfordern Korrekturen an den Beständen, die von Hand fehleranfällig und aufwändig wären.

{{% callout style="info" title="Künstliche Transaktionen" %}}
Bei allen drei Anwendungsfällen erzeugt das System Verkaufs- und Kauftransaktionen für den Mandanten. Dies ist eine Ausnahme vom Grundsatz, dass nur der Benutzer selbst Transaktionen erstellt. Die Auswertungen erkennen diese Systembuchungen: Die [Steuerhilfe Deutschland]({{% ref "/reportportfolio/germantax" %}}) führt bei einer ISIN-Änderung und einem Wertpapiertransfer die Käufe mit ihrem Kaufdatum und Einstand weiter, ohne einen Gewinn auszuweisen, und eine Abspaltung teilt den Einstand auf, statt einen Verkauf zu rechnen. In der Schweiz haben diese Transaktionen keinen Einfluss auf die Steuerberechnung.
{{% /callout %}}

{{% callout style="info" title="Transaktionslimit" %}}
Da ISIN-Änderung, Abspaltung und Wertpapiertransfer Transaktionen erzeugen, werden sie abgelehnt, wenn der Klient das [Transaktionslimit pro Klient](../../transaction/) bereits erreicht hat. Liegt der Bestand noch darunter, wird die Aktion vollständig ausgeführt, auch wenn die Grenze dadurch geringfügig überschritten wird.
{{% /callout %}}

## Übersicht
Die Wertpapieraktionen werden als **TreeTable** unter **Basisdaten** im Navigationsbaum angezeigt. Der Baum hat zwei Wurzelknoten:
+ **Systemaktionen**: Vom Administrator erstellte, systemweite Aktionen. Eine **ISIN-Änderung** erfasst er, wenn ein Wertpapier eine neue ISIN erhält, eine **Abspaltung**, wenn die Aktionäre eines Unternehmens Anteile an einem neuen Unternehmen erhalten. Alle betroffenen Mandanten können die Aktion auf ihre Bestände anwenden.
+ **Eigene Transfers**: Vom Benutzer initiierte Transfers eines Wertpapiers von einem Depot in ein anderes innerhalb desselben Mandanten.

## Spalten der TreeTable
| Spalte | Beschreibung |
|--------|--------------|
| **Name** | Bezeichnung der Aktion bzw. des betroffenen Wertpapiers. |
| **Aktionstyp** | **ISIN-Änderung** oder **Abspaltung** (bei Systemaktionen). |
| **Einstandsaufteilung** | Anteil des Einstands, der auf das abgespaltene Wertpapier übergeht (bei Abspaltungen). |
| **Kursbasis** | Nur für Administratoren: ob die Kursquelle die Kurse des Mutterwertpapiers vor dem Ex-Tag für die Abspaltung bereinigt hat. **Ungeklärt** verlangt eine Prüfung, siehe [Kursbasis]({{% ref "/basedata/securityaction/spinoff" %}}#HISTORYBASIS). |
| **Alte ISIN** | Die bisherige ISIN des Wertpapiers, bei einer Abspaltung die ISIN des Mutterwertpapiers. |
| **Neue ISIN** | Die neue ISIN des Wertpapiers, bei einer Abspaltung die ISIN des abgespaltenen Wertpapiers. |
| **Aktionsdatum** | Datum, an dem die Aktion wirksam wird, bei einer Abspaltung der Ex-Tag. Der Schlusskurs dieses Datums wird für die erzeugten Transaktionen verwendet. |
| **Aus Anzahl** | Splitfaktor-Zähler — gibt an, wie viele alte Anteile umgewandelt werden (z.B. 1 bei einer reinen ISIN-Änderung). Bei einer Abspaltung die Anzahl Anteile des Mutterwertpapiers. |
| **Wird Anzahl** | Splitfaktor-Nenner — gibt an, wie viele neue Anteile daraus entstehen (z.B. 1 bei einer reinen ISIN-Änderung, 10 bei einem 1:10-Split). Bei einer Abspaltung die Anzahl Anteile, die dafür zugeteilt werden. |
| **Betroffen** | Anzahl der betroffenen Mandanten (bei Systemaktionen). |
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
+ [Abspaltung]({{% ref "/basedata/securityaction/spinoff" %}}): Beschreibt das Erfassen, Anwenden und Rückgängigmachen einer Abspaltung und die Aufteilung des Einstands.
+ [Wertpapiertransfer]({{% ref "/basedata/securityaction/securitytransfer" %}}): Beschreibt den Transfer eines Wertpapiers zwischen Depots.
