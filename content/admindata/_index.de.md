---
title: "Administrative Daten - Nachrichtensystem"
date: 2021-03-29T22:54:47+01:00
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
In der Ansicht **Nachricht** können **Anfangsnachrichten** nur an eine **Benutzerrolle** gesendet werden. Alle Nachrichten eines solchen **Nachrichtenthemas** sind für alle Benutzer der angesprochenen Benutzerrolle sichtbar. Allerdings kann in einem solchen Nachrichtenthema auch privat geantwortet werden, also unsichtbar für die anderen Benutzer der Benutzerrolle. Dies ist der Hauptgrund für die Existenz der Tabellenspalte “**Anzahl der Antworten**”.

#### Nachrichtenthema durch Systemnachricht an Benutzers
Auf Nachrichten des Systems sollte reagiert und die mögliche Aktion durchgeführt werden. Es besteht die Möglichkeit, die Systemnachricht an eine bestimmte Benutzerrolle zu **beantworten**. Dies kann verwendet werden, wenn der Empfänger nicht in der Lage ist, das in der Systemnachricht beschriebene Problem selbst zu lösen.

### Einstellung Nachrichten
Hier wird die Zuordnung zwischen **Nachrichtentyp** und **Kommunikationskanal** vorgenommen. Zurzeit unterstützt GT die zwei Kommunikationskanäle **GT Nachricht** und **E-Mail**. Werden keine individuellen Einstellungen durch den Benutzer vorgenommen, gelten die Standardeinstellungen. Nur die Benutzerrolle »Administrator« hat die Möglichkeit, die Nachricht an einen anderen Benutzer weiterzuleiten.
- Eine Nachricht, die an mehrere Benutzer versendet wird, geht immer zum Kommunikationskanal **GT Nachricht**. Zusätzlich kann der Benutzer noch einstellen, ob er eine **E-Mail** erhalten will. 

| Von             | An             | Nachrichtentyp                                                                                                                                                                       | Standard Kanal          |
|-----------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------|
| System          | Benutzer       | **Halten einer nicht aktiven Position**: Ein Instrument hat die Eigenschaft "Aktiv bis oder Fälligkeitstag", falls ein solches Instrument nach diesem Datum gehalten wird.           | GT Nachricht            |
| System          | Benutzer       | **Mögliche fehlende Zinsen/Dividenden**: GT versucht, mögliche fehlende Eintragungen bezüglich Zins oder Dividende zu ermitteln. Dieser Algorithmus erzeugt leider auch Fehlwarnungen. | GT Nachricht            |
| Benutzer/System | Administrator  | **Sperrung Benutzer Fremddaten/Anfragelimit**: Der gesperrte Benutzer möchte zurück in GT. Dies ist auch bei den Benutzereinstellungen sichtbar.                                    | -                       |
| Administrator   | Jeder Benutzer | **Bekanntmachung an alle durch Administrator**: Nur dieser hat die Möglichkeit, eine Nachricht an alle zu senden.                                                                    | GT Nachricht und E-Mail |
| Administrator   | Benutzer       | **Persönliche Nachricht vom Administrator**: Damit kann sich der Administrator direkt mit einem Benutzer unterhalten.      | GT Nachricht und E-Mail |
