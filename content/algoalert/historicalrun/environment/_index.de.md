---
title: "In der Simulationsumgebung arbeiten"
date: 2026-10-01T10:00:00+01:00
draft: false
weight: 5
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="In Bearbeitung - Zielversion V0.38.0" %}}
Die Implementierung von Alarmen und regelbasiertem Handel ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Eine Simulationsumgebung ist eine abgetrennte Kopie Ihrer Portfolios und Konten zu einem Stichtag in der Vergangenheit. Sie ist ein eigener Mandant: Was Sie darin buchen, ändern oder löschen, bleibt darin und berührt Ihr echtes Portfolio nicht. Wie eine Umgebung entsteht, ist unter [Regelbasierter Handel](../../algo/) beschrieben.

Mit **Zur Simulation wechseln** öffnen Sie eine Umgebung, mit **Zum Hauptmandanten wechseln** kehren Sie zurück. Beide Befehle laden die Anwendung neu, damit Navigationsbaum, Menüs und Auswertungen wirklich zur gewählten Umgebung passen und nicht noch Zahlen des zuvor geöffneten Mandanten zeigen.

## Was in einer Umgebung möglich ist
Die folgende Übersicht zeigt, womit Sie in einer geöffneten Umgebung arbeiten können, solange keine Wiederholung läuft, und was ausserhalb bleibt. Massgebend ist immer, wo die Daten hingehören: alles Mandantenspezifische gehört der Umgebung, die Strategie und Ihr Benutzerkonto gehören Ihnen.

| Bereich | In der Umgebung |
|---|---|
| Portfolios, Konten, Watchlists, Korrelationsmatrizen, Importe, Steuerkorrekturen | Bearbeitbar; sie gehören allein der Umgebung. Konten und Portfolios mit Buchungen des Eröffnungsbestandes lassen sich jedoch nicht löschen. |
| Name, Währung und die übrigen Angaben der Umgebung selbst | Bearbeitbar wie bei Ihrem eigenen Portfolio. |
| Transaktionen | Eigene Transaktionen können Sie erfassen, ändern und löschen; sie sind jedoch vergänglich. Die Transaktionen des Eröffnungsbestandes sind geschützt. Beachten Sie dazu die folgenden Abschnitte. |
| Strategie mit ihren Anlageklassen, Wertpapieren und Regeln | Nur lesbar. Ändern Sie die Strategie in Ihrem eigenen Portfolio und wiederholen Sie die Simulation danach erneut. |
| Alarme, einschliesslich ihrer Auswertung und Benachrichtigungen, sowie das Ein- und Ausschalten der Überwachung | Nicht verfügbar. Alarme beobachten laufende Kurse und gehören deshalb ausschliesslich zu Ihrem eigenen Portfolio. |
| Instrumente, Währungspaare, Anlageklassen, Börsen, Importvorlagen, historische Kurse | Im Rahmen Ihrer üblichen Berechtigungen bearbeitbar. Diese Daten sind gemeinsam genutzt und gehören keiner einzelnen Umgebung. |
| Private Instrumente | Erfassbar; sie gehören der Umgebung und werden mit ihr gelöscht. |
| Konto- und Wertpapier-Daueraufträge | Erfassbar; ausgeführt werden sie allein durch die [Historische Wiederholung](../standingorders/). |
| **Persönliche Daten exportieren** und **Löschen meiner Daten und des Benutzerkontos** | Nicht verfügbar. Beide betreffen Ihr Benutzerkonto und wirken nur in Ihrem eigenen Portfolio; die Menüeinträge werden in einer Umgebung deshalb nicht angezeigt. |
| **Lesezugriff teilen** und die Klientenverwaltung | Nicht verfügbar. Eine Freigabe gilt Ihrem eigenen Portfolio, nicht einer Kopie davon. |

Versuchen Sie eine dieser gesperrten Funktionen dennoch, so lehnt Grafioschtrader sie mit einer Meldung ab, statt still etwas am falschen Ort zu ändern.

{{% notice style="info" title="Die Strategie ist gemeinsam, aber geschützt" %}}
Die Strategie wird von Ihrem Portfolio und allen ihren Simulationsumgebungen gemeinsam verwendet. Damit eine aufgezeichnete Wiederholung nachvollziehbar bleibt, lässt sie sich aus einer Umgebung heraus nicht verändern. Wechseln Sie zum Hauptmandanten, passen Sie die Strategie dort an und starten Sie die Wiederholung anschliessend neu.
{{% /notice %}}

{{% notice style="warning" title="Gemeinsam genutzte Daten wirken überall" %}}
Ändern Sie in einer Umgebung ein Instrument, ein Währungspaar oder dessen Kurse, so ändern Sie diese Daten für Ihr eigenes Portfolio und für alle anderen Benutzer, die sie verwenden. Grafioschtrader behandelt eine solche Änderung genau gleich wie im Hauptmandanten und weist in einer Umgebung nicht zusätzlich darauf hin.
{{% /notice %}}

## Der geschützte Eröffnungsbestand
Beim Erstellen einer Umgebung legt Grafioschtrader deren Eröffnungsbestand an: die zum Eröffnungsdatum kopierten Transaktionen oder die Einzahlungen, mit denen eine Umgebung ohne Positionen startet. Auf diesen Bestand setzt jede Wiederholung zurück. Er ist deshalb geschützt und lässt sich weder bearbeiten noch löschen.

In der Transaktionsliste bietet Grafioschtrader für eine Buchung des Eröffnungsbestandes keinen Befehl an, der sie verändern würde: **Bearbeiten**, **Löschen**, **Umwandlung in Kontoübertrag** und **Steuerstatus umschalten** sind deaktiviert. **Erstellen Dauerauftrag** bleibt verfügbar, weil ein neuer Dauerauftrag die Buchung selbst nicht verändert. Ebenso wenig lässt sich ein Konto oder Portfolio löschen, auf dem Buchungen des Eröffnungsbestandes liegen; Grafioschtrader lehnt dies mit der Meldung «Dieses Konto oder Portfolio enthält Eröffnungstransaktionen und kann nicht gelöscht werden.» ab.

Eröffnungsdatum, Eröffnungsart und Eröffnungsbestand lassen sich nicht nachträglich ändern. Möchten Sie von einem anderen Ausgangspunkt aus rechnen, erstellen Sie eine neue Umgebung.

## Wirkung der Historischen Wiederholung
Eine Wiederholung beginnt immer beim Eröffnungsbestand der Umgebung. Bevor der erste Handelstag ausgewertet wird, versetzt Grafioschtrader die Umgebung in genau diesen Zustand zurück. Dabei wird gelöscht, was nicht zum Eröffnungsbestand gehört:

- Sämtliche Transaktionen, die nicht aus der Eröffnung stammen. Das betrifft die Ausführungen, Erträge und Dauerauftragsbuchungen der vorherigen Wiederholung ebenso wie Transaktionen, die Sie selbst in der Umgebung erfasst haben.
- Ein in der Umgebung erfasster **Wertpapiertransfer** und die in der Umgebung angewendeten **Wertpapieraktionen**. Die Wertpapieraktion selbst ist gemeinsam genutzt und bleibt erhalten; Sie können sie danach erneut anwenden.
- Das bisherige Ergebnis mit seinem Verlauf.

Anschliessend werden die Bestände aus dem Eröffnungsbestand neu aufgebaut. Nicht zurückgesetzt werden dagegen Ihre Einstellungen: Portfolios, Konten, Watchlists und die Angaben der Umgebung selbst bleiben unverändert. Eine Wiederholung stellt also nicht die ganze Umgebung wieder her, sondern nur ihren Transaktionsbestand.

{{< mermaid >}}
graph TD
    A["Wiederholung starten..."] --> B{"Bestätigung"}
    B -->|Nein| C["Nichts geschieht"]
    B -->|Ja| D["Dialog Historische Wiederholung"]
    D --> E["Transaktionen ausserhalb des Eröffnungsbestandes und bisheriges Ergebnis löschen"]
    E --> F["Bestände aus dem Eröffnungsbestand neu aufbauen"]
    F --> G["Handelstage auswerten"]
    G --> H["Abgeschlossen: Ergebnis mit Kennzahlen"]
    G --> I["Abgebrochen oder Fehlgeschlagen: Teilergebnis ohne Kennzahlen"]
{{< /mermaid >}}

Weil dabei auch Ihre eigenen Buchungen verloren gehen, fragt Grafioschtrader vor jeder Wiederholung nach: «Wiederholung starten? Alle Buchungen ausserhalb des geschützten Eröffnungsbestandes, einschliesslich Ihrer manuell erfassten Buchungen, werden gelöscht und bisherige Ergebnisse ersetzt.» Verneinen Sie die Frage, wird nichts gestartet und nichts gelöscht.

{{% notice style="warning" title="Eigene Buchungen gehen bei der nächsten Wiederholung verloren" %}}
Transaktionen, die Sie von Hand in einer Umgebung erfassen, sind bewusst vergänglich: Die nächste Wiederholung entfernt sie zusammen mit den Ausführungen des letzten Laufs. Wollen Sie eine Buchung dauerhaft behalten, gehört sie in Ihr eigenes Portfolio oder in eine eigene Umgebung, die Sie nicht erneut wiederholen.
{{% /notice %}}

Ein abgebrochener oder fehlgeschlagener Lauf behält, was er bis dahin gebucht und aufgezeichnet hat. Dieses Teilergebnis können Sie ansehen, Kennzahlen werden dafür jedoch keine ausgewiesen, weil der Lauf sein Enddatum nicht erreicht hat. Die nächste Wiederholung beginnt wieder beim Eröffnungsbestand.

## Während eine Wiederholung läuft
Eine laufende Wiederholung bucht fortlaufend Transaktionen und baut die Bestände der Umgebung neu auf. Solange sie arbeitet, ist die Umgebung deshalb gesperrt: Sie lässt sich weder öffnen noch löschen, und auch gemeinsam genutzte Daten lassen sich nicht aus ihr heraus bearbeiten. **Zur Simulation wechseln** und **Simulation löschen** werden dann nicht angeboten. Eine Sitzung, die die Umgebung bereits geöffnet hat, etwa in einem zweiten Browserfenster, erhält beim nächsten Schritt die Meldung «Diese Simulationsumgebung wird gerade wiederholt. Warten Sie das Ende der Wiederholung ab, bevor Sie sie öffnen.» und kehrt zum Hauptmandanten zurück.

Status und Verlauf verfolgen Sie stattdessen vom Hauptmandanten aus, ebenso **Wiederholung abbrechen**. Ein Abbruch beendet den Lauf nach dem Tag, an dem er gerade rechnet; bis dahin gebuchte Ausführungen bleiben erhalten. Die Umgebung wird erst wieder frei, wenn der Lauf tatsächlich beendet ist, nicht schon im Moment des Abbruchs. Warten Sie deshalb, bis der Status von **In Bearbeitung** auf **Abgebrochen** wechselt, bevor Sie die Umgebung öffnen oder löschen.

{{< mermaid >}}
stateDiagram-v2
    [*] --> Frei
    Frei --> Laeuft: Wiederholung starten
    Laeuft --> Abbrechend: Wiederholung abbrechen
    Abbrechend --> Frei: Lauf beendet
    Laeuft --> Frei: Lauf beendet
    Frei --> [*]: Simulation loeschen
    note right of Frei
        Oeffnen, bearbeiten und
        loeschen moeglich
    end note
    note right of Laeuft
        Gesperrt: weder oeffnen
        noch loeschen
    end note
{{< /mermaid >}}

Je Umgebung läuft immer nur eine Wiederholung, und der Server führt insgesamt nur wenige gleichzeitig aus. Ist diese Kapazität ausgeschöpft, meldet Grafioschtrader dies und Sie versuchen es später erneut.

## Mengenbeschränkungen
Jede Umgebung hat ihr eigenes Budget: Portfolios, Konten, Watchlists und Transaktionen zählen gegen die üblichen Obergrenzen, aber getrennt von Ihrem eigenen Portfolio und getrennt von den anderen Umgebungen. Eine Umgebung kann Ihnen also nicht den Platz in Ihrem Hauptmandanten wegnehmen.

Schon beim Erstellen prüft Grafioschtrader, ob die Kopie in dieses Budget passt. Würde sie mehr Portfolios, Konten, Watchlists, Watchlist-Positionen oder Eröffnungsbuchungen enthalten als erlaubt, wird die Umgebung nicht erstellt, und die Meldung nennt die überschrittene Grenze, zum Beispiel «Die Simulation würde 3 Portfolios enthalten; die Obergrenze ist 2.». Eine Kopie wird nie stillschweigend gekürzt.

Auch eine Wiederholung zählt ihre Buchungen gegen die Obergrenze der Transaktionen. Erreicht sie diese, bucht sie nichts mehr und endet mit dem Status **Fehlgeschlagen**. Das bis dahin Gebuchte und Aufgezeichnete bleibt als Teilergebnis erhalten. Wählen Sie in diesem Fall ein früheres Enddatum oder eine Strategie, die weniger handelt, oder bitten Sie den Administrator um eine höhere Grenze.

Anders verhält es sich bei gemeinsam genutzten Daten. Ein Instrument, das Sie in einer Umgebung anlegen, zählt gegen dasselbe Budget wie eines, das Sie in Ihrem eigenen Portfolio anlegen. Die Anzahl der Simulationsumgebungen je Portfolio ist ebenfalls begrenzt; standardmässig sind es fünf. Wie diese Grenzen verwaltet werden, ist unter [Mengenbeschränkung]({{% ref "/admindata/entitylimit" %}}) beschrieben.

Auch der Verlauf einer Wiederholung hat eine Obergrenze. Ist sie erreicht, zeichnet Grafioschtrader keine weiteren Einträge mehr auf, der Lauf selbst rechnet aber zu Ende. Der Verlauf ist dann unvollständig, das Ergebnis bleibt gültig.

## Löschen einer Umgebung
**Simulation löschen** entfernt die Umgebung mit allem, was allein zu ihr gehört: Portfolios, Konten, Transaktionen, Watchlists sowie das Ergebnis der letzten Wiederholung mit ihrem Verlauf. Ihr eigenes Portfolio, die gemeinsam genutzten Instrumente und die Strategie bleiben unberührt. Solange eine Wiederholung der Umgebung läuft, lehnt Grafioschtrader das Löschen ab; brechen Sie den Lauf ab oder warten Sie sein Ende ab.

Mitgelöscht werden auch private Instrumente, die Sie innerhalb dieser Umgebung angelegt haben, samt ihren Kursen. Ein privates Instrument gehört dem Mandanten, in dem es entstanden ist, und wäre ohne diesen ohne Bedeutung. Legen Sie ein Instrument an, das Sie über die Simulation hinaus behalten möchten, so erfassen Sie es in Ihrem eigenen Portfolio.

**Löschen meiner Daten und des Benutzerkontos** entfernt zuerst sämtliche Simulationsumgebungen Ihres Portfolios und danach Ihre eigenen Daten und das Benutzerkonto. Beides geschieht in einem Schritt: Scheitert etwas, bleibt alles erhalten. Läuft in einer Ihrer Umgebungen gerade eine Wiederholung, wird das Löschen abgelehnt, bis der Lauf beendet ist. Dasselbe gilt, wenn die Daten eines verwalteten Klienten gelöscht werden: Auch dessen Umgebungen werden zuerst entfernt.

{{% notice style="info" title="Umgebungen sind nicht Teil des persönlichen Exports" %}}
**Persönliche Daten exportieren** liefert die Daten Ihres eigenen Portfolios. Die Inhalte einer Simulationsumgebung, also ihre Einstellungen, private Instrumente, eigene Buchungen und die Ergebnisse einer Wiederholung, sind darin nicht enthalten und lassen sich aus dem Export auch nicht wiederherstellen. Das ist beabsichtigt: Eine Umgebung ist eine jederzeit neu erstellbare Kopie, kein eigenständiger Datenbestand. Gemeinsam genutzte Daten wie Instrumente folgen den gewohnten Regeln des Exports, unabhängig davon, ob Sie sie in einer Umgebung oder im Hauptmandanten angelegt haben.
{{% /notice %}}
