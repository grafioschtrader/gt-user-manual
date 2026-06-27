---
title: "Watchlist"
date: 2025-11-18T10:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
Im Hauptbereich sind **4 Registerkarten** für die unterschiedlichen **Wachlisten-Ansichten** ersichtlich, diese unterscheiden sich primär durch ihren Inhalt, wobei sich die ersten Spalten der Tabelle zur Identifizierung des Instruments nicht unterscheiden.
+ **Performance**: Diese gibt einen Überblick der Kursdaten und der **Performance** der **offenen Positionen**. Es ist der beste Ort, um Wertpapier-Transaktionen zu erfassen, zu bearbeiten und zu löschen.
+ **Kurs Datenfeed**: Sie dient der Überwachung der Zuverlässigkeit der **Datenquellen von Kursdaten**.
+ **Zusatzfeld**: Hier können benutzerdefinierte Zusatzfelder angezeigt werden.
+ **Dividenden/Split Feed**: Damit erhalten Sie Informationen hinsichtlich von **Dividenden uns Splits**.

## Funktionen Navigationsbereich
Das Erstellen und Löschen einer Watchlist erfolgt im **Navigationsbereich**.

### Watchlist erstellen
Die Watchlist wird mittels dem **Kontextmenü** auf dem statischen Element **Watchlist** im **Navigationsbereich** erstellt. Es gibt einen Beschränkung bezüglich der Anzahl Watchlisten die ein Benutzer erstellen kann.

### Watchlist löschen
Die Watchlist wird mittels dem **Kontextmenü** auf dem statischen Element **Watchlist** im **Navigationsbereich** gelöscht. Dabei darf die Watchlist keine Instrumente enthalten.

### Performance Watchlist {{< svg "chart-line.svg" svg-icon-size >}}
Im **Navigationsbereich** ist einer bestimmten Watchlist das **Performance** Watchlist gekennzeichnet. Diese Watchlist sollte alle Ihre offenen Positionen enthalten. Dadurch erfolgt automatisch eine Aktualisierung der abhängigen **Währungspaare**.

## Allgemeine Funktionen der Watchlist-Ansichten
Es gibt Funktionen die in allen Watchlisten implementiert sind. Anderseits unterscheiden sich die Tabellenspalten der Watchlisten, daher kann es pro Watchlist unterschiedliche Funktionen geben. Im folgenden werden die Funktionen behandelt die in allen Ansichten verfügbar sind.

### Funktionen markierte Watchlist-Ansicht
Diese Funktionen sind auf der markierten Watchlist anwendbar.
- **Instrument in andere Watchlist verschieben**: Ein Instrument kann mit Drag and Drop auf eine andere Watchlist verschoben werden. Dazu ziehen Sie das Symbol der **Instrumentenart** auf Ihre gewählte **Watchlist** im **Navigationsbereich**.
- **Bestehendes Instrument hinzufügen**: In einer aktiven Watchlist des **Hauptbereiches** kann über das **Kontextmenü** ein bestehendes Instrument hinzugefügt werden. Hierzu öffnet sich ein der entsprechende [Suchdialog für Instrumente](../instrument/searchdialog).
  + Mit der Schaltfläche **Hinzufügen** werden die selektierten Instrumente der Watchlist hinzugefügt.
- **Hinzufügen neues Wertpapier**: Damit wird ein neues **Wertpapier** erstellt und der Watchlist hinzugefügt, weitere Information siehe [Wertpapier und abgeleitetes Instrument](../instrument/securityderived).
- **Hinzufügen neues abgeleitetes Instrument**: Mit dieser Funktion wird ein **abgeleitetes Instrument** erstellt und der Watchlist hinzugefügt, weitere Information siehe [Wertpapier und abgeleitetes Instrument](../instrument/securityderived).
- **Hinzufügen neu erstelltes Währungspaar**: Damit wird ein neues **Währungspaar** erstellt und der Watchlist hinzugefügt, weitere Information siehe [Währungspaar](../instrument/currencypair).

### Funktionen selektiertes Instrument
Diese Funktion können auf einem selektierten Instrument angewendet werden. Einige dieser Funktionen sind selbsterklärend und haben möglicherweise ein Hilfstext beim Menü.
- **Instrument entfernen**: Hiermit kann ein einzelnes selektiertes Instrument aus der Watchlist entfernt werden.
- **Innertag Kursaktualisierung**: Damit wird der Innertag Kurs eines selektierten Instrumentes aktualisiert, wobei der **Zeitabstand** seit der letzten Aktualisierung nicht übersteuert werden kann.
- **Bearbeite Zusatzfeld**: Über diesen Dialog können die selbstdefinierten Zusatzfelder mit Inhalt gefüllt werden. 
- **Kauf, Verkaufen, Zins/Dividende**: Hierzu gibt es weitere Informationen unter [Wertpapiertransaktion](../../transaction/security).
- **Tagesdaten als Liniengrafik oder Tabelle**: Diese Gruppe von Funktionen ist nicht auf die Watchlist beschränkt, daher siehe [Tagesendkurse als Linengrafik](../eodchart).
- **Hyperlink Handelsplatz und Produkt**: Siehe dazu [Wertpapier und abgeleitetes Instrument](../instrument/securityderived/).
- **Innertag- und Historische Daten-Hyperlink**: Diese beiden Hyperlinks sind unterstützend bei der Überprüfung von Konnektoren-Einstellung.
  - Bei gewissen Datenquellen wird nicht eine zusätzliche Webseite öffnet, sondern eine Datei herunter geladen wird.
  - Bei Datenquellen mit einem API-Key ist das Verhalten dieser Funktionen für einen Benutzer mit Administratorrechten anders als für alle anderen Benutzer. Für den Administrator wird der Link im Browser ausgeführt, während für die anderen Benutzer die Ausführung im Backend erfolgt und anschließend das Ergebnis vom Frontend in einem neuen Webbrowser-Tab angezeigt wird. 
- **Nachricht an Ersteller**: Damit kann eine Nachricht an den Ersteller dieses Instruments gesendet werden, weiter Informationen zum siehe [Nachrichtensystem](../../admindata).

### Menü “Ansicht” der Menüleiste
**Spalten anzeigen**: Zur besseren Übersicht können einzelne Spalten ein- oder ausgeblendet werden. Je nach Anzeigegerät kann so die Darstellung optimiert werden. Diese Anpassung bleibt über das Sitzungsende hinweg erhalten.

## Eigenschaften und Tabellenspalten
Spalten werden nicht weiter beschrieben falls diese **Instrument** bzw. in **Anlageklasse** erwähnt wurden. Einige Spalten sind selbsterklärend oder dessen Quickinfo gibt ausreichend Erklärung. Bestimmte Tabellenspalten können ein- und ausblenden werden, siehe dazu [Bedienelemte](../../intro/userinterface/user_setting_ui_controls).

- **Instrumentenart (I)**: Die Symbole der Instrumente können der [Anlageklasse](../../basedata/assetclass) entnommen werden. {{< svg "d.svg" svg-icon-size >}}: Dies steht zusätzlich für ein abgeleitetes Instrument.
