---
title: "Instrumente"
date: 2021-03-14T22:54:47+01:00
draft: false
weight: 25
archetype: "default"
---
In GT wird der allgemeine Begriff Instrument verwendet, weil es ein Sammelbegriff für Wertpapier, Währungspaar, abgeleitetes Instrument und nicht handelbaren »Wertpapier« ist.
- **Wertpapier**: Ein Wertpapier wird einer **Anlageklasse** zugeordnet. Diese Zuordnung bestimmt, wie das Wertpapier gehandelt werden kann.
- **Abgeleitetes Instrument**: In GT kann ein abgeleitetes Instrument gehandelt werden, falls die zugeordnete **Anlageklasse** dies erlaubt.
- **Währungspaar**: In GT kann das Währungspaar nicht gehandelt werden. Falls unterschiedliche Währung in Portfolios zur Anwendung kommen, werden diese primär für die Berechnung der Performance benötigt.

## Kursdaten
In GT macht ein **Instrument** ohne **Kursdaten** keinen Sinn, wobei **Kursdaten** ein Oberbegriff für unterschiedliche Preisdaten ist.
- **Kursdaten Konnektor**: Für die meisten Instrumente werden die Kursdaten über einen **Konnektor** geladen. Für ein **Währungspaar** sind die beiden Konnektoren Innertag- und historische-Kursdaten obligatorisch.
- **Stückelung**: Bei einem **Festgeld** ändert sich der Kurs während der Laufzeit nicht, daher entspricht der Kurs der **Stückelung**. Mehr dazu in [Wertpapier](./securityderived/security/).
- **Historische Kurse für Periode**: Es ist auch möglich das ein Instrument keine öffentlichen Kursdaten hat, aber während der Laufzeit sehr selten die Kurse ändert. In diesem Fall kommt "**Historische Kurse für Periode**" zur Anwendung. Mehr dazu in [Wertpapier](./securityderived/security/).
- **Abgeleitetes Instrument**: Hierbei werden sowohl der **Innertag** wie auch die **historischen Kursdaten** aus einem anderen Bestehenden Instrument abgeleitet. Mehr dazu in [Abgeleitetes Instrument](./securityderived/derivedinstrument/). 

{{< youtube Zij10hSO9OQ >}}

## Konnektoren
Bestimmte **Kursdaten**, **Splits** und **Dividenden** werden über **Konnektoren** aus externen Datenquellen bezogen. Ob ein **Typus** von Konnektoren beim Bearbeiten eines Instrumentes aktiviert ist, wird durch die Selektion der **Anlageklasse** und des **Handelsplatzes** bestimmt. Die Typen von Konnektoren haben bestimmte Gemeinsamkeiten:
- Wird die **Datenquelle geändert**, so werden die bestehenden Daten des bisherigen Konnektoren gelöscht und erneut eingelesen.
   + Bei **Historische Kursdaten** und **Innertag** wird durch das Speichern die Datenquelle für ein Einlesen aktiviert. Wobei das Einlesen im Hintergrund ausgeführt wird, damit das Benutzerinterface nicht zulange blockiert.
   + Bei **Dividenden** und **Splits** erfolgt durch die Speicherung die Erstellung einer **Hintergrundaufgabe**, welche im [Stapelverarbeitungs Monitor](../../admindata/taskdatachangemonitor/) sichtbar wird.
- Je nach der Datenquelle muss eine **URL-Erweiterung** eingegeben werden, welche unterschiedlich schwer ermittelt werden kann. Jedoch befindet sich links neben dem Eingabefeldes eine Schaltfläche mit einer Hilfe.

### Typus Konnektor
Für die unterschiedlichen Typen von Konnektoren gibt es **mannigfaltige Einstellungen**, **Überwachungsmöglichkeit** und teilweise können **Korrekturen** an den **eingelesenen Daten** angebracht werden. Die **historischen Kursdaten** und **Splits** können auch manuell bearbeitet werden. Im folgenden folgt eine Aufzählung wo Sie weiterführende Informationen über die unterschiedlichen Typen von Konnektoren finden:
- **Innertag Kursdaten**: Für viele  Instrumente werden Sie eine [Datenquellen](../externaldata/) finden. Mit der Watchlist-Ansicht [Kurs Datenfeed](../watchlist/pricefeed/) kann diese überwacht werden.
- **Historische Kursdaten**: In GT werden die historischen Kursdaten vielfältig genutzt. Zusätzlich zu den bei **Innertag Kursdaten** genannten Querverweise gibt es zusätzliche Überwachnungen in [Periodenertrag](../../reportportfolio/periodperformance/) und [Vollständigkeit historischer Wertpapier-Kursdaten](../../admindata/historyquotequality/). Die historischen Kursdaten können als [Liniengrafik](../eodchart/) betrachtet werden. Zudem können diese im **Zusatzbereich** mit der Ansicht [Historische Kurse](../externaldata/historyquote/pricedata/) bearbeitet werden
- **Dividenden**: Mehr Informationen finden Sie unter [Historische Daten](../externaldata/historyquote/) und [Dividende/Split Feed](../watchlist/dividendsplit/).
- **Split**: Mehr Informationen finden Sie unter [Wertpapier](./securityderived/security/), [Historische Daten](../externaldata/historyquote/) und [Dividende/Split Feed](../watchlist/dividendsplit/).

### Unsichtbare Konnektoren
Für **Splits** gibt es unsichtbare Konnektoren, die **täglich** einen **Split-Kalender** überwachen. Bei diesen Typus Konnektor kann der Benutzer keine Änderungen vornehmen. Mehr zum Thema gibt es unter [Historische Daten](../externaldata/historyquote/).

{{< youtube UAHPG3zP-GA >}}
