---
title: "Gebühren in der Wiederholung"
date: 2026-09-24T12:00:00+02:00
draft: false
weight: 15
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.38.0" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Eine [historische Wiederholung](../) modelliert **Courtagen** für jeden Kauf und Verkauf, wiederkehrende **Depotgebühren** und prozentuale **Währungsaufschläge**. Sie stammen aus demselben Gebührenmodell. Sie müssen für die Wiederholung deshalb nichts Zusätzliches erfassen. Diese Seite erklärt, welches Modell gilt, wie die Gebühren gebucht werden und was Sie beim Start angeben müssen. Wie ein Gebührenmodell geschrieben wird, beschreibt der [Handelsplattform Plan]({{% ref "/basedata/tradingplatformplan" %}}), den Währungstarif der Abschnitt [Aufschläge bei der Währungsumrechnung]({{% ref "/basedata/tradingplatformplan#aufschläge-bei-der-währungsumrechnung" %}}). Transaktionssteuern wie die Schweizer Stempelsteuer gehören nicht zum Gebührenmodell. Sie schätzt das [Simulationssteuermodell]({{% ref "/algoalert/historicalrun/taxmodel" %}}), wenn **Simulationssteuermodelle anwenden** eingeschaltet ist; ob die Stempelsteuer anfällt, bestimmt das [Händlerland]({{% ref "/basedata/tradingplatformplan#händlerland" %}}) des Handelsplattform Plans.

## Welches Gebührenmodell gilt
Eine Simulationsumgebung ist eine Kopie Ihres Portfolios. Jedes Depot der Kopie behält seinen Handelsplattform Plan und sein allfälliges eigenes Gebührenmodell. Für Courtagen und Depotgebühren bestimmt GT das massgebende Modell wie folgt:
{{< mermaid >}}
graph TD
    A["Depot der Simulation"] --> B{"Eigene Courtagenregeln<br/>des Depots?"}
    B -->|ja| C["Courtagen und Depotgebühren des Depots gelten"]
    B -->|nein| D{"Gebührenmodell im<br/>Handelsplattform Plan?"}
    D -->|ja| E["Courtagen und Depotgebühren des Plans gelten"]
    D -->|nein| F["Keine Courtagen und<br/>keine Depotgebühren"]
{{< /mermaid >}}
Ein [depotspezifisches Gebührenmodell]({{% ref "/tenantportfolio/securityaccounts#gebührenmodell" %}}) ersetzt Courtagen und Depotgebühren gemeinsam, wenn es Courtagenregeln enthält. Übernehmen Sie beim Überschreiben dieses Teils deshalb sowohl die Courtagenregeln als auch den Abschnitt der Depotgebühren. Der Währungstarif wird getrennt vererbt; ein Depotdokument nur mit Währungstarif behält die Courtagen und Depotgebühren des Plans.

Beim Start hält GT die Modelle aller Depots fest. Ändern Sie während eines laufenden Durchgangs einen Plan, rechnet die Wiederholung trotzdem mit dem Stand beim Start. Umgekehrt erzeugt das Speichern eines Modells nie Gebühren in einem echten Depot.

## Courtagen
Für jede Ausführung ermittelt GT die Courtage aus den Regeln des massgebenden Modells, und zwar mit dem Datum der Ausführung. Bei einem Modell mit Zeitperioden gilt also der Tarif, der an jenem Tag gültig war. Die Courtage wird in der Währung des Wertpapiers berechnet, so wie bei einer gewöhnlichen Transaktion.

Kann das Modell für eine Ausführung keinen Betrag liefern, bricht die Wiederholung mit einer Fehlermeldung ab. Das ist der Fall, wenn keine Zeitperiode das Ausführungsdatum abdeckt, wenn keine Regel zutrifft oder wenn das Ergebnis negativ ist. GT behandelt ein solches Geschäft bewusst nicht als kostenlos, weil sonst gerade die nicht beschriebenen Geschäfte die günstigsten der Wiederholung wären. Decken Sie deshalb mit einer Auffangregel (Bedingung `"true"`) und lückenlosen Perioden den ganzen Simulationszeitraum ab.

Freikontingente wie ein freier Trade pro Quartal berücksichtigen auch die Käufe und Verkäufe, die im selben Kalenderjahr vor dem Eröffnungsdatum gebucht wurden. Ein Kontingent verbrauchen nur Ausführungen, die tatsächlich gebucht werden, nicht blosse Berechnungen zur Grösse eines Auftrags. Die Zählung gilt je Depot; ein Kontingent über mehrere Depots derselben Bankbeziehung lässt sich nicht abbilden.

## Depotgebühren
Depotgebühren werden nur berechnet, wenn das massgebende Modell einen Abschnitt `custody` enthält. Fehlt er, gelten die Depotgebühren dieses Depots als **nicht modelliert** und nicht als kostenlos. Ein kostenloses Depot beschreiben Sie mit einer datierten Periode mit dem Betrag `"0"`. Das Modell muss den ganzen Simulationszeitraum abdecken. Eine ungeklärte Periode (`UNRESOLVED`), eine fehlende historische Periode oder fehlende Kurse für die Bewertung stoppen die Wiederholung.

Die Gebühr wird dem Geldkonto belastet, das im selben Portfolio liegt, auf die Gebührenwährung lautet und während der Wiederholung aktiv bleibt. Gibt es kein solches Konto oder mehrere, bestimmen Sie es in den Eröffnungsbeständen mit `cashaccount` (siehe unten). Lautet das gewählte Konto auf eine andere Währung, rechnet GT mit dem historischen Devisenkurs um. Reicht das Guthaben nicht, stehen die üblichen begrenzten internen Geldtransfers zur Verfügung. Kann die Gebühr trotzdem nicht bezahlt werden, schlägt die Wiederholung mit einer verständlichen Meldung fehl.

Bis zur Abrechnung laufen Depotgebühren als offene Abgrenzung auf. Diese Abgrenzung vermindert das simulierte Vermögen bereits vor der Belastung. Mit der Abrechnung sinkt das Bargeld und die Abgrenzung verschwindet, sodass die Gebühr nicht doppelt zählt. Endet die Wiederholung vor der nächsten Rechnung, bleibt die Abgrenzung als Verpflichtung im Ergebnis.

Eine Gebühr je Börse zählt in der ersten Abrechnungsperiode die Börsen der beim Eröffnungsdatum gehaltenen Positionen und jene der Geschäfte, die in dieser Periode schon vor dem Eröffnungsdatum gebucht wurden. Das gilt auch dann, wenn diese Positionen vor Periodenende verkauft werden.

Zwei Tarifänderungen kann die Wiederholung nicht verarbeiten, sie weist den Start ab. Wechselt die Gebührenwährung innerhalb des Simulationszeitraums, braucht es eine neue Simulation. Ändert sich ein belasteter Tarif innerhalb einer Abrechnungsperiode, müssen Sie das in einem depotspezifischen Modell mit passend aufgeteilten Perioden abbilden.

## Eröffnungsbestände Depotgebühren
Beginnt eine Simulation mitten in einer Abrechnungsperiode, sind bis zum Eröffnungsdatum bereits Gebühren aufgelaufen, die GT nicht kennen kann. Diese geben Sie im Dialog der Wiederholung im Feld **Eröffnungsbestände Depotgebühren (YAML)** an. Die Schlüssel sind die Namen der Depots **in der Simulation**. Der [YAML-Editor]({{% ref "/intro/userinterface#yaml-editor" %}}) schlägt innerhalb eines Eintrags die passenden Felder vor und erklärt sie. Das folgende Beispiel ist keine Vorgabe; ersetzen Sie Namen und Beträge durch den Zustand am Eröffnungsdatum:
```yaml
Mein Depot:
  accruedFees: 0
  remainingCredits: 0
  billedThisYear: 0
  assumption: "Bestände anhand der Eröffnungsabrechnung geprüft."
```
| Feld | Bedeutung |
|------|-----------|
| `accruedFees` | Bis zum Eröffnungsdatum aufgelaufene, noch nicht belastete Depotgebühren. |
| `remainingCredits` | Noch nicht verbrauchtes Courtagenguthaben der laufenden Abrechnungsperiode. Darf das Guthaben je Periode nicht übersteigen. |
| `billedThisYear` | Im laufenden Kalenderjahr bereits abgerechnete Depotgebühren, für eine jährliche Obergrenze. |
| `assumption` | Kurze Begründung, woher die Beträge stammen. |
| `cashaccount` | Optional: Kennung des Geldkontos in der Simulation, dem die Gebühren belastet werden. |

Alle Beträge gelten in der Gebührenwährung und ohne Mehrwertsteuer. Führen Sie ein Depot auf, sind alle drei Beträge und die Begründung Pflicht; auch null muss ausdrücklich stehen. Ohne Eintrag geht GT von null aus. Das genügt allerdings nicht, wenn das Modell Gebühren belastet und die Simulation innerhalb einer Abrechnungsperiode beginnt oder eine jährliche Obergrenze kennt. Dann verlangt der Start einen Eintrag für dieses Depot. Eine vor der Eröffnung bereits bezahlte Gebühr steckt im Eröffnungsbargeld und wird nicht nochmals belastet.

**Prüfen** kontrolliert nur den Aufbau des Dokuments. Beim Start prüft GT zusätzlich, ob die Namen zu aktiven Depots der Simulation mit einem Depotgebührenmodell gehören und ob die Beträge zum Modell passen. Ein unbekannter Name wird abgewiesen.

## Schliessung eines Depots während der Wiederholung
Hat ein Depot ein Aktiv-bis-Datum innerhalb des Simulationszeitraums und wird die Gebühr nachschüssig mitten in einer Abrechnungsperiode fällig, legen Sie im Tarif mit `closingPolicy` fest, was bei der Schliessung belastet wird. Ohne diese Angabe weist GT den Start ab.

| `closingPolicy` | Belastung bei der Schliessung | Zulässig bei Bewertung |
|-----------------|-------------------------------|------------------------|
| `ACCRUED` | Die bis zur Schliessung aufgelaufenen täglichen oder monatlichen Beobachtungen. | täglich, monatlich |
| `FULL_PERIOD` | Die volle feste oder am Periodenende bewertete Gebühr; bewertet wird der Bestand am Schliessungstag. | keine, Periodenende |
| `PRORATED` | Feste und am Periodenende bewertete Gebühren sowie Minimum und Maximum je Rechnung, anteilig nach verstrichenen Kalendertagen. | alle |

Eine Kombination ausserhalb dieser Tabelle lehnt die Prüfung des Modells ab. Vorausbezahlte Gebühren werden bei einer Schliessung nicht erstattet.

## Courtagenguthaben
Manche Broker schreiben einen Teil der Depotgebühr als Guthaben für spätere Courtagen gut. In der Wiederholung vermindert ein solches Guthaben nur die Courtage berechtigter Geschäfte, nie Transaktionssteuern. Es ist kein Bargeld und erhöht das Vermögen nicht. Nicht verbrauchtes Guthaben verfällt am Ende der Abrechnungsperiode, und nur erfolgreich gebuchte Geschäfte verbrauchen es.

Damit das Guthaben richtig verteilt wird, müssen die Ausführungen innerhalb einer Abrechnungsperiode in Datumsreihenfolge verarbeitet werden. Führen unterschiedliche Börsenkalender dazu, dass eine frühere Ausführung nach einer bereits gebuchten späteren folgt, stoppt die Wiederholung mit einer Meldung, statt das Guthaben falsch zuzuordnen.

## Doppelte Belastung vermeiden
Planen Sie dieselbe Depotgebühr nicht zusätzlich als [Konto-Dauerauftrag](../standingorders/). Kennzeichnen Sie einen bestehenden Dauerauftrag für Depotgebühren in seiner Notiz mit `[custody]`. Belastet dieser Dauerauftrag dasselbe Geldkonto, auf das ein Depotgebührenmodell abrechnet, weist GT den Start ab, bis Sie den Dauerauftrag in der Simulation entfernt haben. Andere Kontogebühren gehören nicht zum Depotgebührenmodell; Steuerauszüge, Porto, Überträge und Kontoführungsgebühren sind darin nicht enthalten.

## Im Verlauf und im Ergebnis
Im [Verlauf der Wiederholung](../#verlauf-der-wiederholung) erscheint jede Belastung als **Depotgebühr** mit Periode, Mehrwertsteuer und Quelle des Tarifs, und jede Verwendung eines Guthabens als **Courtagenguthaben verwendet**. Die Belastung selbst ist eine gewöhnliche Gebührenbuchung auf dem Geldkonto, verknüpft mit dem simulierten Depot. Unter den Berechnungsannahmen hält GT fest, ob Depotgebühren modelliert sind.

Eine erneute Wiederholung stellt den Eröffnungszustand wieder her und berechnet alle Gebühren neu, statt frühere Simulationsgebühren zu verdoppeln.

## Aufschläge bei der Währungsumrechnung
Die Wiederholung verwendet auch den beim Start festgehaltenen prozentualen Währungstarif. Dieser wird unabhängig vererbt: Ein Depot kann nur den Währungstarif überschreiben und die Courtagen und Depotgebühren des Plans behalten. Umgekehrt kann es Courtagen und Depotgebühren überschreiben und den Währungstarif des Plans behalten. Eine passende Depotregel mit null Prozent ersetzt den geerbten Aufschlag ausdrücklich.
Bei einem Kauf, Verkauf oder einer Ertragszahlung mit Währungsumrechnung wendet GT den Prozentsatz auf den Tagesschlusskurs des Währungspaars am Ausführungs- beziehungsweise Zahlungstag an. Für Tarifstufen zählt der Nettobetrag vor dem Aufschlag einschliesslich der betreffenden Courtagen, Steuern und Marchzinsen. Finanzierungstransfers verwenden den Tarif des begünstigten Depots und den Schlusskurs des Finanzierungstags. Die reine Darstellung einer Dividende in der Wertpapierwährung und die Bewertung in der Berichtswährung erzeugen keinen zusätzlichen Aufschlag. Zeitliche Kursabweichungen innerhalb eines Tages werden nicht simuliert.
Die Berechnungsannahmen zeigen, ob Umrechnungen nur zum Tagesschlusskurs oder mit Aufschlag aus dem Gebührenmodell erfolgen. Fehlt ein passender Abschnitt, eine Periode, eine Regel oder der Kurs für die Tarifwährung, erfolgt die Umrechnung zum Schlusskurs; bei aktiver Währungsmodellierung werden solche Umrechnungen gezählt und gemeldet. Der Verlauf der Wiederholung zeigt dann **Umrechnung ist durch keinen Währungstarif abgedeckt** einmal je Depot, Umrechnungsrichtung, Umrechnungsart und Grund, und eine Buchung mit Aufschlag nennt in ihren Details Prozentsatz und Regel. Eine ungültige Konfiguration lässt die Wiederholung mit **Währungsaufschlagsmodell fehlgeschlagen** scheitern. **Bezahlter Währungsaufschlag (Mandantenwährung)** zeigt die wirtschaftlichen Kosten der tatsächlich gebuchten Umrechnungen; **Umrechnungen ohne Währungstarif** zählt deren Abdeckungslücken. Abgelehnte Aufträge und vorbereitende Grössenberechnungen erhöhen diese Summen nicht. Festgehaltene Durchgänge aus der Zeit vor der Währungsmodellierung behalten ihre reine Schlusskurskonvention.
Unterstützt werden nur prozentuale Aufschläge ab null und unter 5%; fixe Umrechnungsgebühren und Mindestgebühren gehören nicht zum Modell. Berechnet eine Courtagenregel die Umrechnung bereits anhand der Abrechnungswährung, ergänzen Sie für diese Umrechnungen einen ausdrücklichen Nullaufschlag im Währungstarif des Depots, um eine doppelte Modellierung zu vermeiden. Die [beobachteten Wechselkursabweichungen]({{% ref "/reportportfolio/transactioncosts" %}}) ermöglichen den Vergleich erfasster Buchungen mit dem Tarif; beachten Sie dabei die zeitliche Verzerrung der Beobachtungen.
