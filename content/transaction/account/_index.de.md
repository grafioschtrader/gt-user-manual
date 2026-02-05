---
title: "Kontotransaktion"
date: 2021-04-20T02:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
Bei Kontotransaktion ist kein Instrument beteiligt.

## Konto Transaktionsarten
Grundsätzlich gibt es **zwei Dialoge** für die Bearbeitung von Kontotransaktionen. Der einte Dialog beschränkt sich auf ein Konto und der andere umfasst zwei Konten.

### Standard Kontotransaktionarten
Standard Kontotransaktionarten werden immer in der Währung des entsprechenden Kontos durchgeführt, folgende Kontotransaktionen sind damit gemeint:
- **Auszahlung**: Die Entnahme von Geld ab einem bestimmten Konto.
  + Eine Auszahlung kann **Transaktionskosten** haben.
- **Einzahlung**: Die Zuführung von  Geld in ein bestimmtes Konto.
- **Konto- und Depotkosten**: Möglicherweise müssen Sie Kosten für die Kontoführung zahlen. Dazu dient diese Transaktionsart. Falls Sie ein **Depot** angeben, werden diese Auslagen dem Depot und nicht dem Konto angerechnet.
  + Es sind auch negative Kosten möglich, beispielsweise für eine Gutschrift auf dem Depot oder für eine Stornobuchung.
- **Kontozins**: Der Sparzins den Sie für Ihr Geld bekommen. Für negativ Zinsen können Sie auch einen negativen Betrag eingeben.
  + Ein negativer **Kontozins** ist möglich, beispielsweise fü negative Zinsen oder eine Stornobuchung.
  + Kontozins kann mit einer **Steuer** belastet sein.

### Kontoübertrag
Diese Transaktion erzeugt eine Auszahlungs- und Einzahlungstransaktion, d.h. es entstehen zwei Transaktionen die miteinander verbunden sind. Diese Transaktionsart kann auch Portfolio übergreifend sein und unterstützt die **Währungskonvertierung**. Die **Währungskonvertierung** wird oft angewendet, falls innerhalb eines Portfolios eine Aus- bzw. Einzahlung zwischen zwei Kontos unterschiedlicher Währungen stattfindet.

#### Bestimmung Basiswährung und Kurswährung
Es muss ein **Währungspaar** bestimmt werden, falls mit dem **Kontoübertrag** eine **Währungskonvertierung** statt findet. Die **Basiswährung** entspricht der Währung des **Belastungskontos** und die **Kurswährung** der des **Gutschriftkontos**.  

{{% notice note %}}
Währungsgewinne werden ab dem Zeitpunkt der Einzahlung bis zum Zeitpunkt der Auszahlung beim entsprechenden Konto ausgewiesen. Daher sollte der Wechselkurs möglichst exakt die Realität abbilden, d.h. es sollte ein Wechselkurs im Bereich des Tagestief und Tageshöchst am dem Tag der Transaktion eingegeben bzw. übernommen werden.
{{% /notice %}}

## Erstellen einer neuen Kontotransaktion
Eine neue **Kontotransaktion** kann nur auf der **Portfolio-Ansicht** erstellt werden. Beim selektierten **Konto** kann über das **Kontextmenü** einer der Kontotransaktions-Dialoge aufgerufen werden.

## Bearbeiten einer bestehenden Kontotransaktionen
Eine **bestehende Kontotransaktion** kann über unterschiedliche Ansichten bearbeitet werden:
- In der Ansicht [Portfolio](../../reportportfolio/portfolios/) bei der expandierenden Tabellenzeile des entsprechenden **Kontos**. Dies ist eine **Saldoansicht** und erlaubt die Nachverfolgung des saldierten Kontostands. Dieser eignet sich bestens für den Vergleich mit dem Kontoauszug ihrer Handelsplattform.
- In der Ansicht [Transaktionen des Portfolios](../../reportportfolio/transactionlist/), auf der Ebene der einzelnen Portfolios.
- In der Ansicht der [Transaktionen der Portfolios](../../reportportfolio/transactionlist/). Dies ist die höchste Ebene im Navigationsbereich.

## Video mit Praxis von Kontotransaktionen
Kurzer Überblick über Transaktionen, danach Praxis mit zwei Portfolios. Erfassen von Kontotransaktionen wie Einzahlung, Auszahlung, Konto- bzw. Depotkosten und Kontozins. Dabei sind Haupt- und Fremdwährung beteiligt. Eine bestehende Kontotransaktion ädern, den Saldoverlauf und Transaktions-Ansicht betrachten. Abschliessend in der **Ansicht Portfolio** die getätigten Kontotransaktionen erläutern.
{{< youtube C9awa_u1NaU >}}
