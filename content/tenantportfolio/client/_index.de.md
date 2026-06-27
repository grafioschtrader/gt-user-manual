---
title: "Klient"
date: 2026-01-06T22:54:47+01:00
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
- **Währung**: Dies ist die **Währung** des **Klienten**, d.h. die Auswertungen über alle **Portfolios** erfolgt in dieser **Währung**.
- **Zins-/Dividendensteuer übergehen**: In der Schweiz wird bei bestimmten Aktien und Anleihen mit der Dividenden- bzw. Zinszahlung automatisch ein Steuerbetrag von 35% in Abzug gebracht. Dieser Betrag wird aufgrund der Steuerangaben zurück erstattet und die Erträge werden ordentlich als Einkommen versteuert. Falls Sie die Option Zins-/Dividendensteuer übergehen wählen, wird  bei der Gewinnberechnung die Steuer beim Transaktionstyp »Zins/Dividende« übergangen. Somit erhalten Sie eine bessere Vergleichbarkeit der Gewinne von unterschiedlichen Wertpapieren.
- **Geschlossen bis**: In Grafioschtrader können alle Transaktionen jederzeit bearbeitet und vergangene Transaktionen hinzugefügt werden. Diese Flexibilität ist zwar für Korrekturen notwendig, birgt jedoch das Risiko, dass historische Transaktionen versehentlich geändert oder hinzugefügt werden. Benutzer können unbeabsichtigt abgeschlossene Daten aus vergangenen Perioden ändern. Das kann zu folgenden Problemen führen:
- Unerklärliche Berechnungen der historischen Portfolio-Performance.
- Diskrepanzen mit den Kontodaten der Handelsplattform.
- Schwierigkeiten bei der Nachverfolgung, wann Daten tatsächlich geändert wurden und wann Transaktionen stattfanden.
Zur Lösung dieses Problems wurde dieses Datum eingeführt. Dadurch wird das Hinzufügen oder Ändern von Transaktionen in der Vergangenheit unterbunden.  
  - **Konfigurationshierarchie**: Die Portfolioebene hat Vorrang, wenn das Feld „Geschlossen bis” ein Datum aufweist. Die Klientenebene wird als Fallback verwendet, wenn auf Portfolioebene kein Datum gesetzt ist. Es gelten keine Einschränkungen, wenn beide Werte kein Datum enthalten.

## Funktionen
- **Währung Klient und Portfolios**: Auf dem **Hauptelement Portfolios** gibt es diese Funktion, sie setzt die **Währung** des **Klienten** und all seiner **Portfolios**.
