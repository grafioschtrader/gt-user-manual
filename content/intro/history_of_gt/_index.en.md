---
title: "History of GT"
date: 2026-11-19T12:00:00+01:00
draft: false
weight: 2
archetype: "default"
---
Grafioschtrader (GT) started in 2016 and became publicly available on GitHub in March 2021. In 2026, the project therefore looks back on ten years of development. This page highlights selected milestones: new capabilities for investors as well as major changes behind the user interface.

## More than just a portfolio tracker
From the very beginning I wanted more than a portfolio tracker. A tracker mainly answers two questions: what do I own, and how has it developed? Yet the historical price data that GT needs for valuation anyway already allows much that goes far beyond that. I found the correlation between instruments of different sectors, asset classes or countries particularly exciting. Do Swiss equities, gold and US Treasury bonds move together, or do they offset each other? And does this relationship persist over time? The correlation matrix and the rolling correlation were therefore among the early extensions after the publication on GitHub.

Over the years this grew into a whole toolbox that examines instruments regardless of one's own holdings:
- The [correlation matrix]({{% relref "/watchlistinstrument/correlation" %}}) shows within a correlation set how strongly any instruments move together, and the rolling correlation shows how this relationship changes over time.
- The [seasonality heatmap]({{% relref "/watchlistinstrument/seasonalityheatmap" %}}) places the monthly or quarterly returns of several years side by side and reveals recurring patterns over the course of the year.
- A [derived instrument]({{% relref "/watchlistinstrument/instrument/securityderived/derivedinstrument" %}}) calculates its prices with a formula from other instruments, for example 100 grams of gold in Swiss francs from the gold price in dollars per troy ounce and the exchange rate. It can then be evaluated like any other instrument.
- The watchlist is not merely a price list but an observation post. The [Indicators]({{% relref "/watchlistinstrument/watchlist/indicator" %}}) tab compares instruments using self-defined key figures such as the distance to the 200-day moving average, the volatility, the beta against a benchmark instrument or the price/earnings ratio. The [market map]({{% relref "/watchlistinstrument/watchlist/treemap" %}}) shows price movements at a glance, weighted by market capitalisation.
- A lightweight technical analysis supplements the [price chart]({{% relref "/watchlistinstrument/eodchart" %}}) with moving averages, the RSI and drawing tools for your own lines and markings.

An honest remark on technical analysis: I personally do not think much of predicting prices from past patterns. GT will therefore not become a technical analysis tool. Some simple indicators are nevertheless useful, not as a forecast but as orientation. How far below its high is a price, how strongly does it fluctuate, and is it rather expensive or cheap compared with its long-term average? An indicator column answers such questions faster than looking at many individual charts.

On the side of one's own assets, too, GT goes beyond tracking holdings:
- The [performance]({{% relref "/reportportfolio/periodperformance" %}}) of a period shows risk figures such as volatility and drawdown alongside time- and money-weighted returns.
- The [transaction costs]({{% relref "/reportportfolio/transactioncosts" %}}) report shows what trading actually cost, and the fee model comparison checks whether a configured fee model correctly predicts the costs actually incurred. Such a model estimates the costs of a planned trade in advance.
- For the tax return, GT compares the recorded income of Swiss tenants with the official tax values of the [price list]({{% relref "/reportportfolio/dividends" %}}). Different [cost basis methods]({{% relref "/reportportfolio/costbasismethod" %}}) and the [German tax help]({{% relref "/reportportfolio/germantax" %}}) complement this.
- With [alerts and rule-based strategies]({{% relref "/algoalert" %}}), GT monitors prices and holdings. A simulation replays how an allocation or strategy would have behaved in the past without touching the real portfolio. GT never trades on its own; the decision remains with the investor.

Finally, data quality matters to me, because each of these analyses is only as good as the prices it is based on. The [completeness of securities EOD]({{% relref "/admindata/historyquotequality" %}}) view uncovers gaps, the trading calendars distinguish missing prices from exchange holidays, and with [GTNet]({{% relref "/admindata/gtnet" %}}) GT instances can help each other out with price data.

## The beginnings before GitHub
I started GT in 2016 as a JavaFX desktop application, already using Spring Boot for the backend. Initially, I was not enthusiastic about web applications and could not warm to JavaScript. As single-page frameworks evolved, that changed: Angular appealed to me immediately, and PrimeNG provided suitable interface components. This combination let me develop at a similar pace to JavaFX, with a wide choice of JavaScript libraries, especially for charts.

At the beginning of 2017, I therefore moved the application to Angular and PrimeNG. The Spring Boot backend remained its foundation. Today's web application grew out of that change.

GT initially supported a single tenant. Because installation was demanding, I wanted investors to be able to use it without installing and operating their own instance. This led to multi-tenancy: several investors can share one installation while keeping their data separate. The change made the project substantially more complex and multiplied the development effort. The exact date of this introduction before GitHub has not been recorded.

## Key milestones
The timeline condenses the history into a few milestones. Spacing is schematic and does not represent elapsed time. The table below explains further developments and their timing.

{{< mermaid >}}
---
config:
  timeline:
    width: 100
    padding: 10
    taskMargin: 10
  themeVariables:
    fontSize: 20px
---
timeline
    2016 : JavaFX and Spring Boot
    Early 2017 : Move to a web application
    March 2021 : Public on GitHub
    2024 : User-defined fields
    2025–2026 : Reusable libraries
    2026 : GTNet, tax data and Docker
    Aug.–Nov. 2026 : Angular 22, zoneless and signals
{{< /mermaid >}}

## Selected developments in detail
Dates refer to releases or related phases of work. Refactoring changes the internal structure, often improving maintainability and reuse. The libraries and GTNet in particular evolved across several releases. A yellow background in the “Period / version” column marks refactorings without additional user functionality.

| Period / version | Focus | Development and significance | Evidence |
|---|---|---|---|
| 2016 | Project start | A JavaFX desktop application with a Spring Boot backend forms the starting point of GT. | Developer's recollection. |
| Early 2017 | Move to the web | Angular and PrimeNG replace the JavaFX interface. Development speed and the choice of chart libraries drive the decision. | Developer's recollection. |
| Before March 2021, exact date unrecorded | Multi-tenancy | The single-tenant application becomes a shared installation with separate investor data. Investors can use GT without installing it themselves. | Developer's recollection. |
| 5 March 2021 | Public development | The first GitHub commit makes the existing application publicly traceable. The GitHub history thus begins several years after the project itself. | [First commit](https://github.com/grafioschtrader/grafioschtrader/commit/a1c45336ab). |
| May–June 2021, 0.14–0.18 | Transaction import and analysis | Import templates become exchangeable and CSV/PDF imports improve. Basic correlation-matrix functions and dividend-data updates are added. | [Static release texts in 0.14](https://github.com/grafioschtrader/grafioschtrader/blob/V0.14.0/frontend/src/assets/i18n/en.json), [0.18](https://github.com/grafioschtrader/grafioschtrader/blob/V0.18.0/frontend/src/assets/i18n/en.json). |
| November–December 2021, 0.21–0.22 | Correlation and statistics | Rolling correlation shows how the relationship between instruments changes over time. Statistical evaluations extend the correlation view. | [Static release texts](https://github.com/grafioschtrader/grafioschtrader/blob/V0.22.0/frontend/src/assets/i18n/en.json). |
| April–October 2022, 0.25–0.28 | Instruments and exchanges | Support for inverse and leveraged ETFs and ETNs improves. Trading venues receive a unique identifier through the market identifier code, MIC. | [Release texts for 0.25](https://github.com/grafioschtrader/grafioschtrader/blob/V0.26.0/frontend/src/assets/i18n/en.json), [release 0.28](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.28.0). |
| April 2023, 0.30 | Notifications and data checks | Users can forward categories of internal messages to their email address. A daily check highlights certain portfolio inconsistencies, such as instruments held beyond maturity. | [Message forwarding](https://github.com/grafioschtrader/grafioschtrader/issues/62), [consistency check](https://github.com/grafioschtrader/grafioschtrader/issues/63). |
| July–August 2024, 0.32–0.33 | User-defined fields | Personal additional fields for instruments, followed by global additional fields, make GT extensible beyond its predefined master data. | [Release 0.32](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.32.0), [release-note migration](https://github.com/grafioschtrader/grafioschtrader/blob/V0.33.8/backend/grafioschtrader-server/src/main/resources/db/migration/V0_33_8__New_Release_Notes_in_database.sql). |
| {{< refactoring-period >}}From January 2025, continued in 2026{{< /refactoring-period >}} | Reusable base libraries | General functions such as users, roles, tenants, messages and tables are separated from the financial application. The frontend gains its `lib` area; the backend gains `grafiosch-base` and `grafiosch-server-base`. This separation allows reuse in other applications and continues alongside functional development. | [Backend refactoring #142](https://github.com/grafioschtrader/grafioschtrader/issues/142), [frontend refactoring #143](https://github.com/grafioschtrader/grafioschtrader/issues/143), [first commit containing the base modules](https://github.com/grafioschtrader/grafioschtrader/commit/9d14fe8665). |
| January–September 2026, expanded across several releases | GTNet and shared network infrastructure | GT instances can exchange intraday and historical prices and instrument configurations. Securities can be looked up on other instances and imported in batches. General network infrastructure also moves into the base libraries. The original data still comes from external providers. | [Intraday prices #82](https://github.com/grafioschtrader/grafioschtrader/issues/82), [historical prices #172](https://github.com/grafioschtrader/grafioschtrader/issues/172), [network refactoring #174](https://github.com/grafioschtrader/grafioschtrader/issues/174), [securities import #176](https://github.com/grafioschtrader/grafioschtrader/issues/176). |
| March 2026, 0.34 | Automation and configurable rules | Standing orders model recurring transactions. Generic price connectors and fee models make data acquisition and cost calculation more flexible. Debit interest on cash accounts and price updates after market close are added. | Release notes in GT; [release 0.34](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.34.0), [updates after market close #13](https://github.com/grafioschtrader/grafioschtrader/issues/13). |
| March 2026, 0.35 | Custody transfers and Swiss tax data | ISIN changes and securities transfers between custody accounts can be represented. Integration of the ICTax price list adds Swiss tax data and extends the dividends and interest report. | Release notes in GT; [milestone 0.35](https://github.com/grafioschtrader/grafioschtrader/milestone/35). |
| June 2026, 0.36 | Portfolio advice and read access | A portfolio adviser can manage several client tenants and switch between them. Clients or other viewers can receive read-only access. This builds on the earlier introduction of multi-tenancy. | Release notes in GT; [milestone 0.36](https://github.com/grafioschtrader/grafioschtrader/milestone/36). |
| July–August 2026, from 0.36.3 | Installation and quality assurance | Docker simplifies running a complete instance on a server or Raspberry Pi. An end-to-end test suite connects backend and browser tests. Dedicated test applications also exercise the base libraries independently of GT. | [Release 0.36.3](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.36.3), [release 0.36.5](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.36.5). |
| July–August 2026, 0.36.3–0.36.3.1 | Calendars, data exchange and shared texts | Editable rule sets complement index-based trading calendars. GT's own transaction receipts and CSV exports can be imported again. All interface texts are consolidated in the backend, also strengthening the independently usable libraries. | [Release 0.36.3](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.36.3), [release 0.36.3.1](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.36.3.1). |
| {{< refactoring-period >}}August 2026, 0.36.7.1–0.36.7.2{{< /refactoring-period >}} | Angular 22 and Optimus UI | Optimus UI first replaces PrimeNG, followed by Angular 22 and TypeScript 6. This platform change prepares the subsequent frontend modernization. | [Release 0.36.7.1](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.36.7.1), [release 0.36.7.2](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.36.7.2). |
| September 2026, 0.37 | Personal dashboard | A configurable start page of cards brings together winners and losers, recent trading-session results, messages and pending requests. | [Release 0.37](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.37.0). |
| October 2026, 0.37.3–0.38.1 | Reports, risk and monitoring | PDF reports, time-weighted and money-weighted returns, drawdown and volatility extend the evaluations. Alerts, historical simulations, estimated disposal costs and stored daily asset values with additional charts are added. | [Release 0.37.3](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.37.3), [release 0.38.1](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.38.1). |
| October 2026, 0.38.2–0.39 | Security lookup and AI integration | Data providers help fill in new securities. Additional fields can be filled through rules, Wikipedia or suggestions from a language model. An MCP connection lets AI assistants read authorized portfolio data. Fundamental equity values extend watchlist indicators. | [Release 0.38.2](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.38.2), [0.38.3](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.38.3), [0.39](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.39.0). |
| {{< refactoring-period >}}October–November 2026, 0.39.2{{< /refactoring-period >}} | Zoneless, signals and Signal Forms | After the August version upgrade, the frontend's programming model is substantially modernized. GT and the frontend library run without Zone.js; signals and OnPush drive view updates. Strict type checking and new dynamic Signal Forms complement the refactoring. | [Zoneless refactoring #233](https://github.com/grafioschtrader/grafioschtrader/issues/233), [modernization #288](https://github.com/grafioschtrader/grafioschtrader/issues/288), [release 0.39.2](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.39.2). |
| {{< mobile-period >}}October–November 2026, 0.39.2{{< /mobile-period >}} | Mobile web application | An initial mobile web application offers read-only access to dashboard widgets, portfolio reports and watchlists in an interface adapted to small screens. | [Release 0.39.2](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.39.2). |
| November 2026, 0.40 | Cost basis and realized gains | Moving average, FIFO and LIFO allow different assignments of sales to purchases. Annual realized-gain reports and German tax help extend the reporting. Additional fields can be shared with other users, and the watchlist gains a market map. | [Release 0.40](https://github.com/grafioschtrader/grafioschtrader/releases/tag/V0.40.0). |

## Sources and scope
The history before publication on GitHub is based on my recollection as the developer. Later developments were compiled from the Git history, selected GitHub releases and issues, the earlier static release texts and the release notes stored in GT. The selection covers releases through version 0.40.0 of 14 November 2026. Minor fixes, individual new connectors and routine version upgrades are largely omitted; the linked sources provide further detail.
