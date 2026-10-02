---
title: "Zusatzfeld Instrument"
date: 2024-06-23T20:54:47+01:00
draft: false
weight: 12
archetype: "default"
---
Für **Instrumente** gibt es eine **spezielle Implementierung** für benutzerdefinierte Eigenschaften. Diese berücksichtigt die Eigenschaften der Anlageklasse eines Instruments. Beispielsweise definiert der Benutzer ein Zusatzfeld für die Kosten eines ETF. Es würde keinen Sinn machen, wenn dieses Zusatzfeld auch bei der Erfassung von Zusatzdaten für ein Wertpapier einer Aktie erscheinen würde.

## Allgemeine Felder
- **Rendite bis zur Fälligkeit**: Für **Anleihen** steht allen Benutzern das Zusatzfeld "Rendite bis zur Fälligkeit" zur Verfügung. Der Benutzer kann dieses Zusatzfeld nicht ändern. Dieses Zusatzfeld wird aus dem Zinssatz, dem aktuellen Datum und der Fälligkeit der Anleihe berechnet. Der Zinssatz wird aus dem Namen der Anleihe ermittelt, wenn der **Name den Zinssatz als Präfix** enthält. Für die meisten Anleiheinvestoren ist diese Kennzahl eine Entscheidungshilfe für den Kauf oder Verkauf einer Anleihe.
### Yahoo Inhalte für allgemeine Zusatzfelder
Die folgenden Inhalte beziehen sich alle auf Yahoo. Es handelt sich um **zwei Links** und ein **extrahiertes Datum mit Uhrzeit**. Da GT das Yahoo-Symbol nicht für jeden Titel kennt, muss es allenfalls ermittelt werden. Diese Ermittlung kann sehr zeitaufwendig sein, da die Webseite von Yahoo kontaktiert werden muss. Aus diesem Grund gibt es auch einen [Hintergrundjob](../../../admindata/taskdatachangemonitor/taskdescription/#JOB19), der diese Aufgabe übernimmt. Das gleiche kann aber auch interaktiv in der Benutzeroberfläche geschehen, was zu längeren Wartezeiten führen kann.
- **Yahoo Statistik Link**: Die Yahoo-Statistik gibt einen guten Überblick über die verschiedenen Informationen zu einem Wertpapier.
- **Nächster Bilanztermin**: Häufig beeinflusst der Bilanztermin den Aktienkurs erheblich. Sowohl vor als auch nach der Veröffentlichung der Zahlen. Darüber hinaus können auch die Bilanzzahlen anderer Unternehmen den Aktienkurs bewegen, da es Konkurrenten, Abhängigkeiten etc. gibt. Diese Information wird für jedes Wertpapier von der Website gelesen. Dies kann einige Zeit in Anspruch nehmen.
- **Yahoo Ertrag Link**: Je nach Wertpapier finden Sie unter diesem Link eine Historie der Gewinne/Verluste je Aktie und die Angabe der erwarteten Gewinne. Ausserdem werden in den meisten Fällen die zukünftigen Bilanztermine angegeben.

## Eigenschaften und Tabellenspalten
Die Bearbeitung und die Eigenschaften des "Zusatzfeldes Instrument" sind weitgehend identisch mit denen des "Zusatzfeldes allgemein". Es ist zusätzlich um die beiden Eigenschaften **Anlageklasse** und **Finanzinstrument** erweitert. Die Auswahl erfolgt über ein Listenfeld mit Mehrfachauswahl. Das so definierte Zusatzfeld erscheint bei der **Befüllung**, wenn **beide Kriterien** der Anlageklasse des Instruments erfüllt sind.
