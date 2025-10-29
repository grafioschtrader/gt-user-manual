---
title: "Tagesendkurse als Linengrafik"
date: 2021-03-29T22:54:47+01:00
draft: false
weight: 30
archetype: "default"
---
In GT können die **Tagesendkurse als Liniengrafik** eines **Instrumentes** im **Zusatzbereich** als **Diagramm** dargestellt werden. Daraus lässt sich die Performance der Kurse ab einem wählbaren Datum eines Instrumentes ablesen. Wobei sich die angebotene Funktionalität bezüglich dem **Wertpapier** einschliessich **abgeleitetes Wertpapier** und dem **Währungspaar** leicht unterscheiden.

{{% notice info %}}
Die Liniengrafik basiert auf den historischen Kursdaten plus dem letzten Innertag Kurs falls dieser vorliegt.
{{% /notice %}}

{{% notice warning %}}
#### Kurschart oder Performancechart
Wie im [Glossar](../../glossar/) erwähnt, muss zwischen ETF bzw. Fonds **ausschüttend** oder **thesaurierend** wie auch zwischen **Kurs-** und **Performanceindex** unterschieden werden. Ein ähnlicher Unterschied besteht zwischen **Aktien** mit oder ohne **Dividende**.

GT bietet für das Problem der unterschiedlichen Währungen von Instrumenten eine wählbare Währung an. Jedoch werden im Diagramm die täglichen **Schlusskurse** verwendet, diese können je nach Instrument die Erträge mit berücksichtigen oder auch nicht. Daher sollte sich der Benutzer immer bewusst sein, ob in diesem Diagramm **ausschüttende** oder **thesaurierende** ETFs bzw. Kurs- oder Performanceindizes miteinander verglichen werden.

#### GT und diese Problematik
Mit dieser Version verfügt GT nicht über die entsprechenden qualitativen Datenquellen für die exakte Berechnung der Performance eines Instrumentes. Eine Implementierung wird in Betracht gezogen, sobald die Erträge verlässlich aus den Datenquellen bezogen werden können.
{{% /notice %}}

### Aktivierung dieser Ansicht
Die **Liniengrafik** im **Zusatzbereich** kann von Unterschiedlichen Ansichten des **Hauptbereiches** aktiviert werden. In der Praxis wird sie wahrscheinlich meistens durch die Funktionalität der Watchlist genutzt. Jedoch gibt es in GT auch andere Ansichten mit einem Instrument pro Tabellenzeile, wo die folgende Funktionalität über das Kontextmenü angeboten wird.  
+ **Tagesenddaten als Liniengrafik**: Erstellt eine **neue Liniengrafik** mit dem selektierten **Instrument**.
+ **Hinzufügen als Liniengrafik**: Das selektierte **Instrument** wird der **bestehenden Liniengrafik** hinzugefügt. Damit kann die Performance unterschiedliche Instrumente verglichen werden.

### Transaktionen in der Liniengrafik
Bei der Abbildung der Transaktionen wird unterschieden zwischen einem **Wertpapier** bzw. **abgeleiteten Wertpapier** und **Währungspaar**.
- **Wertpapier und abgeleiteten Wertpapier**: Die **Transaktionen** wie **Kaufen**, **Verkaufen** und **Zins/Dividende** werden als gleich grosse Punkte in der Liniengrafik dargestellt.
- **Währungspaar**: Der Durchmesser des Punktes ist ein Abbild der Gesamtsumme der Transaktion. Ausserdem werden auch die Transaktion des **wechselseitigen Währungspaars** abgebildet. So enthält beispielsweise die Liniengrafik des Währungspaar EUR/USD auch die Transaktionen des Währungspaar USD/EUR. Die Grösse des Punkts wird vereinheitlicht sobald der Auswahlkasten **Prozent** markiert ist und/oder mehrere Instrumente bzw. Währungspaare angezeigt werden. Wertpapiertransaktionen mit dem involvierten Währungspaar wird entsprechend in **Kaufen** bzw. **Verkaufen** der betreffenden Währung transformiert.

### Bestand
Für ein **einzelnes Instrument** kann der **Bestand** über den gewählten Zeitraum betrachtet werden. Die Skalierung der **Y-Achse** des **Bestandes** wird auf der **rechten Seite** dargestellt. Auf der linken Seite befindet sich wie immer die Skalierung der Datenreihe der Kurse mit dem Preis- oder der Prozentangabe. Natürlich können auch negative Bestände dargestellt werden. Die Anzeige des Bestands kann im **Kopfteil** des Diagramms **ein- bzw. ausgeschaltet** werden.

### Funktionen
Allgemein verfügt ein [Diagramm über eine gewisse Grundfunktionalität](../../intro/userinterface/user_setting_ui_controls/) die hier nicht weiter beschrieben wird. Zudem besitzt dieses Diagramm über einen **interaktiven Kopfteil** sowie ein **Kontextmenü** für die rudimentäre technische Analyse.

#### Interaktionen im Kopfteil
Dieses Diagramm bietet im **Kopfteil** zusätzliche Interaktionsmöglichkeiten:
- **Datum von**: Ab diesem Datum werden die Kursdaten angezeigt. Für eine schnelle Einstellung gibt es **Knöpfe für unterschiedliche Zeiträume**. 
- **Prozent**: Sobald zwei oder mehr Instrumente als Liniengrafik angezeigt werden, ist die Anzeige der Grafik mit absoluten Werten nur in wenigen Fällen sinnvoll. Daher gibt es dieses Kontrollkästchen, es wird automatisch markiert, falls die Funktion "**Hinzufügen als Liniengrafik**" erstmalig ausgeführt wird. Wobei das System den vom Benutzer eingestellte Zustand dieses Auswahlkasten nicht übersteuert. Die prozentuale Anzeige der unterschiedlichen Instrumente ab einem bestimmten Datum ermöglicht einen besseren Vergleich der Kursänderungen.
- **Verbinde Lücken**: Ist dieses Kontrollkästchen markiert, werden die vorhandenen Kursdaten mit einer Linie verbunden. Bei nicht markierten Auswahlkasten werden die möglichen Lücken von fehlenden Kursdaten sichtbar. Fehlende Kursdaten werden anhand des Handelskalender des entsprechenden Instrumentes ermittelt.
- **Währung**: GT unterstützt die Instrumente in unterschiedlichen Währungen. Bei der Vergleichbarkeit der Performance von Instrumenten muss die Währung daher mitberücksichtigt werden. Ausgehend von der **Währung des Klient** kann eine beliebige Währung der angezeigten Instrumente ausgewählt werden. Die Wahl der Währung hat keine Auswirkung auf die Liniengrafik der **Währungspaare**.

{{% notice note "Währungen und nicht vorhandene Währungspaare" %}}
Es wird oder werden Währungspaare benötigt, damit die Funktion von **Währung** funktioniert. Falls ein verlangtes Währungspaar dem System nicht vorliegt, wird dieses vom System erstellt und die **historischen Kursdaten** direkt **heruntergeladen**. Dies kann die Erstellung der Liniengrafik um einige Sekunden verzögern.
{{% /notice %}}


#### Kontextmenü für technische Indikatoren
Zurzeit unterstützt GT nur den **einfachen** und **exponentiell gleitenden Durchschnitt**. Diese können über das Kontextmenü aktiviert und deaktiviert werden. Zusätzlich sind die Perioden in einem Dialog einstellbar.

{{% notice info %}}
GT wird kein Werkzeug für die **technische Analyse** sein, trotzdem werden zukünftig noch einige **technische Indikatoren** implementiert werden.
{{% /notice %}}

### Tagesendkurse als Linengrafik in der Praxis
{{< youtube qNaBNDIAnZ0 >}}
