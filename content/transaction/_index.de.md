---
title: "Transaktion"
date: 2021-04-20T02:54:47+01:00
draft: false
weight : 20
chapter: true
---
## Transaktion
Es wird unterschieden zwischen **Transaktionen** die sich nur auf **Bankkonten** beziehen oder solche bei denen ein **Instrument** beteiligt ist. Zudem können Transaktionen teilweise mittels **PDF** oder **CSV importiert** aber sicherlich **manuell erfasst** werden.

### Eigenschaften und Plausibilitäten
Bestimmte Plausibilisierungen werden unabhängig der **Transaktionsart** unterlassen bzw. durchgeführt.  
- Ein Konto kann überzogen werden, d.h. ein negativer Kontostand ist möglich.
- **Transaktionszeit**:
  - Transaktionszeit von Wertpapiertransaktionen sind gemäss dem globalen Handelskalender definierten Handelstagen erlaubt.
  - Bezüglich der Transaktionszeit bei Konto Transaktionen gibt es keine Einschränkung.

### Import von Wertpapiertransaktion als CSV
Für den initialen Import der Transaktionen eines Portfolios ist **CSV** wohl die beste Alternative, vorausgesetzt die **CSV**-Datei enthält auch Angaben wie die **Transaktionskosten** und den **Steuerbetrag** bei den **Wertpapiertransaktionen**. Leider ergibt die Erfahrung, dass viele **Handelsplattformen** nur unvollständige **CSV** liefern. **CSV**-Dateien können nur indirekt über [Transaktionsimport](../tenantportfolio/securityaccounts/transactionimport) zu Transaktion werden, dabei wird wie bei **PDF**-Import eine **Importvorlage** der [Import Vorlagengruppe](../basedata/imptranstemplate/) benötigt. Gundlegende Vorteile von **CSV** sind:
- Eine Anonymisierung ist wahrscheinlich nicht notwendig oder einfach zu bewerkstelligen.
- Alle für eine Transaktion notwendigen Informationen könnten vorliegen.
