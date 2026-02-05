---
title: "Introduction"
date: 2026-01-21T15:00:00+01:00
draft: false
weight: 1
archetype: "default"
---
A VIDEO will follow ...

## Why exists Grafioschtrader (GT)
For a few years I used a portfolio management application that met my initial needs. Over time, I realized that transaction and custody costs have a significant impact on the overall return of the portfolio. So I looked for different online stockbrokers that offered the most favorable trading conditions for my investment. So far, only a combination of several trading platforms has met my needs. As a Swiss investor, there is also the fact that sooner or later investments are made in foreign currencies.

Over time, you lose track of how much is invested in what. I thought this didn't have to be the case and, as a software developer, I plunged into the Grafioschtrader adventure. With **GT** I now have an overview of the **positions held**, **performance** and **account balances** per portfolio or cumulated across all portfolios.

## What can GT
+ **The software is free and open source**: GT is released as open source and can be found on [GitHub](//github.com/grafioschtrader).
+ **Multi-tenancy**: GT can be run for a group of investors or in single mode.
+ **Web application**: GT is a web application and provides the clearest results using a desktop web browser.
+ **Multiple portfolios with currency accounts**: Replicates multiple portfolios with one or more securities accounts and one or more bank cash accounts.
+ **Multiple currencies**: Trading securities in different currencies
+ **Trading from the turn of the millennium**: Basic support for historical price data from the year 2000 onwards, noting that obtaining price data from non-traded securities may be a problem.
+ **Different financial instruments**: Stocks, Bonds, ETF, securities without price data, short ETF, CFD, Forex.
+ **Import of transactions**: An import of single or multiple PDFs with securities transactions Via CSV file, account transactions can also be loaded.
+ **Evaluations by asset classes**: Evaluations by common asset classes such as stocks, bonds, real estate, commodities, etc.

## For whom is suitable GT
GT is an attempt to **replicate portfolios** according to **reality**, so there are accounts just like with your online stockbroker. **Each booking** is linked to a **cash account**, so the bookings are replicated according to your trading platform.
+ The investor who does not want to entrust his financial data to a non-transparent platform. You can **host GT yourself**, all you need is a [Raspberry Pi 4](//www.raspberrypi.org/products/raspberry-pi-4-model-b/) with 4GB (possibly 2GB) of RAM.
+ A shareholder who wants to have a **history** of all his **trades**. You can view all closed positions at any time.
+ Private investors who want to know how their **currency profits** are developing. The currency gains on the foreign currency accounts are shown on an ongoing basis.
+ **Investors' club** who want to **host** their own GT instance and would like a member to take care of the technical aspects.
+ Investors with **several portfolios** on different trading platforms. GT provides information per portfolio and can display it in aggregated form.

## For whom GT is not made
Before you waste your lifetime unnecessarily with GT, you should consider the following:
+ Day traders lack **real-time quotes**.
+ Investors who want to manage their trading independently of cash accounts.
+ Investors who only have one portfolio with an online broker and are satisfied with the analyses provided there.
+ There are investors who create portfolios according to certain criteria, such as a dividend portfolio. GT does not support such an approach. On the contrary, the aggregated evaluation of real portfolios on different trading platforms is the goal.
+ In GT there is no support for **alleged real public portfolios** of users. The developers of GT cannot make sense of these possible fake portfolios. Most importantly, a portfolio is individual and hopefully strongly influenced by the circumstances of the respective investor. Reducing this to a few numbers and graphs is nothing more than **disinformation**.

## For whom GT does not work
GT is definitely not a jack of all trades:
+ For investors who want to manage transactions with GT **before** the **year 2000**. GT consistently implements double-digit annual figures.
+ In GT, the combination of **ISIN** and **currency** of an instrument is **unique**. It is therefore not possible to enter the same ETF in EUR for the Frankfurt and Stuttgart stock exchane, for example.
+ For investors who want to assign an instrument to a **sector/topic** and **country/region** at the same time.
+ For investors from Switzerland, Germany or the USA who require a **security number**, **securities identification number (WKN)** or **CUSIP number**. GT uses the International Securities Identification Number **(ISIN)** and the ticker/symbols, which are not always unique.

{{% notice info %}}
The information in this help refers to **GT** version **0.33.16**.
{{% /notice %}}