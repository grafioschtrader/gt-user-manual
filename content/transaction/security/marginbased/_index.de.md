---
title: "Margin basierte Transaktion"
date: 2021-06-16T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
Transaktionen für **Forex** und **CFD** sind **Margin basierte Transaktionen**. Dabei gibt es eine Transaktion, welche die Position **öffnet**, dazu eine oder mehrere Transaktionen die diese geöffnete Position **schliessen**. Erst das Schliessen der Position erzeugt eine kontorelevante Buchung.

{{% notice warning %}}
Die **Margin** oder **Sicherheitsleistung**, ein wichtiger Teil des Hebelhandels, wird in GT nicht berücksichtigt. Es handelt sich um eine **Sicherheitsleistung** und nicht um Gebühren oder **Transaktionskosten**. Sie dient dazu das der Trader nicht jenseits seiner finanziellen Möglichkeiten hohe Handelsgeschäfte tätigt. **GT eignet sich nicht als Überwachungs-Tool für das Eingehen von hohen gehebelten Risiken**.
{{% /notice %}}

## Warum GT das Margin-Trading unterstützt?
Manchmal gibt es am Markt gewisse Gelegenheiten wo es für den Trader schwierig ist, mit den Instrumenten wie Aktie oder ETF einen Vorteil daraus zu gewinnen. Auch ohne den Besitz des zugrunde liegenden Vermögenswert kann ein Trader möglicherweise von einer Marktsituation profitieren. Hierzu zwei Beispiele:
- Wer glaubt das der USD gegenüber dem CHF unterbewertet ist. Kauft den USD gegenüber den CHF und verkauft ihn nachdem er zugelegt hat. Dieser **Forex-Trade** ist wesentlich günstiger als der reale Kauf und Verkauf von USD. 
- Angenommen Sie denken der SMI Index sei wesentlich überbewertet, dann können Sie dem SMI 20 als CFD Verkaufen, d.h. Sie öffnen eine Short-Position. Später wenn der SMI Index gefallen ist, schliessen Sie diese Short-Position und kaufen somit diesen Index günstiger zurück.

Es ergeben sich **Marktrisiken** während Sie diese **Handelspositionen offen haben**. GT versucht diese Marktrisiken nachzubilden. Letztendlich haben solche Transaktionen Auswirkungen auf die Performance eines Portfolios und müssen daher angemessen nachgebildet werden.

{{< youtube Rq7HK3dfeFA >}}

### Forex
**Forex** ist eine technisch einfache und finanziell günstige Art mit Währungspaaren zu handeln, daher kann dieser Handel in GT nachgebildet werden.
{{% notice warning %}}
Die **offenen Devisenpositionen** werden derzeit nicht mit den bestehenden **Cash-Positionen saldiert**. **Mögliche Verluste könnten daher die Cash-Position übersteigen**.
{{% /notice %}}

### CFD
Wenn die Preise an den Finanzmärkte nur steigen würden, dann gäbe es in **GT** wahrscheinlich keine Unterstützung für dieses Finanzinstrument. Wie wir wissen, ist dies nicht der Fall. Mit **CFD** können Sie beispielsweise Ihre Aktienpositionen kurzzeitig absichern oder grundsätzlich auf fallenden Märkte setzen.

## Position öffnen
Da sowohl Short- und Long-Positionen unterstützt werden, kann auf fallende oder steigende Kurse spekuliert werden. Die **Position** wird mit dem **Verkauf** oder **Kauf** geöffnet**. Dabei muss das entsprechende Instrument selektiert sein. Diese beiden Funktionen sind wie üblicherweise über das Kontextmenü erreichbar.

## Position schliessen
Beim Schliessen der Position muss eine der öffnende Position des Instrumentes selektiert sein. Diese werden mit der **expandierende Tabellenzeile** des Instrumentes sichtbar. Auf der oberen hierarchischen Struktur in der **Hierarchietabelle** werden die **öffenten Positionen** dargestellt. Offene Positionen sind zur besseren Erkennbarkeit gelb unterlegt. Die **Finanzierungskosten** und die **schliessenden Positionen** werden als Konten dieser **öffenten Postionen** dargestellt.

## Finanzierungskosten
Bei Forex und CFD-Positionen können Kosten entstehen die periodisch oder auf einmal beglichen werden müssen. Alle diese Kosten können mittels der Transaktion **Finanzierungskosten** erfasst werden.
- Mehrere **Finanzierungskosten** sind für eine geöffnete bzw. geschlossene Position möglich.
- Es können auch negative **Finanzierungskosten** erfasst werden.

### Eigenschaften
Die meisten Eigenschaften sind selbsterklärend und werden daher nicht weiter erläutert, zudem ist bei gewissen Eigenschaften eine Quickinfo vorhanden.
- **Position öffnen oder schliessen**
  - Obwohl die Eigenschaften **Steuer** und **Transaktionskosten** wahrscheinlich für diese Art der Transaktionen nicht genutzt werden, werden sie trotzdem zur Verfügung gestellt.
  - **Wertpapier Risiko**: Es ist das berechnete Risiko das mit dieser Position eingegangen wird.
