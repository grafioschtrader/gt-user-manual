---
title: "Klient"
date: 2026-08-28T10:00:00+02:00
draft: false
weight: 2
archetype: "default"
---
Der Klient wurde mit dem **Registrierungsprozess** erstellt.

## Bearbeiten Klient
Die Eigenschaften des Klienten wurden im **Registrierungsprozess** erfasst und können über den **Navigationsbereich** auf **Hauptelement Portfolios** angepasst werden.

### Eigenschaften
Alle Eigenschaften eines **Klient** können jederzeit geändert werden.
- **Klientenname**: Dieser **Klientenname** dient der Erkennung, falls in zukünftigen Versionen von GT die **Portfolios** auf Wunsch mit anderen Benutzer geteilt werden können.
- **Währung**: Dies ist die **Währung** des **Klienten**, d.h. die Auswertungen über alle **Portfolios** erfolgt in dieser **Währung**. Wird sie geändert, erzeugt GT die auf die neue Klientenwährung fehlenden [Währungspaare](../../watchlistinstrument/instrument/currencypair/) und baut die Bestandstabellen neu auf. Beides geschieht in einer Hintergrundaufgabe, die Auswertungen sind daher erst nach deren Abschluss vollständig.
- **Zins-/Dividendensteuer übergehen**: In der Schweiz wird bei bestimmten Aktien und Anleihen mit der Dividenden- bzw. Zinszahlung automatisch ein Steuerbetrag von 35% in Abzug gebracht. Dieser Betrag wird aufgrund der Steuerangaben zurück erstattet und die Erträge werden ordentlich als Einkommen versteuert. Falls Sie die Option Zins-/Dividendensteuer übergehen wählen, wird  bei der Gewinnberechnung die Steuer beim Transaktionstyp »Zins/Dividende« übergangen. Somit erhalten Sie eine bessere Vergleichbarkeit der Gewinne von unterschiedlichen Wertpapieren.
- **Geschlossen bis**: In Grafioschtrader können alle Transaktionen jederzeit bearbeitet und vergangene Transaktionen hinzugefügt werden. Diese Flexibilität ist zwar für Korrekturen notwendig, birgt jedoch das Risiko, dass historische Transaktionen versehentlich geändert oder hinzugefügt werden. Benutzer können unbeabsichtigt abgeschlossene Daten aus vergangenen Perioden ändern. Das kann zu folgenden Problemen führen:
- Unerklärliche Berechnungen der historischen Portfolio-Performance.
- Diskrepanzen mit den Kontodaten der Handelsplattform.
- Schwierigkeiten bei der Nachverfolgung, wann Daten tatsächlich geändert wurden und wann Transaktionen stattfanden.
Zur Lösung dieses Problems wurde dieses Datum eingeführt. Dadurch wird das Hinzufügen oder Ändern von Transaktionen in der Vergangenheit unterbunden.  
  - **Konfigurationshierarchie**: Die Portfolioebene hat Vorrang, wenn das Feld „Geschlossen bis” ein Datum aufweist. Die Klientenebene wird als Fallback verwendet, wenn auf Portfolioebene kein Datum gesetzt ist. Es gelten keine Einschränkungen, wenn beide Werte kein Datum enthalten.
  - **Ausnahmen**: Funktionen, die ausschliesslich steuerliche Angaben einer Transaktion betreffen und weder Bestände noch Kontosaldi verändern, sind von dieser Sperre bewusst ausgenommen. Dazu gehören **Steuerstatus umschalten** sowie **Ex-Tag aus Steuerdaten setzen** in der Auswertung [Dividende und Zins](../../reportportfolio/dividends/).
- **Land**: Mit dem **Land** halten Sie fest, in welchem Land der Klient steuerpflichtig ist. Die Angabe ist freiwillig und schaltet die länderspezifischen Steuerfunktionen frei; zurzeit wirkt sich einzig die Auswahl **Schweiz** aus. Ist sie gesetzt, zeigt die Auswertung [Dividende und Zins](../../reportportfolio/dividends/) zusätzlich die Spalten **ICTax Erträge** und **ICTax Steuerwert total**, auf einer Wertpapierzeile das Kontextmenü **Steuerjahr-Korrekturen...** sowie im Menü **Ansicht** die Option **Steuerauszug exportieren** für den eCH-0196-Steuerauszug. Voraussetzung ist, dass ein Administrator zuvor die [Steuerdaten](../../admindata/taxdata/) der Eidgenössischen Steuerverwaltung importiert hat; ohne diese Daten bleiben die Spalten leer. Bleibt das Feld leer oder ist ein anderes Land gewählt, entfallen diese Funktionen ersatzlos; alle übrigen Auswertungen sind davon nicht betroffen.
- **Grafioschtrader-Importvorlagen freischalten**: Mit diesem Auswahlkästchen erlauben Sie, dass dieser **Klient** die mitgelieferte Import Vorlagengruppe «Grafioschtrader» benutzen darf. Ist es gesetzt, lassen sich die eigenen Exporte von GT, also der CSV-Export der Transaktionen und die Transaktionsbelege, wieder einlesen, siehe [Grafioschtrader-Import](../securityaccounts/transactionimport/grafioschtrader/). Welche Vorlagengruppe diese Vorlagen enthält, wählen Sie nicht selbst: das legt ein Administrator einmal für die ganze Installation fest, siehe [Import Vorlagengruppe](../../basedata/imptranstemplate/).

{{% notice note %}}
Solange kein Administrator eine Vorlagengruppe für diese Aufgabe bestimmt hat, erscheint das Auswahlkästchen **Grafioschtrader-Importvorlagen freischalten** gar nicht. Holt er das nach, während Sie angemeldet sind, sehen Sie es erst nach Ihrer nächsten Anmeldung.
{{% /notice %}}

{{% notice note %}}
Das **Land** kann bereits im **Registrierungsprozess** beim Erfassen des Klienten gewählt werden. Bei bestehenden Klienten mit der Währung CHF wurde es beim Update automatisch auf **Schweiz** vorbelegt. Eine Änderung wirkt sich erst beim nächsten Aufbau der Auswertung [Dividende und Zins](../../reportportfolio/dividends/) aus.
{{% /notice %}}

## Funktionen
- **Währung Klient und Portfolios**: Auf dem **Hauptelement Portfolios** gibt es diese Funktion, sie setzt die **Währung** des **Klienten** und all seiner **Portfolios**. Da diese Funktion beide Ebenen gleichzeitig umstellt, werden die fehlenden [Währungspaare](../../watchlistinstrument/instrument/currencypair/) für den Klienten und für jedes einzelne Portfolio erzeugt und anschliessend die Bestandstabellen neu aufgebaut, ebenfalls in einer Hintergrundaufgabe.
