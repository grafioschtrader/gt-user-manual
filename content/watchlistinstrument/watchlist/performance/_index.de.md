---
title: "Performanz"
date: 2021-03-09T10:54:47+01:00
draft: false
weight : 10
chapter: true
---
## Performanz
Durch die Selektion einer Watchlist im **Navigationsbaum** wird diese Ansicht standardmässig im Hauptbereich angezeigt. Die **Watchlist-Ansicht** zeigt die Instrumente mit ihren offenen und gehandelten Positionen an. Zusätzlich ist es die einzige Möglichkeit einer **Innertag Kursaktualisierung** in GT.

### Innertag Kursaktualisierung
Die Selektion dieser Watchlist-Ansicht kann eine **Innertag Kursaktualisierung** bewirken, dabei wird der vorgegebene Zeitabstand seit der letzten Kursaktualisierung berücksichtigt. Dieser **Zeitabstand** wird auch bei jedem einzelnen Instrument für eine mögliche Kursaktualisierung herangezogen. Wann die letzte Aktualisierung der Innertag Kurse erfolgte, steht in der Titelzeile der Watchlist. Der Zeitabstand kann der Eigenschaft "gt.w.intra.update.timeout.seconds" der **Globale Einstellung** entnommen werden.

### Funktionen
Diese **Performanz-Ansicht** hat nur die allgemeinen Funktionen auf dem Kontextmenü implementiert.

#### Menü "Ansicht" der Menüleiste
**Zeitrahmen**: Die Einstellung über den Menüpunkt **Zeitrahmen** bestimmt die Periode für die Berechnung der Tabellenspalte "**Zeitrahmen %**". Falls die Zeitperiode gleich oder grösser ein Jahr ist, kommt die **geometrische Durchschnittsrendite** bei der Performanceberechnung zur Anwendung.
{{% notice info %}}
Die Dividenden werden bisher noch nicht eingerechnet. Dies verkompliziert den Vergleich zwischen einem Kurs- und einem Performance-Index. Da im **Kursindex**, auch **Preisindex** genannt, ausschliesslich die Kursbewegung berücksichtigt werden. Hingegen sind im **Performanceindex** auch die Ausschüttungen miteinbezogen.
{{% /notice %}} 

### Expandierende Tabellenzeile
Ein vorhandenes **Expander-Symbol** beim Instrument ist ein Hinweis auf seine bestehenden **Transaktionen**. Weitere Informationen finden Sie unter der Hilfe für diese Ansicht der Transaktionen.