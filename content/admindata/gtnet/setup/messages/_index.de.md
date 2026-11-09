---
title: "Nachrichten im Überblick"
date: 2026-10-27T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
{{% callout style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.40.0" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /callout %}}
Diese Seite führt sämtliche GTNet-Nachrichten auf und beschreibt, was sie auf der eigenen und auf der Gegenseite bewirken. Selbst versendet werden Nachrichten in der Server-Übersicht von [GTNet und Nachrichten](../) über den Kontextmenüpunkt **«GTNet Nachrichten versenden»**. Eine eingegangene Anfrage wird im Bereich «GT Message» der aufgeklappten Zeile mit **«Antworten»** beantwortet, und eine gesendete Ankündigung zur Nichtverfügbarkeit wird dort mit **«Widerrufen»** zurückgenommen. Welche Nachricht im Dialog gerade angeboten wird, hängt vom Zustand der Verbindung ab und ist auf der Seite [GTNet und Nachrichten](../) im Abschnitt «Verfügbare Nachrichten nach Zustand» beschrieben.

### Nachrichten an einen einzelnen Gegenpart
| Nachricht | Art | Wirkung |
|---|---|---|
| Erste Kontaktaufnahme | Anfrage | Beide Instanzen tauschen ihre Zugangsdaten aus. Erst danach ist eine weitere Kommunikation möglich. |
| Token-Erneuerung | Anfrage | Ersetzt die Zugangsdaten beider Richtungen durch neue. Das abgelöste Token bleibt eine Übergangszeit gültig. Lehnt der Gegenpart ab, gilt wieder das bisherige Token. |
| Abfrage dieser Serverliste | Anfrage | Bittet den Gegenpart um die ihm bekannten Instanzen. Wird sie erlaubt, werden diese Instanzen in die eigene Liste übernommen. |
| Meine Serverliste darf nicht mehr abgefragt werden | Ankündigung | Entzieht dem Gegenpart die früher erteilte Erlaubnis zur Abfrage der eigenen Serverliste. Eine spätere Abfrage braucht eine neue Antwort. |
| Anfrage für Datenaustausch | Anfrage | Bittet um den Austausch der im Feld **«Auszutauschende Datenarten»** gewählten Datenarten. Angeboten werden nur Datenarten, die der Gegenpart annimmt und die noch nicht ausgetauscht werden. |
| Datenaustausch widerrufen | Ankündigung | Beendet den Austausch der im Feld **«Auszutauschende Datenarten»** gewählten Datenarten. Angeboten werden nur Datenarten mit aktivem Austausch. |
| Admin-Nachricht | Ankündigung | Ein Freitext an die Administratoren des Gegenparts. Siehe [Admin-Nachrichten](../msgadmin/). |

Der Widerruf eines Datenaustauschs wirkt sofort und nicht erst zu einem späteren Zeitpunkt. Die eigene Instanz schaltet den Austausch der gewählten Datenarten ab, und der Gegenpart schliesst diese Datenarten auf seinem Eintrag der eigenen Instanz. Für eine erneute Aufnahme braucht es wieder eine «Anfrage für Datenaustausch». Ein Widerruf ohne gewählte Datenart wird nicht versendet, denn er würde auf keiner Seite etwas beenden.

### Nachrichten an alle Gegenparts
Diese Ankündigungen gehen gleichzeitig an alle verbundenen Gegenparts.
| Nachricht | Wirkung |
|---|---|
| Server ist jetzt offline | Die Gegenparts führen die eigene Instanz sofort als offline. |
| Server befindet sich während des Zeitraums im Wartungsmodus | Kündigt ein Wartungsfenster an, in dem die eigene Instanz nicht kontaktiert wird. |
| Der Serverbetrieb wird ab diesem Datum eingestellt | Kündigt das Ende der eigenen Instanz an. Ab dem Datum wird sie bei den Gegenparts als «Ausser Betrieb» geführt. |
| Wartung abgesagt | Hebt ein angekündigtes Wartungsfenster auf. Wird mit **«Widerrufen»** auf der gesendeten Ankündigung ausgelöst. |
| Betriebseinstellung abgesagt | Hebt eine angekündigte Betriebseinstellung auf. Wird ebenfalls mit **«Widerrufen»** ausgelöst. |

Die Einzelheiten zu Wartung und Betriebseinstellung sind unter [Wartung und Betriebseinstellung](../availability/) beschrieben.

### Antworten auf Anfragen
Auf eine Anfrage antwortet entweder ein Administrator mit **«Antworten»** oder eine Regel der [Automatischen Nachricht](../../autoanswer/). Bis die Antwort eintrifft, zeigt die anfragende Seite den Status «Antwort erwartet».
| Anfrage | Annahme | Ablehnung |
|---|---|---|
| Erste Kontaktaufnahme | Kontaktaufnahme erfolgreich | Kontaktaufnahme abgelehnt |
| Token-Erneuerung | Token-Erneuerung akzeptiert | Token-Erneuerung abgelehnt |
| Abfrage dieser Serverliste | Abfrage meiner Serverliste erlauben | Abfrage meiner Serverliste nicht erlauben |
| Anfrage für Datenaustausch | Datenanfrage ist angenommen | Datenanfrage ist abgelehnt |

Die Antwort **«Kontaktaufnahme abgelehnt - Server nicht in der Liste»** kann nicht von Hand gewählt werden. Sie wird automatisch gesendet, wenn sich eine unbekannte Instanz meldet und beim eigenen Eintrag **«Server-Erstellung erlauben»** nicht aktiv ist. Eine Ablehnung kann zudem eine Wartezeit enthalten, während der eine erneute Anfrage derselben Art ohne weitere Prüfung abgelehnt wird.

### Rückmeldungen des Systems
Neben den eigentlichen Antworten gibt es Rückmeldungen, die eine Instanz selbständig sendet. Sie entscheiden nichts über die Anfrage und erscheinen deshalb nicht als Antwort in der Nachrichtenliste.

**Empfangen und verarbeitet** bestätigt, dass eine Ankündigung oder Admin-Nachricht angekommen ist.

**Angenommen, ein Administrator entscheidet** bedeutet, dass die Anfrage angekommen ist, aber keine Regel der Automatischen Nachricht gepasst hat. Die Anfrage bleibt auf der anfragenden Seite mit «Antwort erwartet» offen, bis der Administrator des Gegenparts entschieden hat. Seine Antwort trifft dann als eigene Nachricht ein.

**Nicht verarbeitet** bedeutet, dass der Gegenpart die Nachricht abgewiesen hat, etwa weil sie ungültig war oder die Verbindung nicht mehr besteht. Die eigene Nachricht gilt dann als nicht zugestellt.

**Tägliches Anfragelimit für diesen Server erreicht** bedeutet, dass das Kontingent des Gegenparts für den laufenden UTC-Tag aufgebraucht ist. Die Anfrage wurde nicht bearbeitet und gilt wie bei «Nicht verarbeitet» als nicht zugestellt. Sie bleibt also nicht als offene Anfrage stehen und kann gelöscht werden. Siehe [Abfragelimits](../../requestlimits/).

**Einstellungen aktualisiert** sendet die eigene Instanz von selbst an alle Gegenparts, sobald sich beim eigenen Eintrag das **«Tägliche Abfragelimit»**, die eingeschränkte Nutzung oder die Austauscheinstellungen einer Datenart ändern.

### Statusprüfung mit «Ping»
Die Statusprüfung wird nicht über den Dialog versendet, sondern über den Kontextmenüpunkt **«Status jetzt prüfen»** oder automatisch durch die Hintergrundaufgabe «Online-Status verfolgen». Die Antwort des Gegenparts bestimmt nicht nur dessen Online-Status. Sie enthält auch, was der Gegenpart über sich selbst bekanntgibt, und übernimmt diese Angaben in seinen Eintrag: **«Zeitzone»**, **«In Serverlisten weitergeben»**, **«Tägliches Abfragelimit»** sowie je Datenart, ob er Anfragen annimmt, und das Abfragelimit. Ein Gegenpart, mit dem nur Statusprüfungen ausgetauscht werden, behält dadurch keine veralteten Angaben.

### Nachrichten des Datenaustauschs
Sobald ein Datenaustausch vereinbart ist, verständigen sich die Instanzen mit weiteren Nachrichten, die nie im Dialog angeboten werden. Hintergrundaufgaben und die Wertpapiersuche senden sie selbständig, und die Antwort kommt unmittelbar zurück. Sie erscheinen nicht in der Nachrichtenliste. Ihre Häufigkeit und ihr Ergebnis zeigt das [Austauschprotokoll](../../exchangelog/).
| Bereich | Nachrichten |
|---|---|
| Intraday-Kurse | «Intraday Preise anfordern» mit der Antwort darauf, «Intraday-Preis-Push» mit der Bestätigung sowie «Anfrage überschreitet maximales Instrumentenlimit» |
| Historische Kurse | «Historische Kurse anfordern» mit der Antwort darauf, «Historische Kurse übertragen» mit der Bestätigung, «Anfrage Abdeckungsmetadaten historischer Kurse» mit der Antwort darauf sowie «Anfrage überschreitet maximales Limit für historische Daten» |
| Abgleich | «Exchange-Sync-Anfrage» und «Exchange-Sync-Antwort», mit denen zwei Instanzen ihre Austauschkonfiguration der Instrumente abgleichen |
| Wertpapier-Metadaten | «Wertpapiersuche-Anfrage» mit den Antworten «Wertpapiersuche-Antwort», «Wertpapier nicht gefunden» und «Wertpapiersuche abgelehnt» sowie «Anfrage der Metadaten für mehrere Wertpapiere» mit der Antwort darauf |

Die Abläufe sind unter [Austausch letzter Preis](../../exchange/lastprice/) und [Austausch historische Kurse](../../exchange/historicalprice/) beschrieben.
