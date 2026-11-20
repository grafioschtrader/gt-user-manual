---
title: "Transaktionsbelege"
date: 2026-07-15T10:00:00+02:00
draft: false
weight: 20
archetype: "default"
---
GT kann für die **Wertpapiertransaktionen** eines **Instrumentes** eigene **Transaktionsbelege** als **PDF-Dokument** erzeugen. Damit lässt sich eine einzelne Transaktion als Dokument weitergeben oder es lassen sich mehrere Transaktionen eines Instrumentes in einem Schritt als Dokumente archivieren. Der Aufbau des Beleges ist an die Abrechnungen von Schweizer Handelsplattformen angelehnt, als Absender tritt Grafioschtrader auf.

## Erzeugen der Belege
Der Menüpunkt "**Transaktionsbelege**" befindet sich im **Kontextmenü** eines selektierten **Instrumentes**, beispielsweise in der [Watchlist](../../watchlistinstrument/watchlist/) oder in anderen Ansichten mit einem Instrument pro Tabellenzeile. Der Menüpunkt wird nur für **Wertpapiere** angeboten, nicht für **Währungspaare**.

Nach dem Aufruf zeigt ein Dialog die Wertpapiertransaktionen des Instrumentes in einer Tabelle mit den Spalten **Datum**, **Transaktionstyp**, **Bankkonto**, **Anzahl**, **Kurs/Div/usw.** und **Totalbetrag**. Je nach Ansicht, aus der die Funktion aufgerufen wird, enthält die Tabelle nur die Transaktionen des entsprechenden **Portfolios** bzw. **Depots**. Über die Auswahlkästchen werden eine oder mehrere Transaktionen markiert. Die Schaltfläche "**Herunterladen**" wird aktiv, sobald mindestens eine Transaktion markiert ist.

Unterstützt werden die Transaktionstypen **Kaufen**, **Verkaufen**, **Zins/Dividende** und **Finanzierungskosten**. Für **Kontotransaktionen** wie Einlagen oder Auszahlungen können keine Belege erzeugt werden.

## Heruntergeladene Dateien
Bei einer einzelnen markierten Transaktion wird der Beleg direkt als **PDF-Dokument** heruntergeladen. Bei mehreren markierten Transaktionen entsteht ein **ZIP-Archiv** mit einem PDF-Dokument pro Transaktion. Der Dateiname eines Beleges setzt sich aus dem Transaktionsdatum und der Transaktionsart zusammen, beispielsweise «20240315_Kauf.pdf». Fallen mehrere markierte Transaktionen auf denselben Tag, wird zusätzlich die Uhrzeit in den Dateinamen eingefügt.

## Inhalt des Beleges
Jeder Beleg umfasst eine Seite. Im Kopfteil stehen der **Nickname** des Benutzers sowie das beteiligte **Depot** und **Konto**. Die Titelzeile nennt die Art der Börsentransaktion zusammen mit einer Referenz aus «GT-» und der Transaktionsnummer. Danach folgen der Name des Wertpapiers mit **ISIN**, die Titel- und Verrechnungswährung, Anzahl und Preis bzw. Ausschüttung pro Einheit, gegebenenfalls **Marchzinsen** bei Anleihen, **Kommission**, **Abgaben und Steuern** sowie der **Devisenkurs** bei unterschiedlichen Währungen. Die hervorgehobene Totalzeile weist den belasteten bzw. gutgeschriebenen Betrag aus.

Die Sprache des Beleges richtet sich nach der eingestellten Sprache des Benutzers: Deutschsprachige Benutzer erhalten einen deutschen Beleg, alle anderen einen englischen. Beim Transaktionstyp **Zins/Dividende** wird auf dem Beleg zwischen **Zins** bei verzinslichen Instrumenten wie Anleihen oder Geldmarkt und **Dividende** bei allen übrigen Instrumenten unterschieden.
