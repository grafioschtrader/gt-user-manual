---
title: "Transaktion"
date: 2021-04-20T02:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
Es wird unterschieden zwischen **Transaktionen** die sich nur auf **Bankkonten** beziehen oder solche bei denen ein **Instrument** beteiligt ist. Zudem können Transaktionen teilweise mittels **PDF** oder **CSV importiert** aber sicherlich **manuell erfasst** werden.

## Eigenschaften und Plausibilitäten
Bestimmte Plausibilisierungen werden unabhängig der **Transaktionsart** unterlassen bzw. durchgeführt.  
- Ein Konto kann überzogen werden, d.h. ein negativer Kontostand ist möglich.
- **Transaktionszeit**:
  - Transaktionszeit von Wertpapiertransaktionen sind gemäss dem globalen Handelskalender definierten Handelstagen erlaubt.
  - Bezüglich der Transaktionszeit bei Konto Transaktionen gibt es keine Einschränkung.

## Import von Wertpapiertransaktion als CSV
Für den initialen Import der Transaktionen eines Portfolios ist **CSV** wohl die beste Alternative, vorausgesetzt die **CSV**-Datei enthält auch Angaben wie die **Transaktionskosten** und den **Steuerbetrag** bei den **Wertpapiertransaktionen**. Leider ergibt die Erfahrung, dass viele **Handelsplattformen** nur unvollständige **CSV** liefern. **CSV**-Dateien können nur indirekt über [Transaktionsimport](../tenantportfolio/securityaccounts/transactionimport) zu Transaktion werden, dabei wird wie bei **PDF**-Import eine **Importvorlage** der [Import Vorlagengruppe](../basedata/imptranstemplate/) benötigt. Gundlegende Vorteile von **CSV** sind:
- Eine Anonymisierung ist wahrscheinlich nicht notwendig oder einfach zu bewerkstelligen.
- Alle für eine Transaktion notwendigen Informationen könnten vorliegen.

## Genauigkeit von Beträgen und Kursen
Für das Zahlenformat von Beträgen und Kursen wurde ein Kompromiss zwischen den Währungen Iranischer Rial und Bitcoin gefunden. Für den Iranischen Rial werden die Vorkommastellen und für Bitcoin die Nachkommastellen verwendet. 

#### Vorkommastellen
Es sind maximal 14 Vorkommastellen für Beträge und Kurse möglich. Dies sollte für die Darstellung der Währung Iranischer Rial ausreichend sein.

#### Nachkommastellen
Grundsätzlich werden Beträge und Kurse intern mit 8 Nachkommastellen gespeichert. Dies kann zu kleinen Ungenauigkeiten führen. Ein reales Beispiel: Es gibt einen Zins von 1355.70 für die Menge von 1300 Anteilen, dies ergibt einen Zins von »1.04284615384615...« pro Anteil. In GT kann nur 1.04284615 eingegeben und gespeichert werden. Die Berechnung 1.04284615 x 1300 ergibt 1355.699995. Diese kleine Ungenauigkeit wird in der aktuellen GT Version noch sichtbar dargestellt.

#### Unterschiedliche Genauigkeiten bei Währungen
Die meisten Währungen haben eine Unterteilung von 100, wie beispielsweise der CHF, EUR, USD. Der Kuwait-Dinar ist eine Tausenderwährung, somit wird eine Genauigkeit von 3 Nachkommastellen erwartet. Bei Kryptowährungen wie dem Bitcoin werden 8 Nachkommastellen erwartet. Schliesslich gibt es auch Währungen die auf Nachkommastellen verzichten, dazu gehört der Japanische Yen. In der aktuellen Version kann GT dies noch nicht korrekt abbilden. 

{{% notice style="info" title="Automatische Korrektur von Wechselkurs oder Kurs/Div/usw" %}}
Es kann vorkommen, dass in den Transaktionsbelegen gerundete oder gekürzte Nachkommastellen angegeben sind. Aufgrund der Nachkommastellen einer Währung versucht das GT-Backend, den gewünschten Betrag der Kontobuchung möglichst durch Anpassung des Wechselkurses bzw. des Kurses zu erreichen. So wird z.B. aus der Kontobuchung CHF 2887.84995323 ein Betrag von CHF 2887.85 durch Anpassung des Wechselkurses, falls vorhanden, oder sonst durch Anpassung von Kurs/Div/usw. angestrebt. In der Realität wird in diesem Beispiel möglicherweise ein Betrag von CHF 2887.84999998 erreicht.
{{% /notice %}}