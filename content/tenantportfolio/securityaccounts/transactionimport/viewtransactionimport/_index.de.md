---
title: "Ansicht Transaktionsimport"
date: 2021-03-30T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---

**Transaktionsimport** ist eine **Zwischenstufe** nach dem Import und bevor der Import zur Transaktion wird. Nur der erfolgreiche Import mit **Drag & Drop** benötigt die **Ansicht Transaktionsimport** nicht. Alle anderen Arten des Importes werden über diese **Ansicht** erledigt. Diese Ansicht gibt Aufschluss über den **Status** einer potenziellen Transaktion. Zudem können gewisse Korrekturen an **Importposition** angebracht werden. Unterschiedliche Imports derselben potenziellen Transaktionen können diese qualitativ verbessern. Letztendlich wird in dieser Ansicht die **Importposition** zu einer **realen Transaktion** verarbeitet.

### Erstellen und bearbeiten Importgruppe
Es kann mehrere **Importgruppen** geben, diese werden vom Benutzer oder beim fehlgeschlagenen **Drag & Drop** von **PDF** durch dsa System erzeugt.
- **Erstellen Importgruppe**: Erstellen Sie eine neue Importgruppe. Nebst dem Namen kann auch eine Bemerkung angebracht werden. Innerhalb eines **Depot** muss der **Name** einzigartig sein.
- **Bearbeiten Importgruppe**: Name und Bemerkung können geändert werden.
- **Löschen Importgruppe**: Falls es keine **Importpositionen** innerhalb einer **Importgruppe** mehr gibt, kann diese  gelöscht werden.

### Voraussetzung für die Transaktionsfähigkeit
Das System führt Zuordnungen und Berechnungen aufgrund der importierten Werte durch. Eine **Importposition** wird **Transaktionsfähig**, falls folgende Schritte erfolgreicht durchgeführt werden können.
- **Wertpapier Zuordnung**: Aufgrund der **ISIN** oder des **Symbol/Ticker** geschieht die Zuordnung eines bestehenden Wertpapieres.
- **Bankkonto Zuordnung**: Mit der Angabe des Wertes von **Feld "cac"**, siehe [Importvorlage](../../../basedata/imptranstemplate/createimptranstemplate/), geschieht die Zuordnung eines existierenden Bankkontos dieses Portfolios.
- **Gesamtsumme Überprüfung**: Aus den Angaben der importierten Transaktion wird die **Gesamtsumme** errechnet, diese muss mit dem entsprechenden Wert des **Feld "ta"** übereinstimmen.

Ein Teil der angebotenten Funktionen auf der **Importposition** können helfen, die oben genannten Bedienungen zu erreichen.

### Eigenschaften und Tabellenspalten
Bestimmte Tabellenspalten können ein- und ausgeblendet werden, siehe dazu [Bedienelemte](../../../intro/userinterface/user_setting_ui_controls).
- **Hat Transaktion**: Die Importposition wurde schon zu einer Transaktion verarbeitet. 
- **Hat vielleicht Transaktion**: Aufgrund des **Datums der Transaktion**, **Anzahl** und das **Transaktionstype** erkannte das System eine übereinstimmente existierende Transaktion im aktuellen Depot. Es ist nicht möglich, für eine solche Importposition eine Transaktion zu erzeugen.

### Funktionen markierte Transaktionsimport-Ansicht
Es gibt Funktionen die losgelöst von einer in der Tabelle ausgewählten Zeile funktionieren, dies sind die alle Funktionen für das Hochladen von Dateien und die oben beschriebenen Funktionen bezüglich der **Importgruppe**.

### Funktionen auf selektierte Importposition/en
Diese Ansicht unterstützt die Mehrfachauswahl, daher muss jede selektierte Importposition die verlangten Voraussetzung für zu wählende Funktion unterstützen, andernfalls wird der Menüpunkt nicht aktiv geschaltet.

#### Korrekturfunktionen
Die folgenden Funktionen dienen der Korrektur an **Importposition**, damit diese **Transaktionsfähig** werden:
- **Differenz Gesamtsumme akzeptieren**: Dabei wird der Wert der "**Errechnete Gesamtsumme**" akzeptiert und die entsprechende Importposition wird **Transaktionsfähig**. Diese Funktion sollten nur angewendet werden, falls die Abweichung der berechneten zur realen Gesamtsumme sehr gering ist.
- **Korrigiere Multiplikation auf Gesamtsumme**: Diese Funktion kann möglicherweise das Problem **Transaktionsfähigkeit** der Importposition bezüglich der gescheiterten **Gesamtsumme Überprüfung** beheben.
   + Manchmal ist der Wechselkurs im importierten Dokument nicht exakt angegeben, diese Funktion korrigiert den Wechselkurs entsprechend.
   + In GT ist der **Zins in Prozenten** massgebend für die Berechnung der **Gesamtsumme** des Zinses, dieser wird möglicherweise als Jahreszins augewiesen, obwohl der massgebende Zins eine kürzere Zeitdauer abdeckt. Daher kann diese Funktion den **Zins in Prozenten** entsprechend der **Gesamtsumme** anpassen.
- **Instrument zuweisen**: Eier Wertpapiertransaktion können Sie eine Instrument zuweisen. Dazu öffnet sich der [Suchdialog für Instrumente](../../../watchlistinstrument/instrument/searchdialog).
- **Bankkonto zuweisen**: Hiermit können sie der Importposition ein Bankkonto zuweisen.
- **Datum Dividendenzahlung**: Offensichtlich können Dividenden auch am Wochenende ausgezahlt werden. Diese Wochenendauszahlung wird von GT nicht unterstützt. Daher wird eine Zahlung an einem Samstag auf den vorhergehenden Freitag und eine Zahlung an einem Sonntag auf den folgenden Montag verschoben.
- **GTNet-Import für fehlende Wertapapiere erstellen**: Falls Importpositionen ein fehlendes Wertpapier haben, können diese über das GTNet-Peer-Netzwerk abgefragt und automatisch erstellt werden. Siehe [Wertpapierimport für Transaktionen](../securityimportfortransaction/) für Details.

{{% notice style="info" title="Automatische Korrektur von Zinsen und Dividenden bei Wertpapieren" %}}
Grundsätzlich erwartet GT eine Transaktion in der Währung des Instruments. Insbesondere ETFs können in verschiedenen Währungen gehandelt werden, obwohl sie nur eine Fondswährung haben. Die Auszahlung von Zinsen oder Dividenden erfolgt in der Fondswährung. In solchen Fällen kann es sein, dass die Handelsplattform keine Währungsumrechnung vornimmt. Beim Import einer solchen Transaktion ergänzt GT die Transaktion mit einem Währungskurs, der anhand des Handelsdatums ermittelt wird. Selbstverständlich werden der Zins- oder Dividendenbetrag und eventuelle Steuern automatisch angepasst. Diese Ergänzung erfolgt bei der Erstellung der Transaktion und ist daher nur bei der erstellten Transaktion sichtbar.
{{% /notice %}}

#### Transaktionsfunktionen
Die folgenden Funktionen beziehen sich auf das Erstellen einer Transaktion.
- **Erstelle Transaktion**: Die Transformation einer Importposition in eine Transaktion ist nur möglich, falls die Importposition noch **keine Transaktion** und keine "**vielleicht Transaktion**" hat.
- **Übergehe die vielleicht Transaktion**: Das System erkannte die Importposition als mögliche Transaktion und hat diese entsprechend so ausgezeichnet, siehe Eigenschaft **Hat vielleicht Transaktion**. Mit dieser Funktion kann diese Erkennung durch das System für die selektierte/n Importpositionen ausgeschaltet werden.
- **Prüfe auf bestehende Transaktion**: Falls die Funktion "**Übergehe die vielleicht Transaktion**" zur Anwendung kam, aktiviert diese Funktion die Prüfung der Importposition auf bestehende mögliche Transaktion.

### Expandierende Tabellenzeile
Die **expandierende Tabellenzeile** hat zwei unterschiedliche Ansichten:
- **Importposition erkannt**: Das erkannte Ergebnis mit den **Zugeordneten Werte**, die verwendete **Importvorlage** und der **Importstatus** werden angezeigt.
- **Dokument nicht erkannt**: Es gibt eine tabellarische Ansicht mit allen **Importvorlagen** der **Vorlagengruppe**. Dabei erfolgt die Anzeige mit dem letzten erfolgreich erkannten **Feld** pro **Importvorlage**.

### Transaktionsimport in der Praxis
{{< youtube uKzyETfcWRk >}}