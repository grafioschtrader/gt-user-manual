---
title: "Splits und Dividenden"
date: 2026-10-16T12:00:00+02:00
draft: false
weight: 30
archetype: "default"
---
Diese Hintergrundaufgaben übernehmen Splits und Dividenden der Instrumente aus externen Datenquellen und führen die davon abhängigen Daten nach. Ein Split ändert die Bestandsmenge und erfordert splitbereinigte historische Kursdaten; daran sind die Aufgaben 32, 38 und 36 beteiligt. Die Aufgaben 31 und 43 lesen die Dividenden.

## Ablauf eines Splits {#SPLITFLOW}
Die tägliche [Aufgabe 30](../pricedata/#JOB30) liest einen oder mehrere Split-Kalender. Ein Kalender nennt einen Split mit Tickersymbol, Firmennamen und meist auch der Börse oder dem Land der Kotierung. GT ordnet ihn einem eigenen Wertpapier nur zu, wenn das Tickersymbol übereinstimmt, die Kotierung zur Börse des Wertpapiers passt und der Firmenname ausreichend ähnlich ist. Die Prüfung der Kotierung ist nötig, weil Hinterlegungsscheine wie die kanadischen CDRs oft das Tickersymbol der zugrunde liegenden Aktie tragen, aber unabhängig von ihr splitten. So meldete ein Split-Kalender am 16.10.2026 Splits der CDRs von Advanced Micro Devices und CrowdStrike unter den Tickersymbolen AMD und CRWD; die Aktien selbst waren nicht betroffen.

Es kann mehrere Tage dauern, bis ein angekündigter Split auch beim Konnektor des Wertpapiers und in dessen historischen Kursdaten abgebildet ist. Das folgende vereinfachte Flussdiagramm zeigt diesen Ablauf, wobei ein Hexagon-Knoten für eine Hintergrundaufgabe steht:
{{< mermaid >}}
graph TD;
    A{{Aufgabe 30: täglich}} --> B(Ein oder mehrere Split-Kalender lesen)
    B --> |Split einer passenden Kotierung gefunden| C{{+2 Minuten: Aufgabe 32}}
    C --> D(Splits des Wertpapiers über seinen Konnektor einlesen)
    D --> E{Angekündigter Split geliefert?}
    E --> |Nein, längstens 5 Tage nach dem Splitdatum| G{{+1 Tag: Aufgabe 32}}
    G --> D
    E --> |Ja| I(Prüfen, ob die historischen Kursdaten des Datenlieferanten den Split enthalten)
    I --> K{Split in den Kursdaten?}
    K --> |Nein| H{{+ einige Tage: Aufgabe 38}}
    H --> I
    K --> |Ja| R(Historische Kursdaten neu einlesen)
    R --> L{{+0: Aufgabe 36}}
    L --> M(Bestand des Wertpapiers der betroffenen Klienten für diesen Split nachführen)
{{< /mermaid >}}

## 32 - Splits neu einlesen, weil Konnektor geändert wurde oder das Instrument einen Split erfahren hat {#JOB32}
Wird ausgelöst, wenn der Datenkonnektor des Splits geändert wurde oder wenn ein möglicher neuer Split im Splitkalender entdeckt wurde. Er lädt alle Splits für ein bestimmtes Wertpapier neu. Wenn die Splits des Instruments geändert wurden, werden auch die historischen Kursdaten komplett neu eingelesen. Liefert der Konnektor einen im Split-Kalender angekündigten Split noch nicht, wird die Aufgabe am nächsten Tag wiederholt, längstens bis fünf Tage nach dem Splitdatum. Den ganzen Ablauf zeigt [Ablauf eines Splits](#SPLITFLOW).

{{% callout style="info" title="Falsche Split-Detektion" %}}
Splits einer anderen Kotierung mit demselben Tickersymbol werden nicht übernommen, beispielsweise der am 14.11.2025 im Split-Kalender von Yahoo Finance gemeldete Split PLTR.NE. Er betraf nicht die Palantir-Aktie, sondern deren an einer kanadischen Börse gehandelten Hinterlegungsschein. Nennt ein Split-Kalender die Kotierung nicht, oder tragen an derselben Börse zwei Produkte dasselbe Tickersymbol und einen ähnlichen Namen, kann ein Split trotzdem dem falschen Wertpapier zugeordnet werden. Die Aufgabe 32 wird dann während einigen Tagen erfolglos wiederholt. Eine solche Aufgabe mit dem Status «Warten» kann im Monitor gelöscht werden, womit auch ihre weitere Erstellung unterbleibt; spätestens fünf Tage nach dem Splitdatum stellt das System sie von selbst ein.
{{% /callout %}}

## 38 - Prüfe nach einem Split wiederholend ob die historischen Kursdaten dies widerspiegeln und geladen werden können {#JOB38}
Wenn für ein Wertpapier ein Split hinzugefügt wird, kann es einige Tage dauern, bis die angepassten historischen Kursdaten beim Datenprovider vorliegen. Daher wird diese Aufgabe so lange wiederholt, bis die historischen Kursdaten des Wertpapiers erfolgreich eingelesen wurden.

## 36 - Bestandsmenge eines Wertpapiers hat durch ein Split geändert, alle abhängigen Wertpapierbestände der Klienten werden neu aufgebaut {#JOB36}
Die Veränderung von Splits kann sich auf die Bestandstabelle der Instrumente auswirken. Siehe mehr zu diesem Thema unter "[Bericht Periodenertrag und redundante Daten](../../../../reportportfolio/periodperformance/holdingtable)".

## 31 - Dividende lesen, da Konnektor geändert wurde {#JOB31}
Mehr zu diesem Thema, siehe [Dividende](../../../../watchlistinstrument/externaldata/historyquote/).

## 43 - Führt mögliche neue Dividenden der Instrumente über die Konnektoren nach {#JOB43}
Das Laden von Dividenden für die einzelnen Instrumente über Konnektoren wird derzeit mit zwei verschiedenen Algorithmen durchgeführt. Der eine Algorithmus überwacht einen oder mehrere Dividendenkalender, der andere arbeitet mit der erwarteten Periodizität der Dividendenzahlungen:
- **Dividendenkalender**: Der Dividendenkalender wird täglich an den Handelstagen über den entsprechenden Konnektor geladen. Die Dividenden werden hinzugefügt, wenn die in diesem Kalender aufgeführten Instrumente auch in dieser GT-Instanz enthalten sind.
- **Periodizität**: Damit werden Dividendenerträge in der Entität Dividende nachgetragen. Der folgende Algorithmus wird verwendet, um eventuell fehlende Dividendenerträge in der Entität Dividende für Wertpapiere zu ermitteln. Dies geschieht auf Basis des Datums der letzten Dividendenzahlung und der Periodizität der erwarteten Zahlungen. Zusätzlich wird auch die Dividendenzahlung in der Entität Transaktion berücksichtigt, wenn die Dividendenzahlung jünger ist als das Datum in der Entität Dividende.
