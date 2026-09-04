---
title: "Verbindung wiederherstellen"
date: 2026-08-28T22:54:47+01:00
draft: false
weight: 25
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
Die Implementierung von GTNet ist noch nicht vollständig abgeschlossen. Diese Dokumentation beschreibt die geplante und teilweise bereits umgesetzte Funktionalität.
{{% /notice %}}
Diese Seite beschreibt, wie eine bestehende Verbindung zu einem Gegenpart wiederhergestellt wird, nachdem die eigene Instanz ihre Zugangsdaten für diesen Gegenpart verloren hat. Das geschieht typischerweise nach einem Neuaufbau der Datenbank, nach dem Einspielen einer älteren Sicherung oder nach einem Umzug der Instanz auf einen anderen Server, wenn die GTNet-Daten dabei nicht mit exportiert und importiert wurden.

### Woran man das Problem erkennt
Der eigene Eintrag für den Gegenpart zeigt in der Spalte **Autorisiert** kein Häkchen mehr, und eine neue **«Erste Kontaktaufnahme»** wird vom Gegenpart abgelehnt. Als Begründung erscheint die Meldung «Mit dieser Domäne besteht bereits ein Handshake». Dasselbe Vorgehen hilft auch dann, wenn statt dessen die Meldung über ein ungültiges Authentifizierungs-Token erscheint, weil beide Fälle dieselbe Ursache haben: Die beiden Instanzen sind sich über die gemeinsamen Zugangsdaten nicht mehr einig.

Wird stattdessen gemeldet, dass die eigene Instanz nicht in der Serverliste des Gegenparts geführt wird, liegt ein anderer Fall vor. Dann kennt der Gegenpart die eigene Domain überhaupt nicht und nimmt unbekannte Server auch nicht automatisch auf. In diesem Fall muss der Administrator des Gegenparts die eigene Domain zuerst erfassen oder die Einstellung **Server-Erstellung erlauben** aktivieren.

### Warum die eigene Instanz das Problem nicht selbst lösen kann
Die **«Erste Kontaktaufnahme»** ist die einzige Nachricht, die ohne Zugangsdaten beim Empfänger eintrifft – der Absender hat zu diesem Zeitpunkt ja noch keine. Sie darf deshalb zwar eine neue Beziehung eröffnen, aber niemals eine bestehende überschreiben. Andernfalls könnte jeder, der die Domain einer fremden Instanz angibt, deren Zugangsdaten bei allen Gegenparts zurücksetzen. Aus demselben Grund hilft auch eine **«Token-Erneuerung»** nicht weiter: Diese Nachricht setzt gültige Zugangsdaten voraus, und genau die fehlen ja.

Die Verbindung lässt sich deshalb nur auf der Seite wiederherstellen, welche die Zugangsdaten noch besitzt. Es ist eine bewusste Entscheidung des dortigen Administrators, den Gegenpart wieder zuzulassen.

{{< mermaid >}}
flowchart TD
    A["Eigene Instanz hat die Zugangsdaten verloren"] --> B["Erste Kontaktaufnahme an den Gegenpart"]
    B --> C{"Hat der Gegenpart noch Zugangsdaten für uns?"}
    C -->|Ja| D["Abgelehnt: Mit dieser Domäne besteht bereits ein Handshake"]
    D --> E["Administrator des Gegenparts: Neuen Handshake erlauben"]
    E --> B
    C -->|Nein| F["Kontaktaufnahme erfolgreich, Verbindung wiederhergestellt"]
{{< /mermaid >}}

### Wie der Gegenpart davon erfährt
Die Instanz, welche die Zugangsdaten verloren hat, steckt in einer Zwickmühle: Ihre **«Erste Kontaktaufnahme»** wird abgewiesen, und eine Admin-Nachricht kann sie ebenfalls nicht senden, weil dafür genau jene Verbindung nötig wäre, die ihr verweigert wird. Sie hat also keinen Weg, den Administrator der Gegenseite zu benachrichtigen.

Deshalb hinterlässt die Abweisung eine Spur auf der Seite, welche die Zugangsdaten noch besitzt. In der Server-Übersicht erscheint in der Spalte **Neuverbindung angefragt** ein farbig hervorgehobenes Symbol auf der Zeile des betroffenen Gegenparts, sobald dieser vergeblich versucht hat, sich neu zu verbinden. Der Tooltip nennt den Zeitpunkt des letzten Versuchs. Ein Klick auf das Symbol führt dieselbe Rückfrage und dieselbe Aktion aus wie der Menüpunkt **«Neuen Handshake erlauben»**, sodass sich der Fall unmittelbar dort erledigen lässt, wo er sichtbar wird.

Der Eintrag wird laufend auf den jüngsten Versuch nachgeführt und verschwindet, sobald der neue Handshake erfolgt ist oder der Administrator ihn erlaubt hat. Ein Gegenpart, der es wiederholt versucht, erzeugt keine zusätzlichen Einträge.

{{% notice info %}}
Wer regelmässig mit anderen Instanzen Daten austauscht, sollte die Server-Übersicht gelegentlich auf dieses Symbol prüfen. Es ist der einzige Hinweis darauf, dass ein Gegenpart wieder Anschluss sucht.
{{% /notice %}}

### Neuen Handshake erlauben
Auf der Instanz, welche die Zugangsdaten noch besitzt, steht in der Server-Übersicht im Kontextmenü der Menüpunkt **«Neuen Handshake erlauben»** zur Verfügung. Er verwirft die mit dem ausgewählten Gegenpart geteilten Zugangsdaten, sodass dessen nächste **«Erste Kontaktaufnahme»** wieder angenommen wird. Vor dem Verwerfen wird die betroffene Domain in einer Rückfrage genannt. Dasselbe bewirkt ein Klick auf das Symbol in der Spalte **Neuverbindung angefragt**.

Das Vorgehen umfasst folgende Schritte:
1. Auf der Instanz, welche die Zugangsdaten noch besitzt, wird in der Server-Übersicht die Zeile des Gegenparts ausgewählt, der sich nicht mehr verbinden kann.
2. Im Kontextmenü wird **«Neuen Handshake erlauben»** aufgerufen und die Rückfrage bestätigt.
3. Die Zeile zeigt danach in der Spalte **Autorisiert** kein Häkchen mehr, und der Online-Status wechselt auf **Unbekannt**.
4. Auf der Instanz, welche die Zugangsdaten verloren hat, wird nun die Zeile des Gegenparts ausgewählt und über das Kontextmenü eine **«Erste Kontaktaufnahme»** gesendet.
5. Nach der Antwort **«Kontaktaufnahme erfolgreich»** zeigen beide Instanzen in der Spalte **Autorisiert** wieder ein Häkchen.
6. Mit **«Status jetzt prüfen»** lässt sich abschliessend bestätigen, dass die Verbindung tatsächlich wieder trägt.

Der letzte Schritt lohnt sich, weil ein erfolgreicher Handshake allein noch nichts darüber aussagt, ob auch die anschliessende Kommunikation funktioniert. Erst eine Statusprüfung sendet eine Anfrage mit den neuen Zugangsdaten und beweist damit, dass beide Richtungen wieder nutzbar sind.

### Was erhalten bleibt und was sich ändert
Verworfen werden ausschliesslich die Zugangsdaten. Der Eintrag des Gegenparts bleibt mit seiner gesamten Nachrichtenhistorie bestehen, ebenso die Einstellungen zu den Informationsobjekten und die dem Gegenpart erteilten Berechtigungen wie **Serverliste weitergeben** oder das **Tägliche Abfragelimit**. Genau das unterscheidet diesen Weg vom Löschen des Eintrags, bei dem alles Genannte verloren geht.

Bis die neue Kontaktaufnahme abgeschlossen ist, steht der Gegenpart auf **Unbekannt** und der Austausch seiner Informationsobjekte ist geschlossen. Solange findet mit dieser Instanz kein Datenaustausch statt. Weshalb ein Eintrag ohne Zugangsdaten den Status **Unbekannt** trägt, ist unter [GTNet und Nachrichten](../) beschrieben.

{{% notice note %}}
Der Menüpunkt ist nur für Administratoren sichtbar. Für den eigenen Server-Eintrag ist er deaktiviert, ebenso für Einträge, mit denen noch nie eine Verbindung bestanden hat.
{{% /notice %}}

### Wenn der Gegenpart einem anderen Betreiber gehört
Gehört die andere Instanz nicht zum eigenen Verantwortungsbereich, lässt sich das Problem nicht von der eigenen Seite aus beheben. Die erfolglose Kontaktaufnahme wird dort aber sichtbar: Auf der Zeile der eigenen Domain erscheint das Symbol in der Spalte **Neuverbindung angefragt**. Ein aufmerksamer Administrator erkennt also von selbst, dass jemand wieder Anschluss sucht. Besteht zusätzlich ein anderer Kontaktweg, kann er darum gebeten werden, **«Neuen Handshake erlauben»** aufzurufen. Sobald das geschehen ist, genügt eine neue **«Erste Kontaktaufnahme»** von der eigenen Instanz.

### Wie sich das Problem vermeiden lässt
Wird eine Instanz auf einen anderen Server umgezogen oder ihre Datenbank neu aufgebaut, sollten die GTNet-Daten vorher exportiert und nachher wieder importiert werden. Dabei bleiben die Zugangsdaten aller Gegenparts erhalten und keine Verbindung geht verloren. Das Vorgehen ist unter [GTNet und Nachrichten](../) im Abschnitt zum Export und Import der GTNet-Daten beschrieben.
