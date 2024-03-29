---
title: "Basisdaten - Datenänderungswunsch"
date: 2021-04-18T22:54:47+01:00
draft: false
weight: 25
archetype: "default"
---
## Basisdaten
**Basisdaten** sind **geteilte Daten** die von denn Benutzer einer **GT-Instanz** benutzt und modifiziert werden.

### Statisches Hauptelement "Basisdaten"
Auf dem statischen Hauptelement **Basisdaten** im **Navigationsbereich** ist die Funktion **Datenänderungswunsch** implementiert. Obwohl die **Entitäten** von gewissen Informationsklassen geteilt werden, können die nicht von jedem Benutzer beliebig verändert werden.

## Datenänderungswunsch
Das Zusammenspiel von **geteilten Daten**, dem **Besitzer einer Entität** und **Benutzerrechte** können dazu führen, dass Sie einen **Datenänderungswunsch** kreieren oder einen erhalten.
+ Die Benutzer der Gruppe **Limits** oder **ohne Limits** kreieren einen **Datenänderungswunsch** falls diese eine **geteilte Entität** bearbeiteten und speichern. Sie erhalten einen **Datenänderungswunsch** falls jemand der Gruppe **Limits** oder **ohne Limits** einer seiner erstellten **geteilten Entität** verändern möchte.
+ Ein Benutzer der mit **privilegierten oder administrativen Benutzerrechten** kreiert nie einen **Datenänderungswunsch**. Jedoch erhalten diese alle **Datenänderungswünsche**, auch wenn die betroffene **Entität** ursprünglich von einem anderen Benutzer erstellt wurde.

Im folgenden Video sehen wir dies in einem Praxisbeispiel:
{{< youtube zNXjOknUZjo >}}

### Einschränkungen Datenänderungswunsch
Für bestimmte Entitäten werden gewisse Datenänderungswünsche nicht unterstützt.

### Abgeleitetes Instrument
Für die **Preisberechnungsformel** kann kein Datenänderungswunsch angebracht werden.
