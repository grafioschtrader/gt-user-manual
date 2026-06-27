---
title: "Administrative Daten - Nachrichtensystem"
date: 2026-06-26T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
## Administrative Daten
Aufgrund der **Benutzerrechte** unterscheidet sich das statische Hauptelement **Administrative Daten** im **Navigationsbereich**. Das Unterelement "**Benutzer Einstellung**" ist nur für einen Administrator sichtbar.

## Alarme und Nachrichtensystem
GT implementiert ein Alarm- und Nachrichtensystem. Das Alarmsystem ist für Funktionsstörungen von GT gedacht und richtet sich an den Hauptadministrator. Während das Nachrichtensystem für alle Benutzer gedacht ist.

## Alarm
Der [Hauptadministrator](../intro/userrights/#hauptadmin) bekommt **Alarme** von GT direkt an seine E-Mail Adresse gemeldet. Beispielsweise wenn die Verarbeitung der Hintergrundaufgaben nicht mehr arbeitet. Die Behebung einer solchen Störung sollte möglichst Zeitnahe gelöst werden. 

## Nachrichtensystem 
Mit der Datenänderungsanfrage kann bereits ein gezielter Vorschlag zur Verbesserung der Datenqualität für eine einzelne Entität gemacht werden. Andererseits kann es auch Probleme geben, die nicht mit einer einzelnen Entität zusammenhängen. Daher muss es möglich sein, dass der **Benutzer** mit einer der **privilegierten Benutzerrollen** kommunizieren kann. Darüber hinaus kann das **System** auch bestimmte **Überprüfungen der Daten** durchführen und den Benutzer auf eventuell fehlende Einträge hinweisen. Schliesslich muss der **Administrator** in der Lage sein, eine **Nachricht an alle** zu senden. Beispielsweise um eine Wartungsunterbrechung anzukündigen. Dies sind die Hauptgründe, warum GT ein Nachrichtensystem implementiert.

#### Nachricht Benutzer <--> Benutzer
Nur ein Benutzer der Benutzerrolle Administrator kann eine **Anfangsnachricht** direkt einen anderen Benutzer senden. Indirekt funktioniert dies auch für alle anderen Benutzer, indem dieser eine Nachricht aus einem bestimmten **Kontext** heraus erstellen. Beispielsweise kann an den Ersteller eines bestimmten Wertpapiers eine Nachricht geschickt werden. Wie in diesem Fall dargelegt, wird der Empfänger nicht direkt ausgewählt, sondern aus dem Kontext ermittelt.

Um einem Missbrauch des Nachrichtensystems vorzubeugen — beispielsweise dem direkten Versand von Nachrichten über die Programmierschnittstelle unter Umgehung der Benutzeroberfläche — prüft das System bei einer solchen Kontextnachricht, ob der Empfänger tatsächlich der **Ersteller** des referenzierten Instruments ist und ob sich dieses Instrument in einer der **Watchlisten des Absenders** befindet. Nachrichten, welche diese Bedingungen nicht erfüllen, werden abgewiesen. Innerhalb eines bestehenden Nachrichtenthemas können zudem nur die daran beteiligten Benutzer antworten. Benutzer der Benutzerrolle **Administrator** unterliegen dieser Kontexteinschränkung nicht.

Als zusätzlichen Schutz vor Missbrauch ist die Anzahl der Nachrichten, die ein Benutzer der Rolle **Limits** pro Tag versenden darf, begrenzt. Der Administrator steuert dies in den [globalen Einstellungen]({{% relref "/admindata/globalsettings" %}}) über den Parameter `g.limit.day.MailSendRecv` (Standardwert 200). Benutzer mit privilegierten Rollen (Administrator oder ohne Limits) sind von dieser Begrenzung ausgenommen.

#### Nachricht Benutzer <--> privilegierte Benutzerrolle
Eine indirekte Kommunikation zwischen zwei Benutzern findet bereits durch das Anbringen einer Datenänderungsanfrage statt. Andererseits kann es sein, dass der Benutzer unabhängig von einer Änderung einer Entität eine Anfrage an eine bestimmte Benutzerrolle richten möchte. 

#### Systemnachricht --> Benutzer
GT führt in regelmässigen Abständen einige **Überwachungsfunktionen** für private und gemeinsam genutzte Daten durch. In den meisten Fällen wird die Vollständigkeit der Daten überprüft und im Falle einer Unvollständigkeit kann eine Benutzerinteraktion diesen eventuellen Mangel beheben. Daher erstellt das System eine Nachricht und übermittelt diese an den entsprechenden Benutzer oder an die zuständige Benutzerrolle.

### Nachricht/en
Die Nachrichten werden in einer **Hierarchietabelle** dargestellt, wobei das Hauptelement die **Anfangsnachricht** eines **Nachrichtenthemas** darstellen und die **Unterelemente** den folgenden Nachrichtenaustausch enthalten. 
- Für die einfachere Identifizierung wird ein **Nachrichtenthema** mit **Unterelementen** auf der **Anfangsnachricht** farblich hervorgehoben.

#### Eigenschaften und Tabellenspalten
Die meisten Eigenschaften sind selbsterklärend und werden daher nicht weiter erläutert, zudem ist bei gewissen Eigenschaften eine Quickinfo vorhanden.
- **Anzahl der Antworten**: Eine **Anfangsnachricht** wird üblicherweise an eine Benutzerrolle geschickt. Diese kann privat beantwortet werden und ist somit für die anderen Benutzer dieser Benutzerrolle unsichtbar. Aus diesem Grund wird die Anzahl der Antworten angezeigt, damit eventuelle **unnötige Beantwortungen** vermieden werden.

#### Nachrichtenthema eines Benutzers
Oben wurde beschrieben, wie ein Nachrichtenthema von einem Benutzer erstellt werden kann. Die Nachrichten in einem solchen Nachrichtenthema sind privat und es handelt sich um eine Kommunikation zwischen zwei Benutzern.

#### Nachrichtenthema einer Benutzerrolle
In der Ansicht **Nachricht** können **Anfangsnachrichten** nur an eine **Benutzerrolle** gesendet werden. Alle Nachrichten eines solchen **Nachrichtenthemas** sind für alle Benutzer der angesprochenen Benutzerrolle sichtbar. Allerdings kann in einem solchen Nachrichtenthema auch privat geantwortet werden, also unsichtbar für die anderen Benutzer der Benutzerrolle. Dies ist der Hauptgrund für die Existenz der Tabellenspalte “**Anzahl der Antworten**”. Ein solches Rollen-Nachrichtenthema wird erst dann endgültig entfernt, wenn jedes Mitglied der Benutzerrolle es gelöscht hat; diese Bereinigung übernimmt eine tägliche [Hintergrundaufgabe]({{< relref "/admindata/taskdatachangemonitor/taskdescription/#JOB29" >}}).

#### Nachrichtenthema durch Systemnachricht an Benutzers
Auf Nachrichten des Systems sollte reagiert und die mögliche Aktion durchgeführt werden. Es besteht die Möglichkeit, die Systemnachricht an eine bestimmte Benutzerrolle zu **beantworten**. Dies kann verwendet werden, wenn der Empfänger nicht in der Lage ist, das in der Systemnachricht beschriebene Problem selbst zu lösen.

### Einstellung Nachrichten
Hier wird die Zuordnung zwischen **Nachrichtentyp** und **Kommunikationskanal** vorgenommen. Zurzeit unterstützt GT die zwei Kommunikationskanäle **GT Nachricht** und **E-Mail**. Werden keine individuellen Einstellungen durch den Benutzer vorgenommen, gelten die Standardeinstellungen. Nur die Benutzerrolle »Administrator« hat die Möglichkeit, die Nachricht an einen anderen Benutzer weiterzuleiten.
- Eine Nachricht, die an mehrere Benutzer versendet wird, wird standardmässig als **GT Nachricht** zugestellt; zusätzlich kann der Benutzer einstellen, ob er auch eine **E-Mail** erhalten will. Bei der Ankündigung des Administrators an alle kann der Benutzer stattdessen **nur E-Mail** wählen — in diesem Fall wird die Ankündigung nicht als GT Nachricht angezeigt (siehe Warnung unten).

{{% notice warning %}}
Setzt ein Benutzer den Kanal für **Ankündigung an alle durch den Administrator** auf **nur E-Mail**, wird die Ankündigung nicht mehr als GT Nachricht in seinem Posteingang angezeigt. Da GT keine eingehenden E-Mails empfängt, kann ein solcher Benutzer auf diese Ankündigung **nicht antworten**. Wählen Sie »nur E-Mail« nur dann, wenn keine Antwort erwartet wird.
{{% /notice %}}

An den Administrator gerichtete Anfragen — in der Praxis die Anfrage **Sperrung Benutzer Fremddaten/Anfragelimit** eines gesperrten Benutzers — gelangen an den [Hauptadministrator](../intro/userrights/#hauptadmin), also an die Person, deren E-Mail Adresse in `application.properties` unter `gt.main.user.admin.mail` festgelegt ist, und nicht an jeden Administrator. Teilen sich mehrere Personen die Benutzerrolle **Administrator**, so kann der Hauptadministrator eine solche Anfrage an einen anderen Administrator weitergeben, um die Arbeit untereinander aufzuteilen oder um seine eigene Abwesenheit, beispielsweise während der Ferien, zu überbrücken. Der andere Administrator wird über ein Auswahlmenü gewählt, das die übrigen Administratoren mit ihrem Spitznamen auflistet; bleibt die Auswahl leer, verbleibt die Anfrage beim Hauptadministrator.

{{% notice info %}}
Um eine endlose Weiterleitung zu verhindern, lässt GT keine Weiterleitung zu, die einen Zyklus bilden würde — beispielsweise leitet Administrator A an Administrator B weiter, während B bereits an A zurückleitet, oder ein Administrator leitet an sich selbst weiter. Eine solche Einstellung wird abgewiesen.
{{% /notice %}}

| Von             | An             | Nachrichtentyp                                                                                                                                                                       | Standard Kanal          |
|-----------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------|
| Benutzer        | Benutzer       | **Allgemeine Kommunikation von Nutzer zu Nutzer**: Eine direkte Nachricht zwischen zwei Benutzern. Nur ein Administrator kann eine solche Nachricht direkt beginnen; andere Benutzer erreichen einen Empfänger aus einem Kontext heraus. | GT Nachricht            |
| System          | Benutzer       | **Halten einer nicht aktiven Position**: Ein Instrument hat die Eigenschaft "Aktiv bis oder Fälligkeitstag", falls ein solches Instrument nach diesem Datum gehalten wird.           | GT Nachricht            |
| System          | Benutzer       | **Mögliche fehlende Zinsen/Dividenden**: GT versucht, mögliche fehlende Eintragungen bezüglich Zins oder Dividende zu ermitteln. Dieser Algorithmus erzeugt leider auch Fehlwarnungen. | GT Nachricht            |
| System          | Benutzer       | **Änderungsvorschlag für gemeinsame Daten**: Der Eigentümer gemeinsam genutzter Daten wird darüber informiert, dass eine Änderung an diesen Daten vorgeschlagen wurde.              | GT Nachricht            |
| Benutzer/System | Administrator  | **Sperrung Benutzer Fremddaten/Anfragelimit**: Der gesperrte Benutzer möchte zurück in GT. Dies ist auch bei den Benutzereinstellungen sichtbar.                                    | -                       |
| System          | Administrator  | **Mögliche Fehlfunktion Datenlieferanten historisch**: GT informiert den Hauptadministrator, dass eine Datenquelle für historische Kurse möglicherweise nicht mehr funktioniert.    | GT Nachricht            |
| Administrator   | Jeder Benutzer | **Bekanntmachung an alle durch Administrator**: Nur dieser hat die Möglichkeit, eine Nachricht an alle zu senden.                                                                    | GT Nachricht und E-Mail |
| Administrator   | Benutzer       | **Persönliche Nachricht vom Administrator**: Damit kann sich der Administrator direkt mit einem Benutzer unterhalten.      | GT Nachricht und E-Mail |
