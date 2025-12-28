---
title: "Definition Zusatzfeld allgemein"
date: 2024-06-23T20:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
GT gibt die entsprechenden **Eigenschaften** für die **Informationsklassen** vor. Manchmal wäre es jedoch praktisch, wenn der Benutzer eigene Eigenschaften für eine Informationsklasse definieren könnte. Insbesondere im Bereich der **Informationsklasse Instrumente** wäre eine solche Funktion sehr hilfreich. Aus diesem Grund kann der Benutzer für bestimmte Informationsklassen entsprechende **benutzerdefinierte Eigenschaften** anlegen.

Aus dieser Beschreibung geht hervor, dass diese Funktionalität aus **zwei Teilen** bestehen muss. Es gibt einen Teil, in dem die Eigenschaften der **Zusatzfelder definiert** werden, und einen anderen Teil, in dem diese **Zusatzfelder mit Inhalt gefüllt** werden. Die inhaltliche Befüllung erfolgt bei der Bearbeitung der Informationsklasse. Beispielsweise wird für Instrumente und Währungspaare die Befüllung und Darstellung in der Watchlist erfolgen. Dementsprechend wird diese Funktionalität auch dort beschrieben, siehe dazu [Zusatzfeld](../../watchlistinstrument/watchlist/udf/).

{{% notice style="info" title="Zusätzliches Feld für alle Benutzer" %}}
Es gibt auch zusätzliche Felder für alle Benutzer. Warum dies? Ein relationales Datenbankmanagementsystem, auf dem GT basiert, hat gewisse Grenzen bei der Abbildung der unterschiedlichen Ausprägungen von Instrumenten. Eine Anleihe hat zum Teil ganz andere Eigenschaften als eine Aktie. Aus diesem Grund gibt es eine spezielle Implementierung von [Zusatzfeldern für Instrumente](./instruments/). Dort sind einige zusätzliche Felder für alle Benutzer definiert. Diese betreffen Anleihen, Aktien als Direktanlagen und CFD.
{{% /notice %}}

## Erstellen und bearbeiten 
Einige Funktionen können immer ausgeführt werden, andere sind von der in der Tabelle ausgewählten Definition abhängig.

### Funktionen in der Ansicht
Die Bezeichnungen der Menüpunkte können geringfügig von der Realität abweichen, da diese Beschreibung für eine spezielle Implementierung gedacht ist.
Ein **Zusatzfeld** wird im **Hauptbereich** in der **Ansicht Zusatzfeld allgemein** erstellt, bearbeitet und gelöscht. Diese **Ansicht** erreichen Sie im **Navigationsbereich** auf dem statischen Element "Definition Zusatfeld allgemein".
+ **Erstellen** eines "**Definition Zusatzfeldes**" über das **Kontextmenü**.
+ **Bearbeiten** eines **Zusatzfeld** über das **Kontextmenü** bei **selektierten "Definition Zusatzfeld"**.
+ **Löschen** einer "**Definition Zusatzfeld**" über das **Kontextmenü** bei **selektierten "Definition Zusatzfeld"**.

### Funktionen über allgemeine Zusatzfelder
Möglicherweise sind die **allgemeinen Zusatzfelder** für Sie nicht von Interesse. Sie haben daher die Möglichkeit, diese allgemeinen Felder zu deaktivieren. Sie werden dann in den entsprechenden **Informationsklassen** nicht mehr angezeigt. Die Aktivierung bzw. Deaktivierung erfolgt über das **Kontextmenü** der ausgewählten Zeile einer **allgemeinen Zusatzfelddefinition**.

## Eigenschaften und Tabellenspalten
Für die Definition eines Zusatzfeldes sind einige Angaben erforderlich. Um die Struktur der Daten zu erhalten, wird mit Datentypen gearbeitet. Dies dient der Plausibilisierung der Eingaben bei der späteren Befüllung der Zusatzfelder. Nur bei Angabe des Datentyps kann eine Sortierung, Filterung und evtl. eine zukünftige Berechnung auf Basis dieser Daten korrekt funktionieren.
- **Informationsobjekt**: Derzeit werden nur Währungspaare und Instrumente unterstützt, wobei es für Instrumente eine spezielle Implementierung gibt.
- **Reihenfolge GUI**: Die Eingabefelder sind nach dieser aufsteigenden Nummer geordnet.
- **Name Eigenschaft**: Dies ist der Text, der in der Spaltenüberschrift der Tabelle oder links oder über dem Eingabe- oder Ausgabefeld erscheint.
- **Hilfstext**: Die Anzahl der Zeichen von "Name des Eigenschaft" ist begrenzt, daher kann eine ausführlichere Beschreibung der Eigenschaft angegeben werden. Diese wird als Tooltip in der Spaltenüberschrift der Tabelle, beim Eingabefeld oder neben dem Eigenschaftsnamen eines Ausgabefeldes angezeigt. 
- **Datentyp**: Die für die Angabe des **Datentyp** und dessen Feldlänge sind nur bestimmte Kombinationen möglich. Bei bestimmten Datentypen wird die Eingabe der Eingabelänge nicht verlangt. Zuerst folgt die Beschreibung dieser Datentypen:
  - **Datum**: Datums Eigenschaft ohne Zeitangabe.
  - **Datum und Zeit**: Eigenschaft für Datum mit Zeit.
  - **Ja und Nein**: Ein Kontrollkästchen kann verwendet werden, um eine Ja/Nein-Eigenschaft anzuzeigen. 
  - **Website URL**: GT kann nur einen kleinen Teil der für eine Investitionsentscheidung relevanten Informationen abbilden. Weitere nützliche Quellen können über die URLs der Webseiten hinzugezogen werden. Die maximale Länge einer URL darf 256 Zeichen nicht überschreiten.
  - **Eingabeläge oder Grenze**: Für die folgenden Datentypen muss eine Eingabelänger oder Grenze angegeben werden.
    - **Dezimalzahl**: Hier sind die **Gesamtlänge der Dezimalzahl** und die **Anzahl der Nachkommastellen** durch ein Komma getrennt anzugeben. Das Trennzeichen wird nicht in die Gesamtlänge eingerechnet. Beispiel: Der Bereich für "5,4" ist -9.9999 - 9.9999. In diesem Beispiel ist "." das Trennzeichen.
    - **Ganzzahl**: Hier sind die durch Komma getrennten ganzzahligen **Minimal-** und **Maximalwerte** einzutragen. Der Wertebereich kann auch negative Zahlen umfassen. Beispiel: "-10,99" umfasst den Wertebereich von -10 bis 99.
    - **Zeichenkette**: Geben Sie hier, durch Komma getrennt, die **minimale** und **maximale Länge** der Zeichenkette ein. Die maximale Länge darf 2048 Zeichen nicht überschreiten.

## Video Zusatzfelder
Hier können Sie die Zusatfelder in Aktion sehen:
{{< youtube xt0l3_8gYgM >}}
