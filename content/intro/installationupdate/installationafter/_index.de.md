---
title: "GT nach der Installation"
date: 2026-11-25T12:00:00+01:00
draft: false
weight: 60
archetype: "default"
---
Nach der Installation von GT und mit dem **ersten Start** des **Back-End** werden bestimmte geteilte **Informationsklassen** mit einigen **Entitäten** befüllt. Der Benutzer mit der E-Mail-Adresse, die bei der Installation in `application.properties` unter der Eigenschaft `g.main.user.admin.mail` hinterlegt wurde, erhält mit seiner Registrierung die Rechte des [Hauptadministrators](../../userrights/#hauptadmin). Wenige Minuten danach wird er zum Besitzer dieser Entitäten. Die folgenden Angaben beziehen sich auf eine Neuinstallation von GT 0.40.2.
- **Instrumente**: Die Selektion umfasst 65 **Wertpapiere** und drei **Währungspaare**. Darunter sind Indizes, aus deren Kursen GT die Handelstage einzelner Handelsplätze ableiten kann. Die Instrumente nutzen eine Vielfalt unterschiedlicher **Konnektoren** und können Ihnen als Vorlage bzw. Spickzettel für Ihre eigenen Instrumente dienen.
- **Splits** und **Dividenden**: Für einige der beigefügten Wertpapiere sind bereits **Splits** erfasst. Die **Dividenden** lädt GT nach dem ersten Start über die eingerichteten Konnektoren.
- **Historische Kursdaten**: Es werden keine Kurse mitgeliefert. GT lädt die **historischen Kursdaten** der beigefügten Instrumente nach dem ersten Start und führt sie danach kontinuierlich bis zum aktuellen Datum nach. Wie weit die Kurshistorie zurückreicht, hängt vom Instrument und vom Datenanbieter ab.
- **Basisdaten**:
  - **Anlageklassen**: GT liefert 84 **Anlageklassen** mit, deren **Unter Anlageklasse** vorwiegend nach Ländern und Regionen ausgerichtet ist. Warum sich diese [geografische Einteilung](../../../basedata/instrumentbased/assetclass/#unter-anlageklasse) später nur mit Aufwand ändern lässt, erläutert das Kapitel zu den Anlageklassen.
  - **Handelsplatz**: Die 24 «wichtigsten» [Handelsplätze](../../../basedata/instrumentbased/stockexchange/) sind mit ihren Handelskalendern erfasst.
  - **Handelsplattform Plan**: Die 18 [Handelsplattform-Pläne](../../../basedata/tradingplatformplan/) dienen der Zuordnung der **Import Vorlagengruppe**.
  - **Import Vorlagengruppe**: Für den Import von PDF-Belegen und CSV-Dateien stehen 44 Vorlagen in sieben Gruppen zur Verfügung, beispielsweise für Swissquote und DEGIRO. Die Gruppe «Grafioschtrader» dient zum Einlesen von Belegen und Transaktionslisten, die GT selbst exportiert hat. Falls ein Beleg Ihrer Handelsplattform nicht erkannt wird, prüfen Sie die passende [Import Vorlagengruppe](../../../basedata/imptranstemplate/).
- **Administrative Daten**:
  - **Handelskalender Global**: Der [Handelskalender global](../../../admindata/tradingcalendar/) ist die **Obermenge** aller Handelstage. Ein einzelner Handelsplatz kann an weiteren Tagen geschlossen sein, etwa an einem lokalen Feiertag. Diese Ausnahmen ermittelt GT je nach Handelsplatz aus einem Regelsatz oder aus den Kursen eines Referenzindex. Vorbelegt sind die Jahre 2000 bis 2027 sowie 2029 und 2030. Für 2026 und 2027 gelten Montag bis Freitag als mögliche Handelstage, ausgenommen der 1. Januar und der 25. Dezember. Bei einem Update ergänzt GT jedes dieser beiden Jahre nur, wenn es noch keinen einzigen Eintrag hat.
  - **Globale Einstellungen**: Mit den [globalen Einstellungen](../../../admindata/globalsettings/) kann der Administrator das systemweite Verhalten von GT beeinflussen.

{{% callout style="warning" title="Handelskalender 2028" %}}
Das Jahr 2028 ist bei einer Neuinstallation leer. Bevor Sie Transaktionen für 2028 erfassen, öffnen Sie als Administrator den [Handelskalender global](../../../admindata/tradingcalendar/), wählen Sie 2028, markieren Sie die Handelstage und speichern Sie das Jahr.
{{% /callout %}}

{{< youtube tsW2oCACLLA >}}
