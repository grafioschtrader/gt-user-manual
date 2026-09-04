---
title: "Standing orders"
date: 2026-08-04T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
Standing orders enable the automated creation of recurring transactions. They are particularly useful for regular deposits or withdrawals to accounts, as well as for periodic securities purchases as part of a savings plan. Instead of manually entering each transaction, you define the parameters of the standing order once, and GT automatically creates the transactions at the scheduled time.

{{% notice style="info" title="Transaction limit" %}}
The automatic executions of a standing order also count towards the [per-client transaction limit](../). When the limit is reached, the due execution is not carried out but recorded in the failure list. It is resumed automatically on the next run as soon as space is available again by deleting existing transactions.
{{% /notice %}}

## Access in the frontend
Standing orders are located under the tenant view in the **Standing orders** tab. This tab contains two sub-tabs:
- **Standing order cash**: For recurring account transactions such as deposits and withdrawals.
- **Standing order security**: For recurring securities transactions such as buys and sells.

## Repeat configuration
Each standing order contains a repeat configuration that determines when the next transaction is created.

### Repeat unit and interval
The **repeat unit** determines the time measure of the repetition. Together with the **repeat interval**, this defines the spacing between executions:
- **Days**: Execution every *n* days, where *n* is the repeat interval. The **day position**, **execution day** and **execution month** fields are disabled since they are not relevant for day-based intervals.
- **Months**: Execution every *n* months. The **day position** determines the exact day within the month. The **execution month** field is disabled since it is only relevant for yearly repetition.
- **Years**: Execution every *n* years. The **execution month** is selected from a dropdown with localized month names. Together with the **day position**, it determines the exact timing.

### Day position
For monthly or yearly repetition, the **day position** determines the day within the month:
- **Specific day**: Execution on the specified **execution day** (1–28). This is the only day position where the **execution day** field is enabled.
- **First day**: Execution on the 1st of the month. The **execution day** field is disabled since the day is implicit.
- **Last day**: Execution on the last day of the month. The **execution day** field is disabled. This option is particularly convenient for varying month lengths (28, 29, 30, 31 days).

### Non-trading-day adjustment
If an execution date falls on a day on which no trading takes place, GT shifts the execution automatically. The **Non-trading-day adjustment** only determines the direction of that shift:
- **Shift to earlier day**: The execution moves to the preceding suitable day.
- **Shift to later day**: The execution moves to the next suitable day.

Which days are skipped depends on the type of standing order. For a cash standing order only weekends are skipped, because a bank also charges a fee or credits interest on a public holiday. For a security standing order the trading calendar of the stock exchange on which the security is traded is taken into account as well. The execution therefore always lands on a day on which that exchange is actually open. A savings plan with 1 January as its execution day is shifted to the first trading day of the new year in this way.

{{% notice style="note" title="The shift happens only at execution time" %}}
The trading calendar is evaluated only once the execution date has been reached. The **Next execution** column therefore shows the scheduled date and not the trading day it may be shifted to.
{{% /notice %}}

### Quote tolerance
Even on a regular trading day a price may be missing from the price database. So that such a gap does not immediately cause an execution failure, the **Quote tolerance (days)** defines how far GT may deviate from the execution date in order to find a price or an exchange rate. The magnitude of the number is the search window in days on both sides, the sign determines which side is examined first. With 0 the price must exist exactly on the execution date. With −2 GT first searches up to two days into the past and only then up to two days into the future; with 2 it is the other way round. The date of the created transaction always remains the execution date, only the price used may originate from a neighbouring day.

The administrator defines the permitted value range via the global setting `gt.standing.order.quote.tolerance` in the form "min:max". The default "-3:3" allows up to three days in both directions, "0:3" forbids reaching into the past, and "0:0" requires the price exactly on the execution date.

### Examples
| Configuration | Description |
|---|---|
| Months, interval 1, specific day 15 | On the 15th of every month |
| Months, interval 3, first day | On the 1st of every quarter |
| Months, interval 1, last day | On the last day of every month |
| Years, interval 1, month 6, specific day 1 | On June 1st every year |
| Days, interval 14 | Every 14 days |

## Validity and deactivation
Each standing order has a **valid from** and a **valid to** date. The first execution occurs at the earliest on the valid-from date. As soon as the next calculated execution date lies after the valid-to date, the standing order is automatically deactivated. Whether a standing order is still active can be determined from the **Next execution date** column: if a date is shown, the next execution is still pending. If the column is empty, the standing order has been deactivated.

## Catch-up mechanism
If the background process has not been executed for several days, for example due to a server outage, missed execution dates are caught up on the next run. A separate transaction is created for each due date. This also applies when the valid-to date of a standing order has already passed at the time the background process runs, for example because the server was offline during the entire execution period. In that case, all missed execution dates are still processed before the standing order is deactivated. This ensures no scheduled executions are lost.

## Limit
The number of standing orders per tenant is limited. The default value is 50 and can be adjusted by the administrator via the global settings.

## Editing and deletion restrictions
The **transaction type** (e.g. deposit/withdrawal or buy/sell) is always locked after a standing order has been created. It cannot be changed regardless of whether transactions have already been created.

Once a standing order has created transactions, the following additional restrictions apply:
- **Deletion is not possible**: The standing order cannot be deleted because the created transactions reference it. The delete option is hidden from the context menu.
- **Valid-from date is locked**: The valid-from date can no longer be changed once transactions exist, since it anchors the execution history.
- **Transaction fields are read-only**: Only the scheduling fields (repeat configuration, valid-to date, note) can still be edited. The remaining transaction-specific fields (account, amount, security, costs, etc.) are locked to maintain consistency with the already created transactions.

## Showing inactive standing orders
By default, inactive standing orders (those that have been automatically deactivated after reaching their valid-to date) are hidden from the table. To display them, use the **Show inactive standing orders** option in the show menu. The toggle switches between showing and hiding inactive entries.

## Creating a standing order
A new standing order can be created via the **context menu** in the respective standing order table. Additionally, a standing order can be created directly from an existing transaction. To do this, select the desired transaction in a transaction view and call the **Create standing order** function via the context menu. The transaction data such as account, amount and transaction type are used as a template.

## Execution failures
When the execution of a standing order fails, the error is recorded in a failure log. Typical causes for failures with securities standing orders are a price that cannot be found even within the quote tolerance, or a calculated unit count of zero. In any case, the standing order advances to the next scheduled date, so a single failure does not block the entire processing.
In the standing order table, the **Failures** column shows the number of execution failures that have occurred. Standing orders with failures can be expanded using the row expander. The failure table shows the **Execution date** and the **Business error** for each failed date. If an unexpected technical error occurred, the respective failure row can be expanded once more to view the full error text.

{{% notice style="info" title="Failures are cleaned up on deletion" %}}
When a standing order is deleted, all associated failure logs are automatically removed as well.
{{% /notice %}}

## Viewing created transactions
The standing order table includes a **Transactions** column that shows the number of transactions created by each standing order. Standing orders with created transactions can be expanded using the row expander, similar to the failure display. The expanded area shows a transaction table with details such as date, transaction type and amounts. From this table, individual transactions can be edited or deleted via the context menu. Note that editing or deleting a transaction here does not change the standing order's scheduling or configuration — the standing order continues to execute according to its repeat configuration independently of any manual changes to previously created transactions.

## Background processing
The execution of standing orders is carried out by a daily [background process](../../admindata/taskdatachangemonitor/taskdescription/#JOB28). This process checks which standing orders are due and creates the corresponding transactions. The default execution time is after the daily price update, so that current price data is available for securities standing orders. The following simplified flow shows how the background process handles a single standing order:
{{< mermaid >}}
graph TD
    A[Check next execution date] --> B{Execution date due?}
    B -- No --> C[No action]
    B -- Yes --> D{Type of standing order?}
    D -- Cash --> E[Skip weekends]
    D -- Security --> F[Skip weekends and exchange holidays]
    E --> G[Create cash transaction]
    F --> H[Search for a price within the quote tolerance]
    H --> I[Calculate units and costs]
    G --> J[Compute next execution date]
    I --> J
    H -- No price found --> K[Save failure to log]
    K --> J
    J --> L{Next date within validity?}
    L -- Yes --> B
    L -- No --> M[Deactivate standing order]
{{< /mermaid >}}
