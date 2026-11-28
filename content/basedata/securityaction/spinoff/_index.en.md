---
title: "Spin-off"
date: 2026-11-20T12:00:00+01:00
lastmod: 2026-11-20T21:00:00+01:00
draft: false
weight: 10
archetype: "default"
---
In a spin-off, shareholders of the parent company receive shares in the new company. The number of parent shares stays the same. Part of their cost basis passes to the new security.

The following diagram shows the tasks of the administrator and the user, from recording the spin-off to applying it to your own securities accounts:

{{< mermaid >}}
sequenceDiagram
    participant A as Administrator
    participant S as System (GT)
    participant U as User

    A->>S: Record spin-off: parent, child,<br/>ex-date and ratio
    opt Child not yet in GT
        S-->>A: Proposal from the data provider lookup<br/>(name, ticker, connectors)
    end
    opt Propose allocation from closes
        S-->>A: Common price day, closing prices,<br/>exchange rate and estimated percentage
    end
    A->>S: Saves the cost allocation with its source<br/>and the German tax treatment
    S->>S: Saves a new security and counts the tenants<br/>holding the parent before the ex-date
    S->>S: Background task: read splits, decide history basis,<br/>reload the parent's price history
    S->>U: Notification by internal mail
    U->>S: Apply spin-off
    S-->>U: Preview per account: units, prices,<br/>amounts and existing transactions
    U->>S: Chooses Book or Skip per account
    S->>S: Creates parent sale and repurchase and child purchase<br/>for each booked account
    S-->>U: Status: Applied
    opt Correction
        A->>S: Edit spin-off: new cost allocation
        S->>S: Reports calculate with the new percentage,<br/>the bookings stay unchanged
    end
{{< /mermaid >}}

## Record the spin-off
An administrator opens **Base data → Security action**, selects **System actions** and chooses **Record spin-off**. They select the parent and child securities. To add a new child security, they can look it up by ISIN and trading currency and review the proposed name, ticker and connectors.

Use the ex-date and express the ratio in the share basis on that day. For a 4-to-1 spin-off, an account holding 100 parent shares receives 25 child shares.

Enter the published percentage under **Cost allocation** and record its source. **Propose allocation from closes** estimates the percentage and shows the common price day, closing prices and exchange rate. How GT handles a provider split and the parent's price history is described in [Provider split and price history of the parent](#HISTORYBASIS).

## Provider split and price history of the parent {#HISTORYBASIS}
Many data providers, such as Yahoo and EOD Historical Data, report a spin-off as a split of the parent on the ex-date and scale the parent's prices before the ex-date down by that factor. For General Electric, Yahoo reports a split of 1000 to 1281 on 2023-01-04 (GE HealthCare) and one of 1000 to 1253 on 2024-04-02 (GE Vernova). Stored as a split, the factor would multiply every share bought before the ex-date. 300 shares would become 384.3, and together with the bookings of the spin-off the value of the new shares would be counted twice.

For a parent with a recorded spin-off, GT therefore does not take over the split the connector delivers exactly on the ex-date. Splits on other days and splits entered by hand remain. GT stores the prices before the ex-date as they were traded on those days: if the price source scaled them down for the spin-off, GT multiplies them back by the history factor when loading them. The adjustment for real splits is kept. Without a recorded spin-off the provider's split remains, and GT treats the event approximately like a split. Deleting a spin-off restores that state together with the delivered price history.

### History basis
Whether a price history is scaled down for a spin-off depends on the price source and on the individual event, not on whether prices and splits come from the same connector. onvista, for example, delivers Pfizer adjusted for the Viatris spin-off, but Mercedes-Benz unadjusted for Daimler Truck. Yahoo adjusted the prices of Mercedes-Benz without reporting a split. EOD Historical Data adjusts only the close, while open, high and low stay as traded there. GT therefore keeps a **History basis** for each spin-off: **Unadjusted**, **Adjusted** with a **History factor**, or **Undetermined**. It applies to the price source of the parent for which it was decided.

GT decides the history basis every time before it reloads the parent's price history in full. If an administrator has set the history basis and it has not yet been applied, that setting applies. If the stored prices before the ex-date are known to be unadjusted, GT compares them with the newly delivered prices before and after the ex-date; the ratio gives the history factor exactly. The stored prices are known to be unadjusted when they were loaded before the ex-date was reached, or when they were already stored under a decided history basis. If no comparison is possible, an administrator's setting that has already been applied remains. Otherwise GT examines the price step on the ex-date in the delivered prices. The step decides only when it is clear: the rejected reading would have to imply a market move of at least 10 % and three times the other one. GT takes over a history factor in this case only if the split was reported by the same price source. In all other cases the history basis stays **Undetermined**, and GT stores the prices as the source delivers them.

{{< mermaid >}}
graph TD
    A[Full reload of the price history] --> B{History basis set by the<br/>administrator, not yet applied?}
    B -->|Yes| Z1[Administrator's setting]
    B -->|No| C{Stored prices before the<br/>ex-date known to be unadjusted?}
    C -->|Yes, comparison possible| Z2[History factor from comparing<br/>stored and delivered prices]
    C -->|No| D{Applied setting<br/>of the administrator?}
    D -->|Yes| Z1
    D -->|No| E{Clear price step<br/>on the ex-date?}
    E -->|Prices show the drop| Z3[Unadjusted]
    E -->|Prices scaled down,<br/>split from the same source| Z4[Adjusted with the<br/>factor of the split]
    E -->|not clear| Z5[Undetermined:<br/>prices as delivered]
{{< /mermaid >}}

If a price source reports the split before it has adjusted its price history, the delivered prices are still unadjusted. GT recognizes this and multiplies nothing. At the next full reload, the comparison with the stored prices finds the factor of the series adjusted in the meantime.

### What happens after recording
After a spin-off is recorded or deleted and after every change of the history basis, GT queues [background task 81]({{% relref "/admindata/taskdatachangemonitor/taskdescription/splitdividend/#JOB81" %}}). It reads the splits, decides the history basis, reloads the parent's price history and then rebuilds its holdings. While the task is waiting or running, GT refuses the preview and the application with a message. Try again after a few minutes.

The bookings of a spin-off use closing prices from its ex-date onward. These can be scaled down for a later spin-off of the same parent or of the child. GT therefore refuses the preview, the application and the allocation proposal as long as such a later spin-off has the history basis **Undetermined**. An undetermined history basis of the spin-off itself blocks nothing. It can, however, leave the parent's prices before the ex-date too low, and the performance per security then shows a gain across the ex-date of about the value of the new shares.

### Check and set the history basis
The column **History basis** in the system actions shows the administrator where clarification is needed. In the **Edit spin-off** dialog, the button **Provider splits and history basis** shows the splits the split connector reports within ten days of the ex-date, the **Provider split factor** with its **Spin-off part of the provider factor**, the current history basis with its **History source of the decision**, and the **Price step of the stored history** and the **Price step of the delivered history**. A price step is shown with the market move it implies if the prices are adjusted or unadjusted. If the cost allocation was proposed from closes beforehand, the **Factor implied by the closes** appears as well. The same button is available when recording, as soon as parent and ex-date are chosen.

Under **History basis** the administrator chooses **Automatic**, **Unadjusted**, or **Adjusted** with the **History factor** by which the price source divided the prices before the ex-date. The factor of another connector's split can differ from the factor of the price source. Saving reloads the parent's price history, and the note of the spin-off records the change. A history basis that has been set applies only to the price source it was set for. If the parent's price source changes, GT decides again by itself. **Automatic** hands the decision back to GT.

### Example General Electric
General Electric takes prices and splits from Yahoo. The table shows the closing prices as Yahoo delivers them today and as GT stores them with the history factors 1.281 and 1.253:

| Date | Close from Yahoo | History factor | Stored close |
|------|-----------------:|----------------|-------------:|
| 2023-01-03 | 52.94 | 1.281 × 1.253 | 84.98 |
| 2023-01-04 | 56.03 | 1.253 | 70.20 |
| 2024-04-01 | 139.95 | 1.253 | 175.36 |
| 2024-04-02 | 136.47 | 1 | 136.47 |

If the price history was loaded before both ex-dates, comparing the stored with the delivered prices gives both factors exactly. If it was loaded only afterwards, the price step decides. A securities account with 300 shares bought on 2022-12-01 receives 100 shares of GE HealthCare and 75 shares of GE Vernova and keeps its 300 shares of General Electric.

### Real split on the ex-date
If a real split falls on the ex-date, the provider delivers both events as a single figure. Enter the real split by hand for the parent in that case. GT divides the provider's factor by this split and uses only the remainder as the history factor of the spin-off. With a split of 1 to 2, a securities account holding 100 shares is thus entitled to the new shares for 200 shares. The dialog shows the provider split factor next to the factor implied by the closes, so such a combined figure stands out. Do not enter a split by hand for the spin-off itself, because GT never drops a split entered by hand.

## Review the preview and apply
Select the spin-off and open **Apply spin-off**. Review each account's eligible units, new shares, prices and cash amounts. The preview lists existing transactions near the ex-date. GT proposes **Skip** when it finds the complete sequence of parent sale, parent repurchase and child purchase. Choose **Book** for accounts where GT should record the spin-off. **Skip** keeps the transactions already booked.

GT records a sale and repurchase of the parent and a purchase of the child for each booked account. Together they leave the cash account balance unchanged. Record the sale of any fractional shares separately when it appears on your statement.

## Cost basis and corrections
GT divides the cost basis by the recorded percentage. With FIFO or LIFO, each new lot inherits the date of its original purchase. With average cost, GT divides the part of the selected holdings' pooled cost attributable to the account. The spin-off itself realizes no gain.

An administrator can correct the percentage through **Edit spin-off**. The recorded prices stay on the bookings. Reports use the corrected cost basis, including when they recalculate gains on sales from earlier years.

## Reverse the spin-off
**Reverse spin-off** removes the associated system transactions. First reverse any dependent child transactions or applied security actions. You can then correct the original bookings and apply the spin-off again.

## Reports
In the [Position chart]({{% ref "/watchlistinstrument/positionchart" %}}) the number of parent shares stays the same across the ex-date. Invested capital passes to the child at its historical cost, in the main currency as well, without a realized result. For the child this cost counts as its purchase. The market value of the new shares leaves the parent as one single outflow, also in [Performance by security]({{% ref "/reportportfolio/securityperiodperformance" %}}). The parent's price drop on the ex-date therefore does not appear as a loss.

The [German tax help]({{% ref "/reportportfolio/germantax" %}}) follows the **German tax treatment** the administrator records with the spin-off. With **Cost split**, the allocated share of each purchase's euro cost passes to the child and the purchase date stays. Holdings bought before 2009 therefore keep that status for the child as well. If the share relevant for German tax differs from the published one, the administrator enters it under **German tax cost allocation**. With **Taxable distribution**, the market value of the new shares counts as a dividend and the child counts as bought on the ex-date. As long as the treatment is **Undetermined**, the tax help calculates as for a cost split but shows later sales of both securities without an amount. Compare the treatment with your bank statement.

A simulation takes over the spin-off with the portfolio. The cost basis of both securities in the simulation is the same as in the tenant it was created from. The copied spin-off cannot be reversed in the simulation.

## Why three bookings?
A spin-off moves cost from the parent to the child without any cash flowing. GT has no transaction type of its own for this. Such a type would have to be understood by every part of the application: the check of units, the holdings and cash balances, the import and export of transactions, the transaction list and every report. Instead, GT books three ordinary transactions per securities account that together leave the cash balance unchanged. Everything that calculates with purchases and sales therefore stays correct without any change.

The three bookings are valued at market values of the ex-date. The parent is sold at its price before the spin-off, which is the ex-date close plus the value of the new shares, and bought back at the ex-date close. Exactly the market value of the new shares thus leaves the parent and arrives at the child. The cost basis is divided by the recorded percentage only when a report is calculated. The calculations recognize the three bookings by their role and do not treat them as a sale and purchases. That is why nothing has to be rebooked when an administrator corrects the percentage or you change the cost basis method.

An example: an account holds 100 parent shares with a cost basis of 5800. The ratio is 4 to 1, the cost allocation 20 %, and the ex-date closes are 64 for the parent and 48 for the child. The sale price of 76 is the close of 64 plus a quarter of 48. The three bookings cancel out on the cash account while 20 % of the cost basis moves to the child:

{{< mermaid >}}
graph LR
    M1["Parent before the ex-date<br/>100 shares, cost 5800"] -->|"Sale 100 × 76 = +7600"| K["Cash account<br/>balance unchanged"]
    K -->|"Repurchase 100 × 64 = −6400"| M2["Parent after the ex-date<br/>100 shares, cost 4640"]
    K -->|"Purchase 25 × 48 = −1200"| T["Child security<br/>25 shares, cost 1160"]
    M1 -.->|"20 % of the cost, purchase dates kept"| T
{{< /mermaid >}}

The price of this approach: the three bookings appear in the transaction list. They cannot be changed or deleted individually; only **Reverse spin-off** removes them together. GT also rejects a change to an earlier parent booking that would alter the units on the ex-date. In that case reverse the spin-off, correct the booking and apply the spin-off again.

## Limitations
- A spin-off into several new companies cannot be recorded. Only one spin-off per parent and ex-date is possible.
- A split-up, in which the parent dissolves into several successors, and an exchange of parent shares for new shares are not spin-offs in the sense of this action.
- A securities account holding a short position in the parent receives no new shares.
- If the child has no closing price on the ex-date, GT uses the first close of the following ten days. On the days without a price, the child's value is missing from the portfolio's daily values.
- As long as the history basis of a spin-off is **Undetermined**, GT stores the parent's prices before the ex-date as the price source delivers them. A spin-off whose new shares make up less than about 10 % of the parent's value is decided only by the comparison with stored prices or by the administrator.
- GT takes over a provider split that does not fall exactly on the ex-date as a split. The **Edit spin-off** dialog lists the splits around the ex-date.
- A real split on the ex-date must be entered by hand. If it is missing, it is also missing from the entitled number of shares.
- Prices from the archive that are added after a change of the price source keep the history basis under which they were archived.
- GT does not take over prices another GTNet participant delivers for a parent before the latest ex-date, because their history basis is unknown. Only the connector fills those days.
- The trading volume before the ex-date stays as the price source delivers it. With EOD Historical Data and Alpha Vantage the close keeps its dividend adjustment; GT removes only the history factor of the spin-off.
- A correction of the cost allocation applies retroactively, also to realized gains and to the tax help of earlier years.
- Record the cash payment for fractional shares yourself as a sale.
