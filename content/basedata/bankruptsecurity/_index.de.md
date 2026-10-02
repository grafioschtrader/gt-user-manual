---
title: "Instrument ohne Kursdaten"
date: 2026-09-25T22:54:47+01:00
draft: false
weight: 67
archetype: "default"
---
Die [periodische Performance]({{< relref "/reportportfolio/periodperformance" >}}) bewertet einen Klienten nur an Handelstagen, an denen jedes gehaltene Instrument einen Schlusskurs hat. Fehlt der Kurs eines einzigen Instruments, fällt der ganze Tag aus der Auswertung. Reicht diese Lücke bis in die Gegenwart, bleibt kein verwendbarer Handelstag mehr übrig.

Genau das tritt ein, wenn der Emittent eines Instruments keine Kurse mehr liefert — etwa eine Anleihe nach einem Konkurs, deren **Aktiv bis Datum** noch der Verfall ist. Die Konnektoren versuchen weiter, finden aber nichts. Das [lineare Befüllen fehlender Kursdaten]({{< relref "/watchlistinstrument/externaldata/historyquote/pricedata" >}}) schliesst die Lücke einmalig; am nächsten Handelstag ist sie wieder da, und die Auswertungen aller, die das Instrument halten, stehen erneut still.

## Was die Markierung bewirkt
Ein Eintrag in dieser Tabelle ist eine Aussage über das **geteilte Instrument**, nicht über einen einzelnen Klienten. Deshalb erscheint jedes Instrument nur einmal. Der Hintergrundauftrag [30 - Historische- und Innertag-Kursaktualisierung mit Nachführung der Vollständigkeit und Split Kalender update]({{% relref "/admindata/taskdatachangemonitor/taskdescription" %}}#JOB30) füllt nach dem Lauf der Konnektoren die fehlenden Handelstage jedes markierten Instruments mit demselben linearen Befüllen wie die Funktion **Lineares befüllen fehlender Kursdaten**, und zwar bis zum letzten abgeschlossenen Handelstag. Liefert der Datenanbieter an einem Tag doch einen Kurs, bleibt dieser stehen; nur die Tage darum herum werden erzeugt. Eine Lücke am Ende der Reihe wird mit dem letzten bekannten Schlusskurs fortgeschrieben. Bereits geschriebene Kurse werden nicht mehr geändert.

Der Datenkonnektor bleibt am Instrument. Ein später wieder gelieferter Kurs wird ganz normal gespeichert.

{{< mermaid >}}
flowchart TD
    A[Tagesendauftrag 30 startet] --> B[Konnektoren holen vorhandene Kurse]
    B --> C[Markierte Instrumente: fehlende Handelstage linear befüllen]
    C --> D[Periodische Performance hat vollständige Tage]
{{< /mermaid >}}

Ein linear befüllter Kurs ist kein gehandelter Kurs. Bewertet GT ein Instrument damit, erscheint die Spalte **Kurs** gelb, siehe [Aktualität der Kurse]({{< relref "/watchlistinstrument/watchlist/pricefreshness" >}}).

## Wirkung auf die Simulation
Ob ein Instrument noch Kurse liefert und ob es noch gehandelt werden kann, sind zwei verschiedene Fragen. Die Anleihe eines ausgefallenen Emittenten kann weiterhin kotiert werden, obwohl die Börse und die Broker den Handel längst unterbunden haben. Deshalb gibt es neben **Keine Daten seit** ein zweites, davon unabhängiges Datum: **Kein Handel seit**.

Ab dem Tag **Kein Handel seit** kauft und verkauft die [historische Simulation]({{% relref "/algoalert/historicalrun" %}}) das Instrument nicht mehr. Sie erzeugt keine weiteren Coupons und zahlt eine Anleihe bei Verfall nicht zurück. Eine bestehende Position bleibt gehalten und wird zum letzten Kurs bewertet. Ohne dieses Datum gilt das Instrument in der Simulation als handelbar. **Keine Daten seit** hat auf die Simulation keinen Einfluss.

Liefert der Datenanbieter für ein Instrument weiterhin Kurse, obwohl es nicht mehr gehandelt werden kann, lassen Sie **Keine Daten seit** leer und setzen nur **Kein Handel seit**. Ein bereits abgeschlossener Simulationslauf behält das Datum, mit dem er gerechnet wurde; eine Änderung wirkt erst auf einen neuen Lauf.

## Bedienung
Die Verwaltung liegt im **Navigationsbaum** unter **Basisdaten → Instrument ohne Kursdaten**. Die Tabelle zeigt für jedes markierte Instrument:

- **Wertpapier** — Name des Instruments.
- **ISIN** und **Währung** — zur Identifikation.
- **Keine Daten seit** — ab welchem Tag der Emittent nach Ihrer Einschätzung keine Kurse mehr liefert. Das Datum dient nur der Dokumentation; das Befüllen beginnt immer beim letzten tatsächlich vorhandenen Schlusskurs.
- **Kein Handel seit** — ab welchem Tag das Instrument nicht mehr gehandelt werden kann. Die historische Simulation handelt es ab diesem Tag nicht mehr, siehe [Wirkung auf die Simulation](#wirkung-auf-die-simulation).
- **Neuester gelieferter Kurs** — jüngster Schlusskurs, den ein Datenanbieter, ein Import oder Sie selbst erfasst haben, also kein befüllter Kurs.
- **Neuester Kurs** — jüngster Schlusskurs überhaupt, befüllte Tage eingeschlossen. Liegt dieses Datum nach **Neuester gelieferter Kurs**, arbeitet der Tagesendauftrag.
- **Bemerkung** — weshalb das Instrument markiert wurde.

Eine neue Zeile legen Sie über das **Plus-Symbol** in der Tabellenüberschrift an. Das Instrument wählen Sie im Dialog **Instrument zuweisen**; die Suche beschränkt sich auf Wertpapiere, abgeleitete Instrumente und Währungspaare erscheinen nicht. Nach dem Speichern lässt sich das Instrument nicht mehr wechseln — die Zeile gilt genau diesem Instrument. Eine bestehende Zeile bearbeiten Sie über das **Stift-Symbol**, löschen über das **Mülltonnen-Symbol**.

{{% notice style="info" title="Mindestens ein Schlusskurs" %}}
Ein Instrument ohne jeden Schlusskurs kann nicht markiert werden. Das Befüllen braucht einen bekannten Kurs als Ausgangspunkt und würde sonst stillschweigend nichts tun.
{{% /notice %}}

Ist das Instrument bereits markiert, weist GT den zweiten Eintrag mit einer Meldung zurück.

## Benutzerrechte
Markiert werden darf nur ein Instrument, das Sie selbst bearbeiten dürfen. Benutzer der Rolle **Benutzer mit Limits** und **Benutzer ohne Limits** dürfen deshalb nur Instrumente markieren, die sie erstellt haben. Benutzer der Rolle **Privilegierter Benutzer** und **Administrator** dürfen jedes Instrument markieren.

Im Unterschied zu anderen geteilten Daten entsteht hier **kein Datenänderungswunsch**. Wer das Instrument nicht bearbeiten darf, erhält eine Ablehnung. Ein wartender Änderungswunsch würde die Auswertungen aller Halter so lange offen lassen, bis jemand zustimmt — genau die Lage, die diese Markierung beenden soll.

Für die Rolle **Benutzer mit Limits** gelten zusätzlich die Limiten der Informationsklasse **Instrument ohne Kursdaten**: drei Änderungen pro Tag und 200 Einträge pro Ersteller. Der Administrator pflegt beide Werte unter [Limite Informationsklasse]({{% relref "/admindata/entitylimit" %}}).
