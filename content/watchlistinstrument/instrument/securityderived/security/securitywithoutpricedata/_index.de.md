---
title: "Wertpapier ohne Kursdaten"
date: 2026-06-05T10:54:47+01:00
draft: false
weight: 8
archetype: "default"
---
Für bestimmte Wertpapiere wie beispielsweise Festgeld werden keine öffentliche Kurse gestellt. Zu diesem Zweck gibt es "**Historische Kurse für Periode**". Dieser **Reiter** wird automatisch beim Bearbeiten des **Wertpapiers** aktiviert, falls ein **Handelsplatz** ohne Kursdaten ausgewählt wird. Ändert sich der Kurs über die **gesamte Laufzeit** nicht, so entspricht dieser der Angabe der **Stücklung**.

## Reiter "Historische Kurse für Periode"
Hat der Kurs Schwankungen kann dieser unter diesem **Reiter** erfasst werden, dabei wird der Kurs und das Datum für den neuen Kurs eingetragen.

Eine Periode wird erfasst, indem das **Datum für neuen Kurs** und der Kurs eingegeben werden und anschliessend **Übernehmen** angeklickt wird. Ein Kurs gilt ab seinem Datum bis zum nächsten erfassten Datum oder, falls kein weiteres folgt, bis zum Ende der Laufzeit. Über die gesamte Laufzeit wird standardmässig die **Stücklung** als Kurs verwendet, daher muss nur eine Periode erfasst werden, wenn der Kurs davon abweicht. Durch Auswahl einer Zeile lässt sich der Eintrag bearbeiten oder löschen; ein bereits vorhandenes Datum überschreibt den bestehenden Eintrag. Die Änderungen werden erst gespeichert, wenn das **Wertpapier** gespeichert wird.

{{% notice style="info" title="Maximale Anzahl Perioden" %}}
Pro Instrument können standardmässig bis zu 20 Perioden erfasst werden. Eine Administratorin oder ein Administrator kann diese Grenze über die globale Einstellung `gt.max.instrument.historyquote.periods` anpassen. Die Fusszeile der Tabelle zeigt die aktuelle Anzahl und das erlaubte Maximum an.
{{% /notice %}}

Zusätzliche Informationen gibt es im Video von [Kursdaten](../../../).
{{< youtubestartend Zij10hSO9OQ 96 220 >}}
