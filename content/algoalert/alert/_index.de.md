---
title: "Alarme"
date: 2026-09-30T12:00:00+02:00
draft: false
weight: 30
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.38.0" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Das Alarmsystem in GT informiert den Benutzer, wenn konfigurierte Bedingungen auf Wertpapieren erfüllt werden. Alarme können in zwei Kontexten erstellt werden: als eigenständige Wertpapier-Alarme oder als Alarme innerhalb einer regelbasierten Strategie. In beiden Fällen entscheidet der Benutzer selbst über eine allfällige Transaktion, denn GT handelt nie selbständig.

## Eigenständige Wertpapier-Alarme
Eigenständige Alarme werden direkt auf einem Wertpapier erstellt, ohne dass eine regelbasierte Strategie erforderlich ist. Über das Kontextmenü eines Wertpapiers in einer Watchlist, in der Ansicht eines Depots oder in den Berichten [Depots](../../reportportfolio/securityaccountreport/) und [Anlageklassen mit Cash](../../reportportfolio/securitycashaccountreport/) wählen Sie **Alarm hinzufügen...**, worauf sich der Dialog **Strategie Definition** öffnet. Den zugehörigen Eintrag **Strategie Wertpapier**, der das Wertpapier mit seinen Alarmen verbindet, legt GT selbständig an; brechen Sie den Dialog ab, ohne eine Strategie zu erfassen, bleibt davon nichts zurück. In einer Simulationsumgebung und für einen Benutzer mit reinem Lesezugriff fehlt der Eintrag. Diese Alarme werden unabhängig von einer regelbasierten Strategie ausgewertet und eignen sich für die einfache Überwachung einzelner Wertpapiere.

## Alarme in einer regelbasierten Strategie
Innerhalb einer **Portfoliobasierten Strategie** können Strategien auf den Ebenen Portfoliobasierte Strategie, Anlageklasse und Wertpapier zugewiesen werden, siehe [Regelbasierter Handel](../algo/). Ist die Bedingung einer solchen Strategie erfüllt, erzeugt das System ebenfalls einen Alarm. Eine Strategie auf der Portfoliobasierten Strategie wird für die Wertpapiere ihrer Watchlist ausgewertet, eine Strategie auf einer Anlageklasse für deren Wertpapiere. Laufend ausgewertet werden diese Alarme nur für die Portfoliobasierte Strategie, die der [Portfolioüberwachung](../algo/#portfolioüberwachung) zugewiesen ist; eine [historische Simulation](../historicalrun/) wertet dagegen jede Portfoliobasierte Strategie aus. Jeder Alarm hat einen eigenen Schalter **Alarme aktiviert**, den Sie in der Baumtabelle der Portfoliobasierten Strategie setzen. Ein Alarm, bei dem im Dialog **Strategie Definition** das Kontrollkästchen **Aktiv** nicht gesetzt ist, gilt als Entwurf und wird nie ausgewertet. Auch die Portfolio-Neugewichtung auf der Portfolio-Strategie stellt ihre Empfehlungen als Nachrichten zu; wann solche entstehen, beschreibt [Portfolio-Neugewichtung](../strategy/rebalancing/). Verlässt die Aufteilung der überwachten Strategie zwischen zwei Kontrollzeitpunkten ihre Toleranz, meldet GT dies einmal als **Allokation ausserhalb der Toleranz**, siehe [Allokation ausserhalb der Toleranz](../strategy/rebalancing/#allokation-ausserhalb-der-toleranz).

## Alarmarten
Für Wertpapiere stehen sechs Alarmarten zur Verfügung. Die Angaben in Klammern hinter einem Feldnamen sind die in GT verwendeten Kurzbezeichnungen.

| Alarmart | Verfügbare Ebenen | Felder und geprüfte Bedingung |
|---|---|---|
| **Absoluter Gewinn/Verlust** | Wertpapier | **Unters Limit (L)** und **Oberes Limit (U)** ab 0. Beide Grenzen sind einzeln verwendbar; ein Alarm entsteht, sobald der Kurs eine davon durchbricht. |
| **Bestand Gewinn/Verlust** | Portfoliobasierte Strategie, Anlageklasse, Wertpapier | **Gewinn (G)** und **Verlust (L)** als Prozentwert von 1 bis 500 sowie die Kursgrenzen **Oberes Limit (U)** und **Unters Limit (L)** ab 0; mindestens ein Feld ist auszufüllen. Gemessen wird der Gewinn oder Verlust der tatsächlich gehaltenen Position gegenüber ihrem Einstandswert. Ohne offene Position findet keine Auswertung statt. |
| **Gewinn/Verlust in einer Periode** | Wertpapier | **Periode (P)** von 1 bis 999 Tagen sowie **Gewinn (G)** und **Verlust (L)** von 1 bis 500 Prozent. Verglichen wird mit dem letzten Schlusskurs an oder vor dem Tag, auf den die Periode zurückweist. |
| **Gleitender Durchschnitt Kreuzung** | Wertpapier | **Indikatortyp (I)** mit der Auswahl **Einfacher gleitender Mittelwert** oder **Exponentieller gleitender Durchschnitt**, **Periode (P)** von 1 bis 999 und **Kreuzungsrichtung (C)** mit der Auswahl **Darüber** oder **Darunter**. Ein Alarm entsteht, wenn der Kurs den gleitenden Durchschnitt in der gewählten Richtung kreuzt. |
| **RSI Schwellenwert** | Wertpapier | **RSI Periode (R)** von 1 bis 999 sowie **Unterer Schwellenwert (L)** und **Oberer Schwellenwert (U)** von 0 bis 100. |
| **Benutzerdefinierter Ausdruck** | Wertpapier | **Ausdruck (E)** mit höchstens 500 Zeichen. Zur Verfügung stehen die Werte `price`, `prevClose`, `open`, `high`, `low` und `volume` sowie die Funktionen `SMA(n)`, `EMA(n)` und `RSI(n)`. Der Alarm wird ausgelöst, wenn der Ausdruck zutrifft. |

Dieselbe Alarmart darf auf einem Wertpapier mehrfach erfasst werden, beispielsweise zwei absolute Kursgrenzen mit unterschiedlichen Werten. Nur die Portfolio-Neugewichtung und der Mean-Reversion-Dip sind je Ebene einmalig.

## Alarm-Übersicht
Die Übersicht der eigenständigen Alarme eines Mandanten öffnet sich mit dem Knoten **Regelbasierter Handel** im Navigationsbaum. Sie ist als Baumtabelle aufgebaut: Auf der obersten Ebene steht das Wertpapier, darunter als Kindknoten die erfassten Alarme mit ihrer Alarmart in der Spalte **Strategiename**. Alarme, die zu einer Portfoliobasierten Strategie gehören, erscheinen hier nicht; sie werden in der Baumtabelle ihrer Strategie verwaltet.

Die Spalte **Alarme aktiviert** enthält auf der Zeile eines Alarms ein Kontrollkästchen; die Zeile des Wertpapiers hat keines. Eine Änderung wird sofort gespeichert. So lässt sich ein Alarm vorübergehend stilllegen, ohne die Konfiguration zu verlieren. Wird er wieder eingeschaltet, bestimmt GT einen neuen Ausgangswert, damit eine Bewegung während der Pause nicht als Kreuzung gemeldet wird.

Über das Kontextmenü der Wertpapierzeile erfassen Sie mit **Erstellen Strategie Definition** einen weiteren Alarm oder entfernen mit **Löschen Strategie Wertpapier** das Wertpapier samt allen seinen Alarmen. Auf der Zeile eines Alarms stehen **Bearbeiten Strategie Definition** und **Löschen Strategie Definition** zur Verfügung. Beim Bearbeiten lassen sich nur die Parameter ändern, nicht aber die Alarmart.

## Alarm-Auswertung
Alle sechs Arten von Wertpapieralarmen werden im Hintergrund geprüft. Der Administrator legt in den globalen Einstellungen mit `gt.algo.alarm.evaluation.interval.hours` ein Intervall von 2 bis 6 ganzen Stunden fest. Der Standardwert beträgt 4 Stunden. Änderungen gelten ohne Neustart. Eine kürzlich erfolgreiche Auswertung oder ein Prüfversuch im Hintergrund verschiebt den nächsten Hintergrundversuch. Neue Innertag-Kurse können eine frühere Auswertung auslösen. Die Planung prüft alle fünf Minuten auf fällige Arbeit. Dadurch kann die Auswertung bis zu fünf Minuten später beginnen, zuzüglich einer allfälligen Wartezeit in der Aufgabenwarteschlange. Die eigentliche Arbeit erledigt die [Hintergrundaufgabe 50]({{% relref "/admindata/taskdatachangemonitor/taskdescription/#JOB50" %}}).
Gewöhnliche Wertpapiere werden während der Öffnungszeiten ihrer Börse geprüft. Massgebend sind deren Zeitzone und hinterlegte Feiertage. Am Wochenende finden in dieser Zeitzone keine automatischen Prüfungen statt. Als Kryptowährungen klassifizierte Wertpapiere werden an allen Wochentagen geprüft. Diese Ausnahme gilt für Wertpapiere, die das Alarmsystem bereits unterstützt; sie fügt keine Alarme für Kryptowährungs-Währungspaare hinzu.
Nach Börsenschluss und Ablauf der Verzögerung des Datenlieferanten unternimmt GT einen abschliessenden Prüfversuch, sofern der Schlusskurs noch nicht ausgewertet wurde. Dieser Versuch kann vor Ablauf des eingestellten Intervalls stattfinden. Ein fehlgeschlagener Abschlussversuch wird weder wiederholt noch am Wochenende nachgeholt.
GT verwendet einen genügend aktuellen Kurs aus der laufenden Handelssitzung oder fordert einen neuen Kurs an. Mehrere Alarme desselben Wertpapiers teilen sich den heruntergeladenen Kurs. Bleibt dieser zu alt oder fehlen benötigte historische Kurse oder Börsenangaben, entsteht kein Alarm. GT hält den Grund fest und bewahrt den letzten gültigen Vergleichswert für die Erkennung einer Kreuzung. Nach einem fehlgeschlagenen Hintergrundversuch wartet GT das eingestellte Intervall ab.
Kurs-, Durchschnitts- und RSI-Kreuzungen benötigen zunächst einen gültigen Ausgangswert. Die erste gültige Beobachtung legt diesen fest und löst noch keinen Alarm aus. Nach dem Bearbeiten oder erneuten Aktivieren wird der Ausgangswert neu bestimmt, damit eine Zeit der Deaktivierung keine scheinbare Kreuzung erzeugt. Eine Schwellenüberschreitung mit anschliessender Rückkehr zwischen zwei Beobachtungen kann unentdeckt bleiben.
Der folgende Ablauf zeigt den Weg von der fälligen Prüfung bis zur Nachricht.
{{< mermaid >}}
flowchart TD
    S([Alle fünf Minuten: Prüfung auf fällige Alarme]) --> D{Alarm fällig?}
    D -->|Nein| W[Warten bis zum nächsten Intervall]
    D -->|Ja| B{Börse offen oder Kryptowährung?}
    B -->|Nein| W
    B -->|Ja| Q[Kurs je Wertpapier einmal beschaffen]
    Q --> F{Kurs aktuell genug und Daten vollständig?}
    F -->|Nein| R[Grund festhalten, Vergleichswert bewahren, kein Alarm]
    F -->|Ja| E{Bedingung erfüllt?}
    E -->|Nein| W
    E -->|Ja| A{Heute schon gemeldet?}
    A -->|Ja| N[Keine zweite Nachricht]
    A -->|Nein| M[Alarm festhalten und als GT Nachricht zustellen]
{{< /mermaid >}}

## Alarm-Lebenszyklus
Wenn eine Alarm-Bedingung erfüllt wird, hält das System den Alarm mit den Angaben zur ausgelösten Bedingung fest. Anschliessend wird der Benutzer über das interne Nachrichtensystem von GT benachrichtigt, und zwar in der Sprache des Klienten. Der Betreff nennt den Kontext des Alarms, der Nachrichtentext das Wertpapier und die Einzelheiten der Auslösung. Der Benutzer kann die Nachricht lesen, die Situation beurteilen und dann selbständig über die gewünschte Aktion entscheiden. GT führt keine automatischen Transaktionen aus. Konnte eine Nachricht nicht zugestellt werden, wird sie später aus dem festgehaltenen Alarm erneut versendet, ohne dass der Alarm ein zweites Mal ausgelöst wird.

## Deduplizierung
Damit dieselbe Situation nicht mehrfach gemeldet wird, unterscheidet GT einen Alarm nach Klient, Alarm, Wertpapier, Art des Signals, Richtung und Tag. Wiederholte Auswertungen desselben Signals am selben Tag ergeben deshalb nur eine Nachricht. Zwei Wertpapiere unterdrücken sich nie gegenseitig, und eine Kreuzung nach oben und eine nach unten am selben Tag sind zwei getrennte Meldungen.

## Alarmdiagnose
Die Schaltfläche **Alarmdiagnose** über der Alarm-Übersicht öffnet einen Dialog, der zeigt, warum ein Alarm ausgelöst wurde oder eben nicht. Seine drei Register beantworten drei aufeinanderfolgende Fragen. Ein Hinweis über den Registern erinnert daran, dass eine E-Mail mit der Annahme durch den Mailserver als extern zugestellt gilt; ob sie im Postfach angekommen ist, kann GT nicht feststellen. Jede Tabelle zeigt 20 Zeilen je Seite; bei längeren Listen wechseln Sie die Seite unter der Tabelle. In der Filterzeile unter den Spaltenüberschriften lassen sich einzelne Spalten auf bestimmte Werte einschränken. Wählen Sie in einer Spalte mehrere Werte, erscheinen die Zeilen mit einem beliebigen davon.
{{< mermaid >}}
flowchart LR
    A[Auswertung: Liess sich die Regel auswerten?] --> H[Handelsentscheidungen: Was hat der Mean-Reversion-Dip entschieden?]
    H --> B[Benachrichtigungen: Ist die Nachricht angekommen?]
{{< /mermaid >}}
Am unteren Rand des Dialogs führt links das Fragezeichen auf diese Seite. Rechts lädt **Aktualisieren** die Tabellen neu, ohne etwas zu berechnen. **Jetzt auswerten** prüft Ihre eigenen Alarme sofort und berechnet zusätzlich die Portfolio-Neugewichtung und den Mean-Reversion-Dip neu. Anders als die Hintergrundplanung beachtet diese Auswertung die Öffnungszeiten der Börse nicht. Jede sofortige Auswertung und jede erneut gesendete Benachrichtigung zählt zur Limite **Manuelle Alarmaktionen**, siehe [Limiten der Informationsklassen]({{% relref "/admindata/entitylimit" %}}). Einem Benutzer mit reinem Lesezugriff fehlen **Jetzt auswerten** und das Kontextmenü mit **Ausgewählte Benachrichtigung erneut senden**. In einer Simulationsumgebung steht die Alarmdiagnose nicht zur Verfügung, weil Alarme nur das eigene Portfolio überwachen.

### Auswertung
Das Register listet jede Kombination aus Alarm und Wertpapier mit den Spalten **Kontext**, **Wertpapiername**, **Strategietyp**, **Aktiv**, **Auswertungsergebnis**, **Letzter Versuch**, **Letzter Erfolg**, **Kurszeitpunkt** und **Begründung**. Aufgeführt sind die sechs Alarmarten; die Portfolio-Neugewichtung und der Mean-Reversion-Dip erscheinen hier nicht. **Letzter Versuch** und **Letzter Erfolg** werden getrennt geführt, damit ein gescheiterter Versuch nicht wie eine erfolgreiche Prüfung aussieht. **Kurszeitpunkt** nennt den Zeitpunkt des Kurses, auf dem die letzte erfolgreiche Auswertung beruht. Die Tabelle ist nach **Kontext** sortiert; gefiltert werden kann nach **Kontext**, **Strategietyp** und **Auswertungsergebnis**.

| Auswertungsergebnis | Bedeutung |
|---|---|
| **Noch nicht ausgewertet** | Für diesen Alarm fand noch keine Prüfung statt, beispielsweise weil er neu ist oder seine Börse seither nicht geöffnet hatte. |
| **In Bearbeitung** | Die Prüfung läuft gerade. |
| **Ausgewertet** | Die Bedingung wurde vollständig geprüft. Ob dabei ein Alarm entstand, zeigt das Register **Benachrichtigungen**. |
| **Teilweise ausgewertet** | Ein Teil der Bedingung liess sich prüfen, ein anderer nicht. Die **Begründung** nennt den fehlenden Teil. |
| **Nicht verfügbar** | Die Regel liess sich nicht auswerten, und es entstand kein Alarm. Die **Begründung** nennt die Ursache. |

Die folgenden Begründungen erscheinen bei **Nicht verfügbar** oder **Teilweise ausgewertet**:

| Begründung | Bedeutung und Abhilfe |
|---|---|
| **Benötigter Kurs fehlt oder ist veraltet** | Der letzte Kurs ist älter als das eingestellte Auswertungsintervall oder stammt aus einer früheren Handelssitzung. Nach **Jetzt auswerten** am Wochenende, an einem Feiertag oder ausserhalb der Börsenzeiten ist das der Normalfall und kein Fehler; die nächste Prüfung während der Börsenzeiten holt die Auswertung nach. Bleibt die Meldung auch während der Handelszeit bestehen, prüfen Sie den Datenlieferanten des Wertpapiers. |
| **Kursaktualisierung fehlgeschlagen** | Der Datenlieferant hat keinen neuen Kurs geliefert. Prüfen Sie die Einstellungen für den aktuellen Kurs des Wertpapiers. |
| **Instrument hat keine Börse** | Dem Wertpapier ist keine Börse zugeordnet, deshalb kennt GT seine Handelszeiten nicht. |
| **Handelszeiten der Börse fehlen oder sind ungültig** | Bei der Börse fehlen Öffnungs- oder Schlusszeit, oder beide sind gleich. |
| **Zeitzone der Börse ist ungültig** | Die bei der Börse hinterlegte Zeitzone ist unbekannt. |
| **Prozentualer Gewinn/Verlust der Position nicht verfügbar: Einstandswert ist null** | Nur bei **Bestand Gewinn/Verlust** mit **Gewinn (G)** oder **Verlust (L)**: Ohne Einstandswert lässt sich kein Prozentwert berechnen. Die Kursgrenzen **Oberes Limit (U)** und **Unters Limit (L)** werden trotzdem geprüft, das Ergebnis lautet deshalb **Teilweise ausgewertet**. |
| **Auswertung fehlgeschlagen** | Ein unerwarteter Fehler verhinderte die Prüfung. Steht stattdessen ein technischer Text in der Spalte, ist es die unveränderte Fehlermeldung. |

Ausserhalb der Börsenzeiten unternimmt GT im Hintergrund keinen Prüfversuch; die Zeile behält dann das Ergebnis der letzten Prüfung.

### Handelsentscheidungen
Das Register zeigt den aktuellen Vorschlag jedes [Mean-Reversion-Dip](../strategy/meanreversiondip/) der Portfoliobasierten Strategie, die der [Portfolioüberwachung](../algo/#portfolioüberwachung) zugewiesen ist. Andere Strategien, auch die Portfolio-Neugewichtung, erscheinen hier nicht. Ist keine Portfoliobasierte Strategie der Überwachung zugewiesen oder enthält sie keinen Mean-Reversion-Dip, bleibt die Tabelle leer.
Der Mean-Reversion-Dip wird einmal täglich auf Grund des letzten abgeschlossenen Tagesschlusskurses ausgewertet, mit **Jetzt auswerten** auch sofort. Je Strategie und Wertpapier gibt es genau eine Zeile, die jede Auswertung ersetzt; frühere Entscheide sind hier nicht einsehbar. Die Spalten sind **Kontext**, **Strategiename**, **Wertpapiername**, **Bewertungsdatum**, **Aktion**, **Anzahl**, **Betrag**, **Währung**, **Kurs/Div/usw.** und **Begründung**. **Kurs/Div/usw.** ist der Schlusskurs am Bewertungsdatum, **Betrag** steht in der Währung des Mandanten. Das jüngste **Bewertungsdatum** steht zuoberst; gefiltert werden kann nach **Kontext**, **Strategiename** und **Aktion**.

| Aktion | Bedeutung |
|---|---|
| **Kauf** | Die Strategie schlägt einen Kauf der angegebenen Anzahl vor. Auch das Schliessen oder Verkleinern einer Short-Position erscheint als Kauf, denn massgebend ist die Richtung des Auftrags. |
| **Verkauf** | Die Strategie schlägt einen Verkauf vor, beispielsweise eine Gewinnmitnahme, einen Stop-Loss oder bei Short-Positionen einen Einstieg. |
| **Halten** | Es ist nichts zu tun, etwa weil die Einstiegsbedingung nicht erfüllt ist oder die offene Position gehalten wird. |
| **Blockiert** | Eine Handlung wäre fällig, wird aber verhindert, oder die Strategie liess sich nicht auswerten. Die **Begründung** nennt den Grund. |

Nur **Kauf** und **Verkauf** lösen einen Alarm und damit eine Benachrichtigung aus. **Halten** und **Blockiert** sind ausschliesslich in diesem Register sichtbar. Wer sich fragt, weshalb keine Nachricht kam, findet die Antwort deshalb hier. Was die einzelnen Begründungen bedeuten und was Sie dagegen tun können, beschreibt [Ergebnis prüfen](../strategy/meanreversiondip/#ergebnis-prüfen). GT bucht nie selbst eine Transaktion; **Anzahl** und **Betrag** sind Vorschläge.

### Benachrichtigungen
Das Register listet sämtliche festgehaltenen Alarme mit dem Stand ihrer Zustellung, die neueste **Signalzeit** zuoberst. Gefiltert werden kann nach **Kontext**, **Zustellstatus** und **Zustellkanäle**; so finden Sie beispielsweise alle Benachrichtigungen, die eine Prüfung erfordern. Die Spalten sind **Kontext**, **Wertpapiername**, **Signalzeit**, **Zustellstatus**, **Zustellkanäle**, **Zustellversuche**, **Nächster Versuch**, **Interne Zustellung**, **SMTP-Annahme**, **Zustellfehler** und **Signaldetails**. **Interne Zustellung** ist der Zeitpunkt, zu dem die GT Nachricht abgelegt wurde, **SMTP-Annahme** jener, zu dem der Mailserver die E-Mail angenommen hat.

| Zustellstatus | Bedeutung |
|---|---|
| **Ausstehend** | Die Nachricht wartet auf ihre Zustellung. |
| **Wird gesendet** | Die Zustellung läuft gerade. |
| **Wiederholung geplant** | Ein Versuch ist gescheitert. GT versucht es nach 5, 15 und 60 Minuten und danach alle 6 Stunden erneut; **Nächster Versuch** nennt den Zeitpunkt. |
| **Fehlgeschlagen; Prüfung erforderlich** | Nach acht gescheiterten Versuchen gibt GT auf. Die Ursache steht in **Zustellfehler**. |
| **Prüfung erforderlich** | Der Empfänger hat sich seit dem Auslösen geändert, oder eine ältere Benachrichtigung blieb unerledigt. GT sendet sie nicht von sich aus. |
| **Zugestellt** | Alle Zustellkanäle sind erledigt. |
| **Abgebrochen** | Die Strategie wurde gelöscht oder wird nicht mehr überwacht; die Nachricht wird nicht mehr zugestellt. |

Die Zeilen der Tabelle markieren Sie über die Kontrollkästchen. Ist genau eine Benachrichtigung mit **Fehlgeschlagen; Prüfung erforderlich** oder **Prüfung erforderlich** markiert, senden Sie sie über den Eintrag **Ausgewählte Benachrichtigung erneut senden** im Kontextmenü der rechten Maustaste nochmals. Bei jedem anderen Zustellstatus und wenn keine oder mehrere Zeilen markiert sind, ist der Eintrag ausgegraut. Ein bereits erledigter Kanal wird dabei nicht wiederholt. Nach einem Neustart des Servers kann eine Wiederholung eine E-Mail jedoch doppelt versenden.

Nicht mehr benötigte Benachrichtigungen löschen Sie über den Eintrag **Ausgewählte löschen** im selben Kontextmenü; nach einer Bestätigung werden alle markierten Zeilen entfernt. Löschbar ist eine Benachrichtigung erst, wenn ihre Zustellung abgeschlossen ist, also bei **Zugestellt**, **Abgebrochen**, **Fehlgeschlagen; Prüfung erforderlich** und **Prüfung erforderlich**, und wenn sie älter als 10 Tage ist. Bis dahin verhindert sie, dass derselbe Alarm nochmals gesendet wird. Ist auch nur eine markierte Zeile noch nicht löschbar, ist der Eintrag ausgegraut. Alte Benachrichtigungen löscht GT zudem selbständig; wie lange sie aufbewahrt werden, legt der Administrator fest, siehe [Hintergrundaufgaben]({{% relref "/admindata/taskdatachangemonitor/taskdescription/#JOB50" %}}).

## Einschränkungen
Die Alarmart eines bestehenden Alarms lässt sich nicht mehr wechseln; dafür wird ein neuer Alarm erfasst und der alte gelöscht. Währungspaare einer Watchlist können keinen Alarm erhalten, Alarme sind den Wertpapieren vorbehalten. Neben der beschriebenen Hintergrundplanung startet **Jetzt auswerten** in der [Alarmdiagnose](#alarmdiagnose) eine sofortige Auswertung Ihrer eigenen Alarme. Die Anzahl der Portfoliobasierten Strategien, Anlageklassen, Wertpapiere und Strategien je Klient ist über die [Limiten der Informationsklassen]({{% relref "/admindata/entitylimit" %}}) begrenzt. Hat die Installation die Alarmfunktion abgeschaltet, fehlt der Eintrag **Alarm hinzufügen...**, und es wird kein Alarm ausgewertet. Ist zusätzlich der regelbasierte Handel abgeschaltet, fehlt auch der Knoten **Regelbasierter Handel**. Eigenständige Wertpapier-Alarme benötigen nur die Alarmfunktion; Alarme innerhalb einer Portfoliobasierten Strategie werden nur ausgewertet, wenn die Installation auch den regelbasierten Handel eingeschaltet hat.
