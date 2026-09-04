---
title: "GTNet überwachen"
date: 2026-08-31T22:54:47+01:00
draft: false
weight: 45
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
GTNet bietet mehrere Möglichkeiten, den Status und die Aktivität des Datenaustauschs zu überwachen. Die eine Ansicht zeigt, was eingerichtet ist, die andere, was tatsächlich geschieht; die Hintergrundaufgaben verraten, ob die Automatismen laufen.

## Statische Übersicht: Kursdaten austauschen
Die Ansicht [Kursdaten austauschen](../exchange/) bietet eine statische Übersicht darüber, welche Instrumente für den Austausch konfiguriert sind und welche Gegenparts Daten liefern können:
- Alle Wertpapiere und Währungspaare mit ihren Austauscheinstellungen anzeigen
- Zeilen erweitern, um Lieferantendetails zu sehen (welche Gegenparts Daten für jedes Instrument liefern können)
- Nützlich zum Verstehen des aktuellen Konfigurationsstatus

## Dynamische Ansicht: Austauschprotokoll
Die Ansicht [Austauschprotokoll](../exchangelog/) bietet eine dynamische, zeitbasierte Übersicht über die tatsächliche Austauschaktivität:
- Statistiken für Intraday- und historischen Kursaustausch einsehen
- Lieferanten- und Verbraucherstatistiken pro verbundener Instanz verfolgen
- Aggregierte Daten nach Tag, Woche, Monat und Jahr anzeigen
- Erfordert die Aktivierung der Protokollierung über **g.gnet.use.log** in den [Globalen Einstellungen](../globalsettings/)

{{% notice style="note" title="Was die Statistik nicht aussagt" %}}
Die Statistik hält fest, wie viele Instrumente gesendet und wie viele daraufhin aktualisiert wurden. Sie sagt damit etwas über die Zuverlässigkeit einer Lieferung aus, aber nichts über die Richtigkeit der gelieferten Kurse. Siehe dazu den Abschnitt über die Grenzen von GTNet in der [Übersicht](../).
{{% /notice %}}

## Zustellversuche von Nachrichten
Ausgehende Nachrichten, die im Hintergrund an einen oder mehrere Gegenparts gesendet werden, lassen sich in der Ansicht [GTNet und Nachrichten](../setup/) je Empfänger verfolgen. Dazu wird die Zeile aufgeklappt, unter welcher die gesendete Nachricht liegt. Der Bereich **«Zustellversuche»** ist nur für Administratoren sichtbar und zeigt bereits im Titel die Anzahl der Einträge.

Die Tabelle nennt den Nachrichtenzeitpunkt und Nachrichtencode, die Zieldomäne, den Versuchsstatus und die Anzahl tatsächlich ausgeführter Übertragungsversuche. «Letzter Versuch» und «Letzter Fehler» helfen bei einer fehlgeschlagenen Übertragung; «Zugestellt am» zeigt den erfolgreichen Abschluss. Ein übersprungener Versuch erhöht die Spalte «Versuche» nicht, weil dabei keine Verbindung zum Gegenpart aufgebaut wurde.

| Versuchsstatus | Bedeutung |
|----------------|-----------|
| In Warteschlange | Die Nachricht ist eingereiht, aber für diesen Gegenpart noch nicht geprüft worden. |
| Wartet auf Handshake | Die für das Senden nötigen Zugangsdaten fehlen. Nach einem neuen Handshake kann die Zustellung fortgesetzt werden. |
| Wiederholbarer Fehler | Eine Übertragung wurde versucht und ist fehlgeschlagen. Die Hintergrundaufgabe versucht es später erneut. |
| Zugestellt | Der Gegenpart hat die Nachricht angenommen. |
| Gegenstelle ausser Betrieb | Der Gegenpart wird endgültig nicht mehr kontaktiert. |
| Abgelaufen | Die Ankündigung ist nicht mehr wirksam und wird deshalb nicht mehr zugestellt. |

{{< mermaid >}}
stateDiagram-v2
    [*] --> Warteschlange
    Warteschlange --> WartetAufHandshake: Zugangsdaten fehlen
    Warteschlange --> WiederholbarerFehler: Übertragung fehlgeschlagen
    Warteschlange --> Zugestellt: Nachricht angenommen
    WartetAufHandshake --> WiederholbarerFehler: Übertragung nach Handshake fehlgeschlagen
    WartetAufHandshake --> Zugestellt: Nachricht nach Handshake angenommen
    WiederholbarerFehler --> WiederholbarerFehler: Erneuter Versuch fehlgeschlagen
    WiederholbarerFehler --> Zugestellt: Erneuter Versuch erfolgreich
    Warteschlange --> AusserBetrieb: Gegenpart eingestellt
    WartetAufHandshake --> AusserBetrieb: Gegenpart eingestellt
    WiederholbarerFehler --> AusserBetrieb: Gegenpart eingestellt
    Warteschlange --> Abgelaufen: Ankündigung nicht mehr wirksam
    WartetAufHandshake --> Abgelaufen: Ankündigung nicht mehr wirksam
    WiederholbarerFehler --> Abgelaufen: Ankündigung nicht mehr wirksam

    state "In Warteschlange" as Warteschlange
    state "Wartet auf Handshake" as WartetAufHandshake
    state "Wiederholbarer Fehler" as WiederholbarerFehler
    state "Gegenstelle ausser Betrieb" as AusserBetrieb
{{< /mermaid >}}

Für die Kennzeichnung in **«GT Message»** werden alle Empfänger einer Nachricht zusammengefasst. Sobald mindestens ein Gegenpart die Nachricht angenommen hat, gilt sie insgesamt als zugestellt, auch wenn andere Ziele nicht erreicht wurden. Eine rote Fehlermarkierung erscheint erst, wenn alle Ziele endgültig ohne Erfolg abgeschlossen sind; solange noch eine Zustellung möglich ist, bleibt die Nachricht ausstehend. Für eine genaue Beurteilung einer Rundsendung ist deshalb der Bereich «Zustellversuche» massgebend.

Bei Wartungs- und Betriebseinstellungsankündigungen endet ein offener Versuch spätestens dann, wenn die Ankündigung nicht mehr wirksam ist. Der vollständige Ablauf ist unter [Wartung und Betriebseinstellung](../setup/availability/#zustellung-der-ankündigungen) beschrieben.

## Hintergrundaufgaben
Mehrere Hintergrundaufgaben erledigen GTNet-Operationen automatisch. Diese können im [Aufgaben-Datenänderungsmonitor](../../taskdatachangemonitor/) überwacht werden:

| Aufgaben-ID | Name | Beschreibung |
|-------------|------|--------------|
| 20 | Online-Status verfolgen | Prüft kurz nach dem Serverstart einmalig den Online- und Ausgelastet-Status aller konfigurierten Gegenparts; Online gilt nur bei einer erfolgreichen GTNet-Protokollantwort, Gegenparts ohne abgeschlossenen ausgehenden Handshake werden auf «Unbekannt» gesetzt. Gegenparts, die ausser Betrieb sind oder sich in einem angekündigten Wartungsfenster befinden, werden übersprungen. Eine einzelne Instanz kann jederzeit über den Kontextmenüpunkt «Status jetzt prüfen» in der [GTNet-Server-Übersicht](../setup/) neu geprüft werden. |
| 22 | Austauschprotokolle aggregieren | Fasst Austauschprotokolleinträge zu täglichen, wöchentlichen, monatlichen und jährlichen Perioden zusammen |
| 23 | Konfigurationen synchronisieren | Synchronisiert Austauschkonfigurationen mit verbundenen Gegenparts und aktualisiert Lieferantendetails |
| 24 | Einstellungsänderungen übertragen | Sendet Einstellungsaktualisierungen an alle verbundenen Gegenparts bei lokalen Änderungen |
| 25 | Zukünftige Nachrichten zustellen | Verarbeitet geplante Nachrichten wie Wartungsankündigungen und setzt Instanzen, deren angekündigtes Datum der Betriebseinstellung erreicht ist, auf «Ausser Betrieb» |

Für detaillierte Beschreibungen jeder Aufgabe siehe [Hintergrundaufgaben](../../taskdatachangemonitor/taskdescription/).
