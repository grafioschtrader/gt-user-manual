---
title: "Wartung und Betriebseinstellung"
date: 2026-08-31T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Eine GTNet-Instanz, die ohne Vorwarnung verschwindet, verursacht bei allen Gegenparts ins Leere laufende Anfragen und wird dort früher oder später als «Offline» geführt. Wer eine Wartung plant oder den Betrieb ganz einstellt, kündigt dies deshalb im Voraus an. Die Gegenparts richten sich automatisch danach und fragen die Instanz im angekündigten Zeitraum gar nicht erst an.

### Die beiden Ankündigungen im Vergleich
Für die geplante Nichtverfügbarkeit gibt es zwei Ankündigungen, die sich in ihrer Dauer und in ihrer Endgültigkeit unterscheiden.

| | Wartungsfenster | Betriebseinstellung |
|---|---|---|
| Nachricht | «Server befindet sich während des Zeitraums im Wartungsmodus» | «Der Serverbetrieb wird ab diesem Datum eingestellt» |
| Einzugebende Felder | «Beginn» und «Ende» | «Betrieb eingestellt ab» |
| Mehrere gleichzeitig offen | Ja, sofern sie sich nicht überschneiden | Nein, nur eine |
| Wirkung beim Empfänger | Die Instanz wird im Zeitraum nicht kontaktiert | «Server Online» wird auf «Ausser Betrieb» gesetzt |
| Endet von selbst | Ja, mit «Ende» | Nein, der Zustand ist endgültig |
| Widerruf möglich | Vor «Beginn» | Vor «Betrieb eingestellt ab» |

### Ein Wartungsfenster ankündigen
Ein Wartungsfenster wird in der Ansicht [GTNet und Nachrichten](../) angekündigt. Dazu wählt man den eigenen Server-Eintrag aus – oder gar keine Zeile – und ruft im Kontextmenü **«GTNet Nachrichten versenden»** auf. Als Nachrichtentyp steht dort «Server befindet sich während des Zeitraums im Wartungsmodus» zur Verfügung. Einzutragen sind die beiden Felder **«Beginn»** und **«Ende»**; beide müssen in der Zukunft liegen und «Ende» muss nach «Beginn» liegen.

Es dürfen mehrere Wartungsfenster gleichzeitig angekündigt sein, solange sie sich zeitlich nicht überschneiden. Ein Fenster, das in ein bereits angekündigtes hineinragt, wird mit einer Fehlermeldung abgewiesen. Damit lässt sich beispielsweise eine Reihe wiederkehrender Wartungsabende im Voraus bekanntgeben.

Auf der eigenen Instanz ändert die Ankündigung nichts. Sie ist ausschliesslich eine Information an die Gegenparts; der eigene Server läuft bis zum tatsächlichen Beginn der Wartung unverändert weiter und muss zum angekündigten Zeitpunkt selbst heruntergefahren werden.

### Was während eines Wartungsfensters geschieht
Beim Empfänger wird das angekündigte Fenster gespeichert und ausgewertet, sobald es beginnt. Zwischen «Beginn» und «Ende» wird die betreffende Instanz weder nach Intraday-Kursen noch nach historischen Kursen oder Wertpapier-Metadaten gefragt, und es werden auch keine Nachrichten an sie gesendet. Die Prüfung des Online-Status lässt die Instanz in diesem Zeitraum ebenfalls unangetastet, damit eine geplante Abschaltung nicht fälschlicherweise als «Offline» festgehalten wird.

Nach «Ende» wird die Instanz wieder ganz normal verwendet. Es ist kein Eingriff nötig und es muss auch keine Entwarnung gesendet werden; das Fenster läuft von selbst aus.

Welche Fenster ein Gegenpart angekündigt hat, zeigt die Server-Übersicht: Die aufgeklappte Zeile enthält den Bereich **«Wartungsfenster»** mit «Beginn» und «Ende» jedes gemeldeten Fensters. Bereits abgelaufene Fenster bleiben dort sichtbar, bis die zugehörige Nachricht gelöscht wird.

### Die Betriebseinstellung ankündigen
Wird eine Instanz dauerhaft abgeschaltet, kündigt man dies mit der Nachricht «Der Serverbetrieb wird ab diesem Datum eingestellt» an. Einzutragen ist das Feld **«Betrieb eingestellt ab»**, das in der Zukunft liegen muss. Es kann immer nur eine Betriebseinstellung offen sein; solange eine angekündigt ist, wird der Nachrichtentyp nicht mehr zur Auswahl angeboten.

Bis zum angekündigten Datum bleibt alles unverändert: Die Gegenparts verwenden die Instanz weiterhin als Datenlieferant und senden ihr Nachrichten. Die Vorankündigung kostet also keinen Datenaustausch, sondern gibt den Gegenparts Zeit, sich nach anderen Quellen umzusehen.

### Was ab dem angekündigten Datum geschieht
Ab dem Datum «Betrieb eingestellt ab» setzt der Empfänger die Spalte **«Server Online»** der betreffenden Instanz auf **«Ausser Betrieb»**. Gleichzeitig erhalten alle Informationsobjekte dieser Instanz den «Status Server» **«Geschlossen»** und «Anfrage akzeptieren» **«Geschlossen»**, womit die Instanz aus der Lieferantenauswahl fällt.

Dieser Zustand ist endgültig. Weder eine Prüfung des Online-Status noch eine eingehende Nachricht der betreffenden Instanz hebt ihn wieder auf – auch dann nicht, wenn der Server wider Erwarten weiterhin antwortet. Damit wird verhindert, dass eine abgekündigte Instanz durch einen zufällig erfolgreichen Ping stillschweigend wieder in Betrieb genommen wird.

Die Umstellung erledigt eine Hintergrundaufgabe, die alle fünf Stunden läuft. Sie erfolgt deshalb nicht auf die Minute genau, sondern innerhalb weniger Stunden nach dem angekündigten Datum.

{{< mermaid >}}
stateDiagram-v2
    [*] --> Unbekannt
    Unbekannt --> Online: Ping erfolgreich
    Online --> Offline: Ping fehlgeschlagen
    Offline --> Online: Ping erfolgreich
    Online --> AusserBetrieb: Angekündigtes Datum erreicht
    Offline --> AusserBetrieb: Angekündigtes Datum erreicht
    AusserBetrieb --> Unbekannt: Betriebseinstellung abgesagt

    state AusserBetrieb: Ausser Betrieb

    note right of AusserBetrieb : Wird weder durch eine Statusprüfung noch durch eine Nachricht aufgehoben
{{< /mermaid >}}

Nach der Betriebseinstellung kann der Eintrag über das Kontextmenü der Server-Übersicht gelöscht werden. Mit ihm verschwinden auch dessen Nachrichten, Austauscheinstellungen und Austauschprotokolle. Wer die Instanz stattdessen behalten möchte, etwa weil sie doch wieder in Betrieb geht, setzt «Server Online» im Bearbeitungsdialog von Hand auf einen anderen Wert.

### Eine Ankündigung widerrufen
Beide Ankündigungen lassen sich zurücknehmen. Dazu wählt man in der Nachrichtenliste des eigenen Eintrags die gesendete Ankündigung aus und ruft im Kontextmenü **«Widerrufen»** auf. Damit wird «Wartung abgesagt» beziehungsweise «Betriebseinstellung abgesagt» an alle Gegenparts gesendet; diese verwerfen daraufhin das Wartungsfenster respektive die vorgemerkte Betriebseinstellung.

Der Menüpunkt erscheint nur, solange der angekündigte Zeitpunkt noch in der Zukunft liegt: bei einem Wartungsfenster vor dessen «Beginn», bei einer Betriebseinstellung vor dem Datum «Betrieb eingestellt ab». Ein bereits laufendes Wartungsfenster wird nicht vorzeitig beendet, sondern läuft mit «Ende» aus.

### Zustellung der Ankündigungen
Ankündigungen gehen an alle Gegenparts, mit denen ein Datenaustausch eingerichtet und der Handshake abgeschlossen ist. Sie werden unmittelbar nach dem Absenden zugestellt; ist ein Gegenpart in diesem Moment nicht erreichbar, wiederholt eine Hintergrundaufgabe die Zustellung später. Ein Gegenpart, dessen Handshake erst nach der Ankündigung zustande kommt, erhält sie ebenfalls noch, sofern sie zu diesem Zeitpunkt noch gültig ist.

Für jeden Empfänger wird ein eigener Zustellversuch geführt. Ein vorübergehender Verbindungsfehler betrifft deshalb nur diesen Gegenpart und verhindert nicht, dass andere die Ankündigung erhalten. Gehen die Zugangsdaten nach dem Einreihen verloren, wartet der Versuch auf einen erneuten Handshake. Wird der Gegenpart dagegen endgültig als «Ausser Betrieb» geführt oder endet die Gültigkeit der Ankündigung, wird nicht mehr zugestellt. Solche Ergebnisse bleiben zusammen mit der Nachricht sichtbar, statt wie ein noch nicht bearbeiteter Versuch auszusehen.

Administratoren können diese Ergebnisse im aufgeklappten eigenen Server-Eintrag unter **«Zustellversuche»** prüfen. Die einzelnen Status und ihre Bedeutung sind unter [GTNet überwachen](../../monitoring/#zustellversuche-von-nachrichten) beschrieben.
{{% notice info %}}
Kündigen Sie ein Wartungsfenster frühzeitig an. Eine Instanz, die zum Zeitpunkt der Ankündigung selbst nicht erreichbar ist, erfährt erst mit einem der nächsten Zustellversuche davon – und richtet sich bis dahin nicht nach dem Fenster.
{{% /notice %}}
