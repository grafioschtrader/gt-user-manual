---
title: "Abgeleitetes Instrument"
date: 2022-04-10T22:54:47+01:00
draft: false
weight: 8
archetype: "default"
---
Ein abgeleitetes Instrument ermöglicht, die Kursdaten von einem oder mehreren Wertpapieren durch eine **Formel** von minimal einem anderen Instrument berechnen zu lassen. Somit hat ein abgeleitetes Instrument immer eine Abhängigkeit zu mindestens einem anderen Instrument. Ein abgeleitetes Instrument kann einem **Handelsplatz** und **Anlageklasse** zugeordnet werden und ist somit in GT handelbar.

### Formel
Eine Formel besteht aus Zahlen, Variablen, mathematischen und boolesche Operatoren und möglichen Funktionen. GT verwendet das Framework [EvalEx - Java Expression Evaluator](//github.com/uklimaschewski/EvalEx), daher siehe die Webseite für die möglichen Funktionen.

#### Variable die Zuordnung zum einem Instrument 
Maximal können **5** Variablen mit den Buchstaben **o, p, q, r, s** in einer Formel benutzt werden. Die Variablen müssen einem Wertpapier zugeordnet werden. Diese Variablen sind die Platzhalter für den entsprechenden **historischen** oder  **Innertag-Kurs**.

### Abgeleitetes Instrument in der Praxis
Ein Praxisbeispielt mit einem abgeleiteten Instrument. Von einer Feinunze Gold in USD zu 100 Gramm Gold in CHF. Dabei wird ein Wertpapier, Währungspaar und eine Formel für die Berechnung der historischen- und Innertag-Kurse genutzt.
{{< youtube iGJWAh55VkY >}}

Zusätzliche Informationen gibt es im Video von [Kursdaten](../../../).