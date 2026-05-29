---
title: "Korrelationsmatrix"
date: 2025-06-01T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
Die Kenntnisse aus einer Korrelationsmatrix können durchaus unterstützend bei der Diversifizierung eines Portfolios sein. Gilt es doch, mit einem bestimmten Mix aus Anlageklassen für jede Wetterlage an den Märkten gerüstet zu sein. Dies kann durch eine Auswahl kaum korrelierender Anlageklassen gelingen. Dabei wird das Verlustrisiko reduziert, ohne gleichzeitig die Rendite zu schmälern. Die meisten Korrelationen verändern sich im Verlaufe der Zeit. Daher implementiert GT auch eine **rollierende Korrelation** als Liniengrafik. Die Berechnungen basieren auf dem **Schlusskurs** der **Tagesenddaten**, wobei dabei unterschiedliche Stichprobenzeiträume angewendet werden. Zusätzlich kann optional ein **Zeitabschnitt** mit Datumsangaben begrenzt werden.

{{% notice note "Warnung!" %}}
#### Korrelation und Dividende
Wie im [Glossar](../../glossar/) erwähnt, muss zwischen ETF bzw. Fonds **ausschüttend** oder **thesaurierend** wie auch zwischen **Kurs-** und **Performanceindex** unterschieden werden. Ein ähnlicher Unterschied besteht zwischen **Aktien** mit oder ohne **Dividende**.

#### GT und diese Problematik
GT berücksichtigt in der Version die Dividenden für die Korrelationen nicht. Dies kann zu leichten Verzerrungen bei den Resultaten führen.
{{% /notice %}}

## Korrelationsmatrix und Korrelationsset
Eine Korrelationsmatrix wird durch die Einstellungen eines einzelnen **Korrelationsset** bestimmt. Ein Benutzer kann mehrere Korrelationsset erstellen und bearbeiten.
- Der Korrelationsmatrix kann ein Instrument **ohne Kursdaten nicht hinzugefügt** werden. Ein solches definiert sich durch einen **Handelsplatz ohne Kursdaten**.
- Die maximale Anzahl von Korrelationsset ist durch den Wert von "`gt.max.correlation.set`" in Globale Einstellungen gegeben.
- Die maximale Anzahl Instrumente pro Korrelationsset ist durch den Wert von "`gt.max.correlation.instruments`" in Globale Einstellungen definiert.

## Funktionalität
Die **Korrelationsmatrix** wird durch die Selektion des statischen Elementes "**Watchlist**" im **Navigationsbaum** angesteuert. Wir unterscheiden die Funktionalität bezüglich der **Basisdefinition** und den **Instrumenten** eines Korrelationsset. Die **Korrelationsmatrix** entsteht dadurch, dass die Basisdefinition auf die entsprechenden Instrumente angewendet wird.

#### Erstellen und bearbeiten Korrelationsset
Die **Basisdefinition** eines Korrelationsset kann wie folgt bearbeitet werden:
- Erstellen eines **Korrelationsset** über das **Kontextmenü**.
- Bearbeiten eines **Korrelationsset** über das **Kontextmenü** bei **selektiertem Korrelationsset**.
- Löschen eines **Korrelationsset** über das **Kontextmenü** bei **selektiertem Korrelationsset**.

Die meisten Eigenschaften eines bestehenden Korrelationsset können direkt in der Ansicht Korrelationsmatrix bearbeitet werden. Durch die Schaltfläche "**Speichern und berechnen**" wird die Korrelationsmatrix entsprechend aktualisiert.

{{% notice note "Berechnung der Korrelationsmatrix nicht möglich!" %}}
Möglicherweise liegen keine überlappende Schlusskurse der Instrumente vor. In diesem Fall wird keine Korrelationsmatrix angezeit. Stattdessen wird in der Tabelle der Instrumente mit den Daten des ältesten bzw. jüngsten Tagesendkurs ausgegeben. Dies erleichtert die Überprüfung, ob die Eigenschaften `Datum von` bzw. `Datum bis`, keine überlappende Schlusskurse oder fehlende Schlusskurse die Berechnung der Korrelationsmatrix möglicherweise verunmöglichen.
{{% /notice %}}

#### Funktionen für Instrumente
Bei den Funktionen für Instrumente, wird zwischen selektierten Instrument oder keiner Selektion unterschieden. Zusätzlich bewirkt ein **Zellklick** in die Korrelationsmatrix möglicherweise weitere Funktionalität für die **rollierende Korrelation**.

##### Funktion bei selektiertem Korrelationsset
Einem Korrelationsset werden bestehende **Instrumente** hinzugefügt auf welche danach die **Basisdefinition** angewendet wird.
- **Bestehendes Instrument hinzufügen**: Die Funktionalität für das Hinzufügen eines Instruments ist über das Kontextmenü erreichbar. Hierzu öffnet sich ein entsprechende [Suchdialog für Instrumente](../instrument/searchdialog/). Die Auswahl der Instrumente wird implizit durch die Wahl der anderen Instrumente dieses Korrelationsset eingeschränkt. Das heisst, die Eigenschaften "**Handel ab**" und "**Aktiv bis oder Fälligkeitstag**" der anderen gewählten Instrumente können die Auswahl einschränken.

##### Funktionen selektiertes Instrument
- **Instrument entfernen**: Das selektierte Instrument kann aus der Korrelationsmatrix entfernt werden.
- **Hyperlink Handelsplatz und Produkt**: Siehe dazu [Wertpapier und abgeleitetes Instrument](../instrument/securityderived/).

##### Funktion selektiertes Instrument und Zellenklick
Bei einem **Klick** in eine **Zelle** der Korrelationsmatrix wird das Menü Ansicht bzw. das Kontextmenü mit der Funktion für die rollierende Korrelationsgrafik der beiden Instrumente erweitert. Eine bestehende rollende Korrelationsgrafik kann auch mit weiteren Korrelationen erweitert werden.

## Eigenschaften
{{% notice note "Grenze von Stichprobenzeitraum \"Täglicher Ertrag\"" %}}
Ein Stichprobenzeitraum der auf den **täglichen Ertrag** basiert ist nicht immer aussagekräftig. Bei ETFs die denselben Index nachbilden wird eine Korrelation nahe 1 erwartet. Trotzdem kann es erhebliche Unterschiede geben, beispielsweise beim S&P 500. Dieser wird von unterschiedlichen Anbietern in verschiedenen Regionen angeboten. Durch die Zeitzonen ergeben sich unterschiedliche Handelszeiten und somit auch differenzierte Schlusskurse. Anders bei **"Monatlicher Ertrag"**, hier haben die verschiedenen Schlusskurse der Handelsplätze kaum Auswirkung auf die Korrelation.
{{% /notice %}}

- **Datum von** und **Datum bis**: Damit kann ein Zeitabschnitt für die Korrelation definiert werden. Es können beide oder auch nur ein einzelnes Datum gesetzt werden.
- **Stichprobenzeitraum**: 
  - Beim "**Jährlicher Ertrag**" basieren die Berechnungen jeweils auf den verfügbaren **Schlusskurse** des ersten Handelstag eines **Jahres**. Für dieses vom System ermittelte Datum liegen die Tagesendkurse aller relevanten Instrumente vor.
  - Beim "**Monatlicher Ertrag**" basieren die Berechnungen jeweils auf den verfügbaren **Schlusskurse** des ersten Handelstag eines **Monates**. Für dieses vom System ermittelte Datum liegen die Tagesendkurse aller relevanten Instrumente vor.
- **Gleitende Korrelation**: Diese Eigenschaft beeinflusst die **rollierende Korrelation** in der grafischen Darstellung.
- **Währungsbereinigt**: Falls Instrumente mit unterschiedlichen Währungen vorliegen, kann optional die Berechnung mit Währungsbereinigung ausgeführt werden. Die entsprechenden Währungspaare werden durch das System bestimmt und sind durch den Benutzer nicht einsehbar. Auf mögliche Währungspaare in der bestehenden Korrelationsmatrix hatte diese Einstellung keinen Einfluss.

{{% notice note "Währungsbereinigt und nicht vorhandene Währungspaare" %}}
Es wird oder werden möglicherweise **Währungspaare** benötigt, damit die Korrelationsmatrix währungsbereinigt erstellt werden kann. Falls ein verlangtes Währungspaar dem System nicht vorliegt, wird dieses vom System erstellt und die **historischen Kursdaten** direkt heruntergeladen. Dies kann die Erstellung der Korrelationsmatrix um einige Sekunden verzögern.
{{% /notice %}}

## Expandierende Tabellenzeile
Durch den Klick auf das geschlossene **Expander-Symbol** werden die "Rendite und statistische Daten" angezeigt, siehe dazu [Rendite und statistische Daten](./returnstatdata/).

## Rollende Korrelation
Im Zusatzbereich kann eine gewünschte rollende Korrelation angezeigt werden. Weitere Korrelationen können einer bestehenden Grafik hinzugefügt werden. Das Auslösen der Schaltfläche "**Speichern und berechnen**" aktualisiert auch die rollende Korrelation.
- Wenn ein **Datum von** definiert ist und die entsprechenden Schlusskurse verfügbar sind, wird dieses als das Startdatum der rollende Korrelation gewählt.

## Video Korrelationsmatrix und statistische Daten
Hier können Sie die Korrelationsmatrix in Aktion sehen:
{{< youtube NfA8VxYBSfo >}}
