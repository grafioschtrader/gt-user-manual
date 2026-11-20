---
title: "Portfolio"
date: 2026-11-10T18:00:00+01:00
draft: false
weight: 5
archetype: "default"
---
In most cases, the portfolio is a representation of a single trading platform. Several **bank accounts** and **securities accounts** can be created in a **portfolio**.
+ A **portfolio** is assigned a **currency**, which may differ from the **client**'s **currency**.
+ A portfolio cannot be deleted as long as it still has a securities account or a bank account.

## Creating and editing a portfolio
A portfolio is created, edited and deleted via the **navigation area**.
+ **Create** a **portfolio** via the **context menu** on the **static element** Portfolios.
+ **Edit** a **portfolio** via the **context menu** on the corresponding **element** of the portfolio.
+ **Delete** a **portfolio** via the **context menu** on the corresponding **element** of the portfolio.

### Properties
The **properties** of the portfolio can be changed completely at any time.
- **Portfolio or bank name**: The **name** of the portfolio; this name must be unique for a client.
- **Currency**: The evaluations on this **portfolio** refer to this **currency**. If it is changed, GT creates the [currency pairs](../../watchlistinstrument/instrument/currencypair/) missing for the new portfolio currency and rebuilds the holding tables. Both happen in a background task, so the evaluations are only complete once that task has finished.
- **Closed until**: Transactions dated on or before this date can no longer be added, changed or deleted. A date set here takes precedence over the corresponding date of the client. A detailed description including the exceptions can be found under ["Closed until" in Client](../client/#properties).
- **Cost basis method**: By default this is **Same as client**; the portfolio then uses the method of the [client](../client/#properties) and follows every later change there. If you choose **Moving average**, **FIFO (oldest purchases first)** or **LIFO (newest purchases first)** instead, this method applies to all security accounts of this portfolio, regardless of the setting of the client. This makes sense when a trading platform keeps its statements by a different method. Here too a change takes effect immediately and retroactively. In evaluations over several portfolios it can also shift the gain of sales in other portfolios that use the moving average, see [Cost basis method](../../reportportfolio/costbasismethod/#scope).
- **Withholding tax**: This field appears only for a client with the main currency euro and the country Germany. It tells the [German tax help](../../reportportfolio/germantax/) whether a domestic paying agent withholds the tax on the income of the accounts of this portfolio. Existing portfolios are set to **Not assigned**: their income then appears in no block of the tax return until you choose **A domestic paying agent withholds the tax** or **No domestic withholding tax**. The tax help assumes that a portfolio holds the accounts of exactly one institution. Put accounts of different banks in separate portfolios.
