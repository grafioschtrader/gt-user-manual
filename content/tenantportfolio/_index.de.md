---
title: "Klient und Portfolios"
date: 2021-03-14T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
GT definiert einen **Klient** aus dem Zusammenzug aller **Portfolios** und **Watchlists**. Zusätzlich enthält er die Informationen bezüglich der Auswertung über alle Portfolios. Ein Klient kann ein oder mehrere Portfolios enthalten, die Anzahl möglicher Portfolios ist beschränkt. Im folgenden Entity-Relationship-Diagramm ist sind die Beziehungen dieser **privaten Daten** dargestellt. Daraus entnehmen wir, beispielsweise das eine Konto einem Portfolio zugeordnet ist.

{{< mermaid >}}
erDiagram
    Klient ||--|{ Portfolio : hat
    Portfolio ||--|{ Konto : hat
    Portfolio ||--|{ Depot : hat
    Klient ||--|{ Watchlist : hat
    Watchlist }o--|{ Instrument : hat
    Konto ||--|{ Transaktion : hat
    Transaktion |o--o| Transaktion : referenziert
    Transaktion }o--o| Instrument : referenziert
    Transaktion }o--o| Depot : referenziert
{{< /mermaid >}}

Das folgende Video erklärt Ihnen die Erstellung von Portfolios, Depots und Kontos:
{{< youtube Xhc4T6_HluQ >}}
