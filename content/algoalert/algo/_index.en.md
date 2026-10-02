---
title: "Rule based trading"
date: 2026-09-26T10:00:00+02:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.38.0" %}}
Allocation configuration, simulation environments and their historical replay are available. Evaluating future, simulated price paths is still under development.
{{% /notice %}}
Rule-based trading in GT is based on a hierarchical tree structure. At the root sits a **Portfolio based strategy**, which sets the share of your equity that may be invested at all. Below it, asset classes with percentage weightings are created, and within the asset classes the individual securities, again with a weighting. Strategies can additionally be attached on each of these three levels.

{{< mermaid >}}
graph TD
    A["Portfolio based strategy<br/>(Name, Maximum investment, Watchlist)"] --> B["Asset class<br/>(Weighting %)"]
    A --> C["Custom category<br/>(Weighting %)"]
    B --> D["Security of the asset class<br/>(Weighting %)"]
    B --> E["Security of the asset class<br/>(Weighting %)"]
    C --> H["Security from the watchlist<br/>(Weighting %)"]
    A --> S1["Strategy"]
    D --> F["Strategy"]
    H --> I["Strategy"]
{{< /mermaid >}}

## Creating a portfolio based strategy
You create a new strategy in the navigation tree via the context menu of **Rule-based Trading** with **Create Portfolio based strategy...**. The dialog contains the following fields:

| Field | Description |
|---|---|
| **Name** | Name of the strategy, at most 40 characters. |
| **Maximum investment** | The share of your net equity, between 2% and 100%, that the strategy may invest. All weightings below it divide exactly this amount. |
| **Watchlist** | The linked watchlist. It determines which asset classes are offered in the dialog and supplies the securities of a custom category. |

You can already give asset classes in this dialog. **Add asset class** appends another row with an asset class and its **Weighting** between 0.5% and 100%, and **Remove last asset class** takes the most recently entered row back. At most nine rows are possible. Only asset classes that occur in the selected watchlist are offered, and an asset class already chosen in another row cannot be chosen a second time. Further asset classes and all securities are added afterwards in the tree table.

There is no dialog for editing the portfolio based strategy afterwards. Its **Weighting**, which is the value of **Maximum investment**, can be changed directly in the tree table. Whether it is evaluated in the background is decided solely by its assignment to [portfolio monitoring](#portfolio-monitoring); the portfolio based strategy has no switch of its own to activate or deactivate it.

## The tree table
Clicking the strategy in the navigation tree opens the tree table **Rule-based Trading**. It shows the portfolio based strategy with its asset classes, securities and strategies. Asset classes are shown in bold.

| Column | Content |
|---|---|
| **Name** | For an asset class its category, subcategory and instrument type or the name of the custom category, for a security its name and currency. |
| **Weighting** | The share of the parent node. The value can be edited directly in the cell. |
| **Sum of weightings** | Adds up the weightings of the nodes directly below, so you see at once whether a level reaches 100%. |
| **Trading from** and **Active until date** | The trading period of a security. |

The table highlights flaws of the allocation in colour. A **Sum of weightings** that does not reach 100% is shown in red. Also shown in red are the name of an asset class that carries a positive weighting but contains no active, positively weighted and tradable security, and the name of a security that is unsuitable for a strategy. An **Active until date** in the past is highlighted in yellow. The hint below the table summarises these rules.

### Readiness of the strategy
Below the table GT shows whether the strategy can be used as it stands. **Ready for simulation and replay.** means that a simulation environment can be created and a replay started. If a portfolio rebalance exists as well, the rebalancing comparison can also be computed; without one GT reports **Without a portfolio rebalance there is no rebalancing comparison.** The individual findings follow below.

Readiness is determined anew each time it is shown and is never stored. While you build a strategy, its weightings necessarily do not yet add up to 100%; findings can also change without you editing the strategy, for instance when a security's end of trading passes or the linked watchlist loses a security.

Findings listed in red prevent use. As long as one of them remains, GT refuses **Create simulation environment...**, **Start replay...** and the rebalancing comparison, naming the reason:
- The weightings of the asset classes, or those of the securities of one asset class, do not add up to 100%.
- A weighting is missing or lies outside 0% to 100%, or **Maximum investment** lies outside that range.
- The portfolio rebalance is incomplete, or the security band or the maximum number of traded securities of an asset class is invalid.
- An active Mean Reversion Dip cannot be executed, or its security is not part of the linked watchlist.
- A security with an active Mean Reversion Dip is assigned to more than one asset class.

The other findings are only reported, because GT can handle them. A security unsuitable for strategies is not traded in a replay, and its weighting is redistributed to the other securities of its asset class. The share of an asset class without a tradable security stays uninvested. A security without a Mean Reversion Dip in two asset classes and an empty asset class are also only shown.

If a sum drifts because you changed individual weightings by hand, **Normalize percentages** scales the values of the nodes directly below the selected node back to 100%; any rounding difference is carried by the last node. **Normalize all percentages** on the portfolio based strategy does so for the asset classes and for the securities of each asset class.

Security rows carry a checkbox. When securities are ticked, **Delete selected securities** appears at the top of the context menu, which removes several securities in one step. Depending on the node, the context menu offers these entries:

| Node | Context menu entries |
|---|---|
| Portfolio based strategy | **Add Asset class**, **Normalize percentages**, **Normalize all percentages**, **Create Strategy definition** |
| Asset class | **Edit Asset class**, **Delete Asset class**, **Add Security**, **Normalize percentages**, **Create Strategy definition** |
| Security | **Edit Security**, **Delete Security**, **Create Strategy definition** |

An asset class can only be deleted once it contains neither securities nor strategies. **Create Strategy definition** is greyed out when no further strategy is available for the node. Inside a simulation environment the tree table is read only; it offers neither a context menu nor editable cells there.

## Adding and editing an asset class
**Add Asset class** and **Edit Asset class** open the dialog **Strategy asset class**. An asset class of the tree is either an asset class of GT or a freely named group, the **Custom category**.

| Field | Description |
|---|---|
| **Custom category** | When ticked, a freely named group is created instead of an asset class of GT. This choice is only possible when creating. |
| **Name** | Name of the custom category, at most 40 characters. Only editable, and then required, while **Custom category** is ticked. |
| **Asset class** | The asset class of GT, shown as category, subcategory and instrument type. Only editable, and then required, without **Custom category**. It cannot be changed when editing. |
| **Security allocation band (percentage points)** | Optional, 0 to 100. Overrides the setting of the same name of the portfolio rebalance for this asset class. |
| **Maximum traded securities per asset class** | Optional, from 1. Overrides the setting of the same name of the portfolio rebalance for this asset class. |
| **Primary securities account** | Optional. The securities account in which a simulation books the trades of this node in the first place. All securities accounts of your portfolios are offered. |
| **Secondary securities account** | Optional. The securities account used when the primary one cannot execute a trade. It can only be chosen once a primary securities account is selected. |
| **Weighting** | Required, 0.1% to 100%. The share of this asset class in the amount released by **Maximum investment**. |

Each asset class of GT can occur only once in a strategy; asset classes already in use and those of CFDs are therefore not offered. Custom categories are not subject to this restriction. Choosing an asset class of GT or a custom category determines which securities are offered later: an asset class of GT offers its securities, a custom category the securities of the strategy's watchlist.

The two fields for the band and the number of traded securities normally stay empty; the setting of the [Portfolio rebalance](../strategy/rebalancing/) then applies. The security band refers in percentage points to the target amount of the asset class, the trade limit counts distinct securities per checkpoint.

## Adding and editing a security
**Add Security** and **Edit Security** on an asset class open the dialog **Strategy security**.

| Field | Description |
|---|---|
| **Security** | Required. A dropdown with a search field. Each entry shows name and currency as well as the trading period from **Trading from** to **Active until date**. |
| **Primary securities account** and **Secondary securities account** | Optional, with the same meaning as for the asset class. |
| **Weighting** | Required, 0.1% to 100%. The share of this security in the amount of its asset class. |

Which securities are offered depends on the parent asset class. Below an asset class of GT, all securities of that asset class visible to you appear, regardless of any watchlist. Below a custom category, the securities of the watchlist linked to the portfolio based strategy appear. In both cases securities already placed below the same asset class are left out.

CFDs, Forex and instruments with a leverage factor other than 1 are neither offered nor saved. If the strategy was created from historical holdings, the day after its reference date counts as the opening date; a security whose trading ended before it cannot be added either. Securities whose trading starts only after that reference date remain selectable. In the dropdown a red date marks a trading start after the strategy reference date or a trading end before today.

When editing, the security itself, the securities accounts and the weighting can all be changed.

{{% notice style="info" title="A security belongs in exactly one asset class" %}}
As each asset class of GT occurs only once, a security appears there at most once. The same security can, however, additionally appear in any number of custom categories, because GT does not check this when saving. Strategies on the level of a security, such as the [Mean Reversion Dip](../strategy/meanreversiondip/), require the instrument to belong to exactly one asset class. Therefore assign each security only once.
{{% /notice %}}

## Assigning strategies
Strategies can be assigned on all three levels: the portfolio based strategy itself, an asset class and a security. **Create Strategy definition** in the context menu opens the dialog **Strategy definition**, in which you select the desired strategy type under **Strategy name**. Only the types permitted on that level are offered. **Portfolio rebalance** is available on the portfolio based strategy only, because the weightings of the tree already state the targets below it. Detailed information about the individual strategy types can be found under [Strategy](../strategy/).

## Portfolio monitoring
Only the portfolio based strategy you have assigned to portfolio monitoring is evaluated in the background: its portfolio rebalance as well as every alert and strategy below it. You assign it in the navigation tree through the context menu of the strategy with **Use for portfolio monitoring** and remove the assignment with **Stop portfolio monitoring**. At most one strategy is assigned; in the navigation tree it carries an eye and the suffix "Portfolio monitoring". A strategy without this assignment rests in the background and keeps its whole configuration, so you can build it at your own pace.

Simulations and historical replays do not depend on this assignment. They evaluate every portfolio based strategy as it is configured.

Asset classes and securities have no switch of their own in the tree table. You pause an individual security or an individual strategy with the **Active** checkbox in the **Alert** tab of the client, see [Alerts](../alert/).

## Create an allocation from a watchlist
Choose **Create strategy from watchlist** in the context menu of **Rule-based Trading** and enter a **Name** and a **Watchlist**. GT creates one asset class for each asset class of the instruments in the watchlist and places every instrument below its asset class. As a watchlist carries no amounts, both levels are weighted equally: the asset classes among themselves and the instruments within their asset class. **Maximum investment** is set to 100%, and you adjust it together with the weightings afterwards. Currency pairs of the watchlist are not taken over; every instrument needs an asset class, and the watchlist must contain at least one instrument.

## Create an allocation from historical holdings
Choose **Create strategy from portfolio** in the context menu of **Rule-based Trading** and enter a **Name** and a **Reference date**. The date must be yesterday or earlier; the first day with a transaction is eligible. GT includes transactions through the end of that day and uses historical closing prices and exchange rates available on or before it.

The allocation is built from the holdings of that day alone; a watchlist is neither needed nor linked. Missing prices or exchange rates prevent creation and are listed in the error. Every held security needs an asset class. GT creates the complete allocation together. Because no watchlist is linked, custom categories of such a strategy offer no securities, and the [Mean Reversion Dip](../strategy/meanreversiondip/) finds no eligible instrument.

**Maximum investment** is the gross exposure divided by net portfolio equity. Gross exposure adds the absolute exposure of long and short positions before offsetting them. Equity includes signed position values, cash and liabilities; margin positions contribute their closing settlement value. An allocation requires positive equity and exposure, and exposure cannot exceed equity.

For equity of 100,000 and exposure of 48,200, **Maximum investment** is 48.2%. An asset class with 50% **Weighting** receives 24,100 of that budget. A security weighted at 60% within that class receives 14,460. The generated weights follow the exposures on the reference date and sum to 100% among asset classes and separately among the securities of each class. Cash remains in the cash accounts.

## Create a simulation environment
Choose **Create simulation environment...** from the context menu of a portfolio based strategy. A simulation environment is a separated copy of your portfolios and accounts as of a day in the past, in which you can examine your strategies against past price data without touching your real portfolio.

### Prerequisites
All it takes is a portfolio based strategy. Asset classes and securities need neither exist nor be fully weighted at this point; the environment is created even for a strategy that is still empty. Whether it then evaluates anything is a different question, answered by the last section of this page.

The two initialization modes that read your existing portfolio additionally need an opening date your portfolio actually reached: it must fall on or after the first transaction, and the day of that first transaction is itself eligible. Before that day there are neither positions nor cash to read, and the environment would open empty without your asking. **Manual cash balances** takes no positions or cash from the portfolio and is therefore not bound by this lower limit. By default up to five environments may exist at the same time; the administrator can change that limit.

### The fields of the dialog
| Field | Meaning |
|---|---|
| **Name** | Name of the environment, at most 40 characters; pre-filled with the name of the strategy. |
| **Opening date** | The completed trading day whose closing state the environment opens with. Yesterday is the latest date allowed. |
| **Initialization mode** | Determines what that opening state is built from. |
| Amount per cash account | Appears only for **Manual cash balances**, with one field for every cash account of your portfolios. An account whose field stays empty is not taken over into the environment; enter 0 to take over an account without money. At least one field must be filled. |

### What the opening date stands for
The opening date is more than a label, because it settles four things at once. It is the boundary of what is taken over: transactions through the end of that day are copied, later ones are not. It is the booking date of the opening transactions through which cash enters the environment. When the source portfolio is liquidated it is also the valuation day, because positions are valued at that day's closing prices and exchange rates. And it is the immovable beginning of every historical replay: evaluation starts on the first trading day after it, which is why a replay later asks only for an end date.

After creation the date can no longer be changed, because the opening state was booked from it already; a date moved afterwards would no longer describe the existing bookings. For a different starting point you therefore create a further environment.

### Choosing the initialization mode
{{< mermaid >}}
graph TD
    A["What should the environment open with?"] --> B["Carry on with the<br/>existing positions"]
    A --> C["Start with cash only"]
    B --> D["Take over source portfolio"]
    C --> E["Enter the amounts yourself"]
    C --> F["Use the value of the positions<br/>on the opening date"]
    E --> G["Manual cash balances"]
    F --> H["Liquidate source portfolio to cash"]
{{< /mermaid >}}

| Initialization mode | Opening state |
|---|---|
| **Take over source portfolio** | Copies transactions through the opening date, including transfers and their internal links, and rebuilds positions and cash balances. Later transactions are excluded. |
| **Manual cash balances** | Takes over only the cash accounts you entered an amount for, the portfolios they belong to and the securities accounts of those portfolios. A portfolio without a chosen cash account is left out entirely. Every positive amount becomes an opening deposit in the account's currency; an amount of 0 produces no transaction. Source trades are excluded. |
| **Liquidate source portfolio to cash** | Values positions at the historical close and adds their signed closing proceeds to the source cash balances. Creates only opening deposits or withdrawals, with no positions or hypothetical sale transactions, fees or taxes. |

For **Liquidate source portfolio to cash**, save once to check the calculated opening balances by **Cash account**, **Currency** and **Balance**. Proceeds retain their source cash account wherever attribution is clear. Otherwise, choose a destination for each listed position under **Select cash account** and save again to refresh the preview. A further **Save** of the resolved preview creates the environment. A line in the preview showing nothing but an instrument name or a currency pair means that no usable closing price exists for that date; it has to be supplied first. Closing a short or margin position can reduce cash, and a negative opening balance becomes a withdrawal.

### After the creation
The environment copies portfolios, accounts and the strategy's watchlist. The strategy itself is shared, but read only inside an environment: change it in your own portfolio and replay the simulation afterwards. Transactions and balances, in contrast, belong to the simulation alone. Use **Switch to simulation** to enter it and **Switch to main tenant** to return. The navigation tree lists each environment with its opening date and its initialization mode. You start a historical replay from the context menu of the environment with **Start replay...**.

**Delete simulation** removes its private data and pending work while preserving the shared strategy and main portfolio. Older environments without a known opening date remain available for inspection and deletion; the navigation tree marks them **Opening definition missing, create again**, because their opening state can no longer be reconstructed. A historical replay is no longer possible for them.

### What a simulation actually evaluates
Creating an environment does not by itself place trading orders; these arise only with a [Historical replay](../historicalrun/). A replay over an empty strategy ends as **Completed** and without a single trade. For trading to happen the hierarchy has to be complete.

**Portfolio rebalance** trades on the level of the securities only. An asset class without assigned securities therefore receives a target amount, but nothing is bought. In addition the weightings of the asset classes and, separately, those of the securities within each asset class must add up to 100%. Where they do not, the strategy is not ready: neither can an environment be created nor a replay started, see [Readiness of the strategy](#readiness-of-the-strategy).

Strategies on the level of a security additionally require the instrument to belong to exactly one asset class. Otherwise the course of the replay shows the event **Unavailable** with the reason **Complete the active allocation and its weights** or **Instrument has more than one allocation**.

CFD, Forex and leveraged positions that enter the environment with the opening state are closed after the opening and excluded from further simulation trading.

Independently of that, the available price data limits what can be evaluated at all. The opening date refers to your portfolio and not to the instruments, so it may lie before the first price of an instrument. Before the price data begins a replay decides nothing, and days whose closing state cannot be valued completely are marked **Unavailable** in the course of the run and left out of the figures. When you create an environment the dialog tells you from when the instruments of the strategy carry price data.

{{% notice style="info" title="Note" %}}
A historical replay evaluates the **Portfolio rebalance** and the [Mean Reversion Dip](../strategy/meanreversiondip/) independently of [portfolio monitoring](#portfolio-monitoring) and books their purchases and sales. Price and indicator alerts are never evaluated in a simulation, see [Where each strategy takes effect](../#where-each-strategy-takes-effect).
{{% /notice %}}
