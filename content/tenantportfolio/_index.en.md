---
title: "Cliend and Portfolios"
date: 2018-01-13T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
GT defines a **client** from the aggregation of all **portfolios** and **watchlists**. It also contains information on the evaluation of all portfolios. A client can contain one or more portfolios, the number of possible portfolios is limited. The following entity relationship diagram shows the relationships between these **private data**. From this we can see, for example, that an account is assigned to a portfolio.

{{< mermaid >}}
erDiagram
    Client ||--|{ Portfolio : has
    Portfolio ||--|{ Account : has
    Portfolio ||--|{ Securities-Account : has
    Client ||--|{ Watchlist : has
    Watchlist }o--|{ Instrument : has
    Account ||--|{ Transaction : has
    Transaction |o--o| Transaction : references
    Transaction }o--o| Instrument : references
    Transaction }o--o| Securities-Account : references
{{< /mermaid >}}