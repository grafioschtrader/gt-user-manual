---
title: "Alert"
date: 2026-09-30T12:00:00+02:00
draft: false
weight: 30
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.38.0" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
The alert system in GT informs the user when configured conditions on securities are met. Alerts can be created in two contexts: as standalone security alerts or as alerts within a rule-based strategy. In both cases the user decides about any transaction, because GT never trades on its own.

## Standalone Security Alerts
Standalone alerts are created directly on a security without requiring a rule-based strategy. Via the context menu of a security in a watchlist, in the view of a security account or in the reports [Security accounts](../../reportportfolio/securityaccountreport/) and [Asset Classes with Cash](../../reportportfolio/securitycashaccountreport/) choose **Add alert...**, whereupon the **Strategy definition** dialog opens. GT creates the corresponding **Strategy security** entry, which links the security with its alerts, by itself; if you cancel the dialog without entering a strategy, nothing is left behind. In a simulation environment and for a user with read-only access the entry is missing. These alerts are evaluated independently of a rule-based strategy and are suitable for simple monitoring of individual securities.

## Alerts Within a Rule-based Strategy
Within a **Portfolio based strategy**, strategies can be assigned on the levels portfolio based strategy, asset class and security, see [Rule based trading](../algo/). When the condition of such a strategy is met, the system generates an alert as well. A strategy on the portfolio based strategy is evaluated for the securities of its watchlist, a strategy on an asset class for the securities of that asset class. These alerts are evaluated continuously only for the portfolio based strategy assigned to [portfolio monitoring](../algo/#portfolio-monitoring); a [historical run](../historicalrun/), in contrast, evaluates any portfolio based strategy. Each alert has its own switch **Alerts enabled**, which you set in the tree table of the portfolio based strategy. An alert whose checkbox **Active** is not set in the **Strategy definition** dialog is a draft and is never evaluated. A portfolio rebalance on the portfolio strategy delivers its recommendations as messages as well; when these arise is described under [Portfolio Rebalancing](../strategy/rebalancing/). If the allocation of the monitored strategy leaves its tolerance between two checkpoints, GT reports this once as **Allocation outside its tolerance**, see [Allocation outside its tolerance](../strategy/rebalancing/#allocation-outside-its-tolerance).

## Alert Types
Six alert types are available for securities. The letters in brackets after a field name are the short designations used in GT.

| Alert type | Available levels | Fields and measured condition |
|---|---|---|
| **Absolute price gain/lose** | Security | **Lower limit (L)** and **Upper limit (U)** from 0. Both bounds can be used on their own; an alert is raised as soon as the price crosses one of them. |
| **Holdings gain/lose** | Portfolio based strategy, Asset class, Security | **Gain (G)** and **Lose (L)** as a percentage from 1 to 500 as well as the price limits **Upper limit (U)** and **Lower limit (L)** from 0; at least one field must be filled in. What is measured is the gain or loss of the position actually held, relative to its cost basis. Without an open position no evaluation takes place. |
| **Gain/loss in a period** | Security | **Period (P)** from 1 to 999 days plus **Gain (G)** and **Lose (L)** from 1 to 500 percent. The comparison uses the last closing price on or before the day the period points back to. |
| **Moving average crossing** | Security | **Indicator type (I)** with the choice **Simple Moving Average** or **Exponential Moving Average**, **Period (P)** from 1 to 999 and **Cross direction (C)** with the choice **Above** or **Below**. An alert is raised when the price crosses the moving average in the chosen direction. |
| **RSI threshold** | Security | **RSI period (R)** from 1 to 999 plus **Lower threshold (L)** and **Upper threshold (U)** from 0 to 100. |
| **Custom expression** | Security | **Expression (E)** with at most 500 characters. The values `price`, `prevClose`, `open`, `high`, `low` and `volume` are available, as are the functions `SMA(n)`, `EMA(n)` and `RSI(n)`. The alert is raised when the expression holds. |

The same alert type may be entered more than once on a security, for example two absolute price bounds with different values. Only the portfolio rebalancing and the Mean Reversion Dip are limited to one per level.

## Alert Overview
The overview of the standalone alerts of a tenant opens with the node **Rule-based Trading** in the navigation tree. It is built as a tree table: the top level holds the security, below it as child nodes the alerts entered, with their alert type in the column **Strategy name**. Alerts that belong to a portfolio based strategy do not appear here; they are managed in the tree table of their strategy.

The column **Alerts enabled** contains a checkbox on the row of an alert; the row of the security has none. A change is saved immediately. An alert can thus be suspended temporarily without losing its configuration. When it is switched on again, GT establishes a new starting observation, so that a movement during the pause is not reported as a crossing.

Through the context menu of the security row you enter a further alert with **Create Strategy definition**, or remove the security together with all its alerts with **Delete Strategy security**. On the row of an alert, **Edit Strategy definition** and **Delete Strategy definition** are available. When editing, only the parameters can be changed, not the alert type.

## Alert Evaluation
All six security alert types are checked in the background. The administrator sets `gt.algo.alarm.evaluation.interval.hours` in the global settings to a whole number from 2 to 6 hours; the default is 4 hours. Changes take effect without restarting. A recent successful evaluation or background attempt postpones the next background attempt. New intraday observations can trigger an earlier evaluation. The scheduler checks for due work every five minutes, so evaluation can start up to five minutes later, plus any waiting time in the task queue. The actual work is done by [background task 50]({{% relref "/admindata/taskdatachangemonitor/taskdescription/#JOB50" %}}).
Ordinary securities are checked during their exchange's opening hours, using the exchange's time zone and recorded holidays. No automatic checks take place on weekends in that time zone. Crypto-classified securities are checked throughout the week. This exception applies to securities already supported by the alert system; it does not add alerts for cryptocurrency currency-pair entries.
After the exchange closes and the data provider's delay has elapsed, GT makes one final attempt if the closing observation has not already been evaluated. This attempt can occur sooner than the configured interval. A failed closing attempt is not repeated or caught up over the weekend.
GT reuses a sufficiently recent quote from the current trading session or requests a new one. Several alerts for the same security share the downloaded quote. If the quote remains too old, or required history or exchange information is missing, no alarm is generated. GT records the reason and preserves the previous valid crossing observation. A failed background attempt waits for the configured interval before another attempt.
Price, moving-average and RSI crossings need a valid starting observation first. The first valid observation establishes it and raises no alert yet. Editing or reactivating an alert establishes a new starting observation, so that a period of deactivation cannot create an apparent crossing. A movement across a threshold and back between two observations can remain undetected.
The following flow shows the path from the due check to the message.
{{< mermaid >}}
flowchart TD
    S([Every five minutes: check for due alerts]) --> D{Alert due?}
    D -->|No| W[Wait until the next interval]
    D -->|Yes| B{Exchange open or cryptocurrency?}
    B -->|No| W
    B -->|Yes| Q[Obtain the quote once per security]
    Q --> F{Quote recent enough and data complete?}
    F -->|No| R[Record the reason, keep the comparison value, no alert]
    F -->|Yes| E{Condition met?}
    E -->|No| W
    E -->|Yes| A{Already reported today?}
    A -->|Yes| N[No second message]
    A -->|No| M[Record the alert and deliver it as a GT message]
{{< /mermaid >}}

## Alert Lifecycle
When an alert condition is met, the system records the alert with the details of the triggered condition. The user is then notified via GT's internal messaging system, in the language of the client. The subject names the context of the alert, the message body the security and the details of the trigger. The user can read the message, assess the situation and then independently decide on the desired action. GT does not execute automatic transactions. If a message could not be delivered, it is sent again later from the recorded alert, without the alert being raised a second time.

## Deduplication
So that the same situation is not reported repeatedly, GT distinguishes an alert by client, alert, security, kind of signal, direction and day. Repeated evaluations of the same signal on the same day therefore result in a single message. Two securities never suppress one another, and a crossing upwards and one downwards on the same day are two separate messages.

## Alert diagnostics
The button **Alert diagnostics** above the alert overview opens a dialog that shows why an alert fired or why it did not. Its three tabs answer three consecutive questions. A note above the tabs points out that an e-mail counts as delivered externally once the mail server has accepted it; GT cannot tell whether it arrived in the inbox. Every table shows 20 rows per page; for longer lists you change the page below the table. The filter row below the column headers restricts individual columns to particular values. If you select several values in one column, the rows with any of them are shown.
{{< mermaid >}}
flowchart LR
    A[Evaluation: Could the rule be evaluated?] --> H[Trading decisions: What did the Mean Reversion Dip decide?]
    H --> B[Notifications: Did the message arrive?]
{{< /mermaid >}}
At the bottom of the dialog the question mark on the left leads to this page. On the right, **Refresh** reloads the tables without calculating anything. **Evaluate now** checks your own alerts immediately and additionally recalculates the portfolio rebalancing and the Mean Reversion Dip. Unlike the background schedule, this evaluation ignores the opening hours of the stock exchange. Every immediate evaluation and every notification sent again counts towards the limit **Manual alert actions**, see [limits of the information classes]({{% relref "/admindata/entitylimit" %}}). A user with read-only access does not see **Evaluate now** and the context menu with **Retry selected notification**. Alert diagnostics are not available in a simulation environment, because alerts only watch your own portfolio.

### Evaluation
The tab lists every combination of alert and security with the columns **Context**, **Security name**, **Strategy type**, **Active**, **Evaluation outcome**, **Last attempt**, **Last success**, **Quote timestamp** and **Reason**. It covers the six alert types; the portfolio rebalancing and the Mean Reversion Dip do not appear here. **Last attempt** and **Last success** are kept apart so that a failed attempt does not look like a successful check. **Quote timestamp** is the time of the price on which the last successful evaluation was based. The table is sorted by **Context**; it can be filtered by **Context**, **Strategy type** and **Evaluation outcome**.

| Evaluation outcome | Meaning |
|---|---|
| **Never evaluated** | The alert has not been checked yet, for example because it is new or its stock exchange has not been open since. |
| **Running** | The check is in progress. |
| **Evaluated** | The condition was checked completely. Whether an alert resulted is shown in the tab **Notifications**. |
| **Partially evaluated** | Part of the condition could be checked, another part could not. The **Reason** names the missing part. |
| **Unavailable** | The rule could not be evaluated and no alert was raised. The **Reason** names the cause. |

The following reasons appear with **Unavailable** or **Partially evaluated**:

| Reason | Meaning and remedy |
|---|---|
| **Required quote is missing or stale** | The last price is older than the configured evaluation interval or belongs to an earlier trading session. After **Evaluate now** at a weekend, on a holiday or outside trading hours this is the normal case and not an error; the next check during trading hours catches up. If the message persists during trading hours, check the data provider of the security. |
| **Quote refresh failed** | The data provider did not deliver a new price. Check the settings for the current price of the security. |
| **Instrument has no stock exchange** | No stock exchange is assigned to the security, so GT does not know its trading hours. |
| **Trading hours of the stock exchange are missing or invalid** | The opening or closing time of the stock exchange is missing, or both are equal. |
| **Time zone of the stock exchange is invalid** | The time zone stored with the stock exchange is unknown. |
| **Holding percentage unavailable: cost basis is zero** | Only for **Holdings gain/lose** with **Gain (G)** or **Lose (L)**: without a cost basis no percentage can be calculated. The price limits **Upper limit (U)** and **Lower limit (L)** are still checked, which is why the outcome is **Partially evaluated**. |
| **Evaluation failed** | An unexpected error prevented the check. If a technical text appears in the column instead, it is the unchanged error message. |

Outside trading hours GT makes no background attempt; the row then keeps the result of the last check.

### Trading decisions
The tab shows the current proposal of every [Mean Reversion Dip](../strategy/meanreversiondip/) of the portfolio based strategy that is assigned to [portfolio monitoring](../algo/#portfolio-monitoring). Other strategies, including the portfolio rebalancing, do not appear here. If no portfolio based strategy is assigned to monitoring, or it contains no Mean Reversion Dip, the table stays empty.
The Mean Reversion Dip is evaluated once a day on the last completed daily closing price, and immediately with **Evaluate now**. There is exactly one row per strategy and security, which each evaluation replaces; earlier decisions cannot be viewed here. The columns are **Context**, **Strategy name**, **Security name**, **Valuation date**, **Action**, **Units**, **Amount**, **Currency**, **Price/Div/etc.** and **Reason**. **Price/Div/etc.** is the closing price on the valuation date, **Amount** is in the currency of the client. The latest **Valuation date** comes first; the table can be filtered by **Context**, **Strategy name** and **Action**.

| Action | Meaning |
|---|---|
| **Buy** | The strategy proposes buying the given number of units. Closing or reducing a short position also appears as a buy, because the direction of the order is what counts. |
| **Sell** | The strategy proposes a sale, for example taking profit, a stop-loss or, for short positions, an entry. |
| **Hold** | Nothing needs to be done, for example because the entry condition is not met or the open position is kept. |
| **Blocked** | An action would be due but is prevented, or the strategy could not be evaluated. The **Reason** names the cause. |

Only **Buy** and **Sell** raise an alert and therefore a notification. **Hold** and **Blocked** are visible in this tab only. Anyone wondering why no message arrived finds the answer here. What the individual reasons mean and what you can do about them is described in [Review the result](../strategy/meanreversiondip/#review-the-result). GT never books a transaction itself; **Units** and **Amount** are proposals.

### Notifications
The tab lists all recorded alerts with the state of their delivery, the latest **Signal time** first. It can be filtered by **Context**, **Delivery status** and **Delivery channels**; this way you find, for example, all notifications that require a review. The columns are **Context**, **Security name**, **Signal time**, **Delivery status**, **Delivery channels**, **Delivery attempts**, **Next attempt**, **Internal delivery**, **SMTP acceptance**, **Delivery error** and **Signal details**. **Internal delivery** is the time the GT message was stored, **SMTP acceptance** the time the mail server accepted the e-mail.

| Delivery status | Meaning |
|---|---|
| **Pending** | The message is waiting for delivery. |
| **Sending** | Delivery is in progress. |
| **Retry scheduled** | An attempt failed. GT tries again after 5, 15 and 60 minutes and every 6 hours afterwards; **Next attempt** shows when. |
| **Failed; review required** | After eight failed attempts GT gives up. The cause is shown in **Delivery error**. |
| **Review required** | The recipient has changed since the alert fired, or an older notification remained unresolved. GT does not send it on its own. |
| **Delivered** | All delivery channels are done. |
| **Cancelled** | The strategy was deleted or is no longer monitored; the message will not be delivered. |

You mark rows of the table with their check boxes. When exactly one notification with **Failed; review required** or **Review required** is marked, send it again with the entry **Retry selected notification** in the context menu of the right mouse button. For any other delivery status, and while no row or several rows are marked, the entry is greyed out. A channel that was already completed is not repeated. After a server restart, however, a retry may send an e-mail twice.

Notifications you no longer need are deleted with the entry **Delete selected** in the same context menu; after a confirmation all marked rows are removed. A notification can only be deleted once its delivery has finished, that is with **Delivered**, **Cancelled**, **Failed; review required** and **Review required**, and once it is older than 10 days. Until then it prevents the same alert from being sent again. If even one marked row cannot be deleted yet, the entry is greyed out. GT also deletes old notifications by itself; how long they are kept is set by the administrator, see [Background tasks]({{% relref "/admindata/taskdatachangemonitor/taskdescription/#JOB50" %}}).

## Limitations
The alert type of an existing alert can no longer be changed; instead a new alert is entered and the old one deleted. Currency pairs of a watchlist cannot carry an alert, alerts are reserved for securities. Besides the background schedule described above, **Evaluate now** in [Alert diagnostics](#alert-diagnostics) starts an immediate evaluation of your own alerts. The number of portfolio based strategies, asset classes, securities and strategies per client is bounded by the [limits of the information classes]({{% relref "/admindata/entitylimit" %}}). If the installation has switched the alert function off, the entry **Add alert...** is missing and no alert is evaluated. If rule-based trading is switched off as well, the node **Rule-based Trading** is missing too. Standalone security alerts need only the alert function; alerts within a portfolio based strategy are evaluated only when the installation has rule-based trading switched on as well.
