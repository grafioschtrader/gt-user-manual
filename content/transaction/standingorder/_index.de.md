---
title: "Daueraufträge"
date: 2026-08-04T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
Daueraufträge ermöglichen die automatisierte Erstellung wiederkehrender Transaktionen. Sie eignen sich besonders für regelmässige Einzahlungen oder Auszahlungen auf Konten sowie für periodische Wertpapierkäufe im Sinne eines Sparplans. Anstatt jede Transaktion manuell zu erfassen, definieren Sie einmalig die Parameter des Dauerauftrags, und GT erstellt die Transaktionen automatisch zum geplanten Zeitpunkt.

{{% notice style="info" title="Transaktionslimit" %}}
Auch die automatischen Ausführungen eines Dauerauftrags zählen zum [Transaktionslimit pro Klient](../). Ist die Grenze erreicht, wird die fällige Ausführung nicht durchgeführt, sondern in der Fehlerliste vermerkt. Sie wird beim nächsten Lauf automatisch nachgeholt, sobald durch das Löschen bestehender Transaktionen wieder Platz vorhanden ist.
{{% /notice %}}

## Zugang im Frontend
Die Daueraufträge befinden sich unter der Mandantenansicht im Reiter **Daueraufträge**. Dieser Reiter enthält zwei Unter-Reiter:
- **Dauerauftrag Konto**: Für wiederkehrende Kontotransaktionen wie Einzahlungen und Auszahlungen.
- **Dauerauftrag Wertpapier**: Für wiederkehrende Wertpapiertransaktionen wie Käufe und Verkäufe.

## Wiederholungskonfiguration
Jeder Dauerauftrag enthält eine Wiederholungskonfiguration, die bestimmt, wann die nächste Transaktion erstellt wird.

### Wiederholungseinheit und Intervall
Die **Wiederholungseinheit** bestimmt das Zeitmass der Wiederholung. Zusammen mit dem **Wiederholungsintervall** ergibt sich der Abstand zwischen den Ausführungen:
- **Tage**: Ausführung alle *n* Tage, wobei *n* das Wiederholungsintervall ist. Die Felder **Tagesposition**, **Ausführungstag** und **Ausführungsmonat** werden deaktiviert, da sie bei tagesbasierten Intervallen nicht relevant sind.
- **Monate**: Ausführung alle *n* Monate. Die **Tagesposition** bestimmt den genauen Tag im Monat. Das Feld **Ausführungsmonat** wird deaktiviert, da es nur bei jährlicher Wiederholung relevant ist.
- **Jahre**: Ausführung alle *n* Jahre. Der **Ausführungsmonat** wird über ein Auswahlfeld mit lokalisierten Monatsnamen gewählt. Zusammen mit der **Tagesposition** bestimmt er den genauen Zeitpunkt.

### Tagesposition
Bei monatlicher oder jährlicher Wiederholung bestimmt die **Tagesposition** den Tag innerhalb des Monats:
- **Bestimmter Tag**: Ausführung am angegebenen **Ausführungstag** (1–28). Dies ist die einzige Tagesposition, bei der das Feld **Ausführungstag** aktiviert ist.
- **Erster Tag**: Ausführung am 1. des Monats. Das Feld **Ausführungstag** wird deaktiviert, da der Tag implizit ist.
- **Letzter Tag**: Ausführung am letzten Tag des Monats. Das Feld **Ausführungstag** wird deaktiviert. Diese Option ist bei unterschiedlichen Monatslängen (28, 29, 30, 31 Tage) besonders praktisch.

### Anpassung an Handelstage
Fällt ein Ausführungsdatum auf einen Tag, an dem nicht gehandelt wird, verschiebt GT die Ausführung automatisch. Die **Anpassung an Handelstage** bestimmt dabei lediglich die Richtung der Verschiebung:
- **Auf früheren Tag verschieben**: Die Ausführung rückt auf den vorangehenden geeigneten Tag.
- **Auf späteren Tag verschieben**: Die Ausführung rückt auf den nächstfolgenden geeigneten Tag.

Welche Tage übersprungen werden, hängt von der Art des Dauerauftrags ab. Bei einem Konto-Dauerauftrag werden nur Wochenenden ausgelassen, denn eine Bank belastet eine Gebühr oder schreibt einen Zins auch an einem Feiertag gut. Bei einem Wertpapier-Dauerauftrag wird zusätzlich der Börsenkalender jener Börse berücksichtigt, an welcher das Wertpapier gehandelt wird. Die Ausführung landet damit immer auf einem Tag, an dem diese Börse tatsächlich geöffnet ist. Ein Sparplan mit dem 1. Januar als Ausführungstag wird auf diese Weise beispielsweise auf den ersten Handelstag des neuen Jahres verschoben.

{{% notice style="note" title="Verschiebung erfolgt erst beim Ausführungslauf" %}}
Der Börsenkalender wird erst ausgewertet, wenn das Ausführungsdatum erreicht ist. Die Spalte **Nächste Ausführung** zeigt deshalb den geplanten Termin und nicht den allenfalls verschobenen Handelstag.
{{% /notice %}}

### Kurstoleranz
Auch an einem regulären Handelstag kann in der Kursdatenbank ein Kurs fehlen. Damit eine solche Lücke nicht sogleich zu einem Ausführungsfehler führt, legt die **Kurstoleranz (Tage)** fest, wie weit GT vom Ausführungsdatum abweichen darf, um einen Kurs oder einen Wechselkurs zu finden. Der Betrag der Zahl ist das Suchfenster in Tagen nach beiden Seiten, das Vorzeichen bestimmt, welche Seite zuerst geprüft wird. Bei 0 muss der Kurs genau am Ausführungsdatum vorliegen. Bei −2 sucht GT zuerst bis zu zwei Tage in der Vergangenheit und erst danach bis zu zwei Tage in der Zukunft, bei 2 ist es umgekehrt. Das Datum der erstellten Transaktion bleibt in jedem Fall das Ausführungsdatum, einzig der verwendete Kurs kann von einem benachbarten Tag stammen.

Den zulässigen Wertebereich legt der Administrator über die globale Einstellung `gt.standing.order.quote.tolerance` in der Form «min:max» fest. Die Vorgabe «-3:3» erlaubt beide Richtungen bis zu drei Tage, «0:3» verbietet den Griff in die Vergangenheit und «0:0» verlangt den Kurs genau am Ausführungsdatum.

### Beispiele
| Konfiguration | Beschreibung |
|---|---|
| Monate, Intervall 1, Bestimmter Tag 15 | Am 15. jedes Monats |
| Monate, Intervall 3, Erster Tag | Am 1. jedes Quartals |
| Monate, Intervall 1, Letzter Tag | Am letzten Tag jedes Monats |
| Jahre, Intervall 1, Monat 6, Bestimmter Tag 1 | Am 1. Juni jedes Jahres |
| Tage, Intervall 14 | Alle 14 Tage |

## Gültigkeit und Deaktivierung
Jeder Dauerauftrag hat ein **Gültig von** und ein **Gültig bis** Datum. Die erste Ausführung erfolgt frühestens am Gültig-von-Datum. Sobald das nächste berechnete Ausführungsdatum nach dem Gültig-bis-Datum liegt, wird der Dauerauftrag automatisch deaktiviert. Ob ein Dauerauftrag noch aktiv ist, lässt sich an der Spalte **Nächste Ausführung** erkennen: Ist ein Datum eingetragen, steht die nächste Ausführung noch an. Ist die Spalte leer, wurde der Dauerauftrag deaktiviert.

## Nachholmechanismus
Falls der Hintergrundprozess für einige Tage nicht ausgeführt wurde, beispielsweise bei einem Serverunterbruch, werden versäumte Ausführungsdaten beim nächsten Lauf nachgeholt. Für jeden fälligen Termin wird eine eigene Transaktion erstellt. Dies gilt auch dann, wenn das Gültig-bis-Datum eines Dauerauftrags zum Zeitpunkt der Verarbeitung bereits in der Vergangenheit liegt, etwa weil der Server während der gesamten Ausführungsperiode offline war. In diesem Fall werden alle versäumten Ausführungsdaten noch verarbeitet, bevor der Dauerauftrag deaktiviert wird. Auf diese Weise gehen keine geplanten Ausführungen verloren.

## Begrenzung
Pro Mandant ist die Anzahl Daueraufträge begrenzt. Der Standardwert beträgt 50 und kann vom Administrator über die globalen Einstellungen angepasst werden.

## Bearbeitungs- und Löscheinschränkungen
Die **Transaktionsart** (z.B. Einzahlung/Auszahlung oder Kaufen/Verkaufen) wird nach dem Erstellen eines Dauerauftrags immer gesperrt. Sie kann unabhängig davon, ob bereits Transaktionen erstellt wurden, nicht mehr geändert werden.

Sobald ein Dauerauftrag Transaktionen erstellt hat, gelten folgende zusätzliche Einschränkungen:
- **Löschen nicht möglich**: Der Dauerauftrag kann nicht gelöscht werden, da die erstellten Transaktionen auf ihn verweisen. Die Löschoption wird im Kontextmenü ausgeblendet.
- **Gültig-von-Datum gesperrt**: Das Gültig-von-Datum kann nicht mehr geändert werden, sobald Transaktionen existieren, da es die Ausführungshistorie verankert.
- **Transaktionsfelder schreibgeschützt**: Nur die Terminierungsfelder (Wiederholungskonfiguration, Gültig-bis-Datum, Notiz) können noch bearbeitet werden. Die übrigen transaktionsspezifischen Felder (Konto, Betrag, Wertpapier, Kosten usw.) sind gesperrt, um die Konsistenz mit den bereits erstellten Transaktionen zu gewährleisten.

## Inaktive Daueraufträge anzeigen
Standardmässig werden inaktive Daueraufträge (solche, die nach Erreichen ihres Gültig-bis-Datums automatisch deaktiviert wurden) in der Tabelle ausgeblendet. Um sie anzuzeigen, verwenden Sie die Option **Inaktive Daueraufträge anzeigen** im Ansichtsmenü. Der Umschalter wechselt zwischen dem Ein- und Ausblenden inaktiver Einträge.

## Erstellen eines Dauerauftrags
Ein neuer Dauerauftrag kann über das **Kontextmenü** in der jeweiligen Dauerauftrags-Tabelle erstellt werden. Zusätzlich besteht die Möglichkeit, einen Dauerauftrag direkt aus einer bestehenden Transaktion zu erstellen. Dazu wird die gewünschte Transaktion in einer Transaktionsansicht selektiert und über das Kontextmenü die Funktion **Dauerauftrag erstellen** aufgerufen. Die Transaktionsdaten wie Konto, Betrag und Transaktionsart werden dabei als Vorlage übernommen.

## Ausführungsfehler
Wenn die Ausführung eines Dauerauftrags fehlschlägt, wird der Fehler in einem Fehlerprotokoll festgehalten. Typische Ursachen für Fehler bei Wertpapier-Daueraufträgen sind ein Kurs, der auch innerhalb der Kurstoleranz nicht gefunden wird, oder eine berechnete Stückzahl von null. Der Dauerauftrag wird in jedem Fall zum nächsten Termin weitergeschaltet, sodass ein einzelner Fehler nicht die gesamte Verarbeitung blockiert.
In der Dauerauftrags-Tabelle zeigt die Spalte **Fehler** die Anzahl der aufgetretenen Ausführungsfehler an. Daueraufträge mit Fehlern können über den Zeilenexpander aufgeklappt werden. Die Fehlertabelle zeigt für jeden fehlgeschlagenen Termin das **Ausführungsdatum** und den **Geschäftsfehler** an. Falls ein unerwarteter technischer Fehler aufgetreten ist, kann die betreffende Fehlerzeile ein weiteres Mal aufgeklappt werden, um den vollständigen Fehlertext einzusehen.

{{% notice style="info" title="Fehler werden beim Löschen bereinigt" %}}
Wird ein Dauerauftrag gelöscht, werden auch alle zugehörigen Fehlerprotokolle automatisch entfernt.
{{% /notice %}}

## Erstellte Transaktionen einsehen
Die Dauerauftrags-Tabelle enthält eine Spalte **Transaktionen**, die die Anzahl der durch den jeweiligen Dauerauftrag erstellten Transaktionen anzeigt. Daueraufträge mit erstellten Transaktionen können über den Zeilenexpander aufgeklappt werden, ähnlich wie bei der Fehleranzeige. Der aufgeklappte Bereich zeigt eine Transaktionstabelle mit Angaben wie Datum, Transaktionsart und Beträgen. Über das Kontextmenü können einzelne Transaktionen bearbeitet oder gelöscht werden. Das Bearbeiten oder Löschen einer Transaktion hat keinen Einfluss auf die Terminierung oder Konfiguration des Dauerauftrags — dieser wird unabhängig von manuellen Änderungen an bereits erstellten Transaktionen weiterhin gemäss seiner Wiederholungskonfiguration ausgeführt.

## Hintergrundverarbeitung
Die Ausführung der Daueraufträge erfolgt durch einen täglichen [Hintergrundprozess](../../admindata/taskdatachangemonitor/taskdescription/#JOB28). Dieser prüft, welche Daueraufträge fällig sind, und erstellt die entsprechenden Transaktionen. Der Standardzeitpunkt der Ausführung liegt nach der täglichen Kursaktualisierung, damit aktuelle Kursdaten für Wertpapierdaueraufträge zur Verfügung stehen. Der folgende vereinfachte Ablauf zeigt, wie der Hintergrundprozess einen einzelnen Dauerauftrag verarbeitet:
{{< mermaid >}}
graph TD
    A[Nächstes Ausführungsdatum prüfen] --> B{Ausführungsdatum fällig?}
    B -- Nein --> C[Keine Aktion]
    B -- Ja --> D{Typ des Dauerauftrags?}
    D -- Konto --> E[Wochenende überspringen]
    D -- Wertpapier --> F[Wochenende und Börsenfeiertage überspringen]
    E --> G[Kontotransaktion erstellen]
    F --> H[Kurs innerhalb der Kurstoleranz suchen]
    H --> I[Stückzahl und Kosten berechnen]
    G --> J[Nächstes Ausführungsdatum berechnen]
    I --> J
    H -- Kein Kurs gefunden --> K[Fehler im Protokoll speichern]
    K --> J
    J --> L{Nächstes Datum innerhalb Gültigkeit?}
    L -- Ja --> B
    L -- Nein --> M[Dauerauftrag deaktivieren]
{{< /mermaid >}}
