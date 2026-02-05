---
title: "Verbindungs-API-Key"
date: 2022-02-28T01:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
Einige externe Datenquellen verlangen einen **API-Key**, unabhängig dessen Angebot. Ein Benutzer mit dem **Administrator**-Benutzerrechten kann diese **API-Keys** einsehen und bearbeiten.

## Erstellen und bearbeiten Verbindungs-API-Key
Pro unterstützte Datenquelle kann ein API-Key und der Abonnementstype gesetzt werden. Ein Verbindungs-API-Key wird im Hauptbereich in der Ansicht Verbindungs-API-Key erstellt, bearbeitet und gelöscht. Diese Ansicht erreichen Sie im Navigationsbereich auf dem statischen Unterelement **Verbindungs-API-Key** unter **Administrative Daten**.
- **Erstellen** eines Verbindungs-API-Key über das Kontextmenü.
- **Bearbeiten** eines Verbindungs-API-Key über das Kontextmenü bei selektiertem Verbindungs-API-Key.
- **Löschen** eines Verbindungs-API-Key über das Kontextmenü bei selektiertem Verbindungs-API-Key.

## Eigenschaften und Tabellenspalten
Die Eigenschaft **Datenquelle** ist einzigartig und widerspiegelt ob GT einen entsprechenden Konnektor für diese Datenquelle implementiert hat. 
- **Datenquelle**: Name der Datenquelle.
- **API-Key**: Den API-Key der Datenquelle.
- **Abonnementstyp**: Die meisten Datenquelle haben gestaffeltes Angebot. Möglicherweise nutzt GT das Angebot der Datenquelle gemäss dem Abonnementstyp.

{{% notice note %}}
**API-Key ist verschlüsselt in der Datenbank**\
Der **API-Key** wird verschlüsselt in der Datenbank gespeichert. Dabei benutzt GT als Passwort die Umgebungsvariable `JASYPT_ENCRYPTOR_PASSWORD`. Diese Umgebungsvariable wird auch von `application.properties` verwendet.
{{% /notice %}}