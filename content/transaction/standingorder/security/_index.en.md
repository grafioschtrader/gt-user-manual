---
title: "Standing order security"
date: 2026-02-28T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
The security standing order automatically creates recurring buy or sell transactions. Compared to the cash standing order, it is considerably more comprehensive, as the quantity can be calculated from a price, costs can be defined via formulas, and any necessary currency conversion is taken into account.

## Transaction types
Two transaction types are available for security standing orders:
- **Buy**: Regular purchase of a security. A typical use case is a monthly ETF savings plan.
- **Sell**: Regular sale of a security. Can be used, for example, for a gradual reduction of a position.

## Investment mode
The central configuration of the security standing order is the **investment mode**. This determines whether a fixed number of units or a fixed amount is used per execution. Exactly one of the two modes must be chosen.

### Fixed units
In this mode, the same number of units is bought or sold with each execution. The resulting amount varies depending on the current price of the security.

### Fixed amount
In this mode, a fixed monetary amount is invested with each execution. The number of units is calculated based on the current price. This mode is particularly suitable for a savings plan, as more units are acquired at low prices and fewer at high prices (dollar-cost averaging effect). Two additional options are available:
- **Fractional units**: When enabled, fractional shares can be acquired (e.g. 5.237 units). When disabled, the calculated number of units is rounded down to whole units.
- **Amount includes costs**: When enabled, the **invest amount** is the total amount including costs. The effectively invested sum is then the amount minus costs. When disabled, the **invest amount** is the pure investment amount and costs are charged additionally.

## Calculation at execution
When a security standing order is executed, the historical closing price of the security on the execution date is required. If no price is available, the execution is skipped for that date and an entry is created in the [failure log](../#execution-failures).

### Calculation flow
The following flow shows how the background process calculates the securities transaction:
{{< mermaid >}}
graph TD
    A[Look up historical closing price] --> B{Price available?}
    B -- No --> C[Log failure]
    B -- Yes --> D{Investment mode?}
    D -- Fixed units --> E["Units = configured value"]
    D -- Fixed amount --> F["Units = amount / price"]
    F --> G{Fractional units allowed?}
    G -- No --> H[Round units down]
    G -- Yes --> I[Keep units as calculated]
    H --> J{Units > 0?}
    I --> J
    J -- No --> C[Log failure]
    J -- Yes --> K[Calculate costs]
    E --> K
    K --> L[Calculate cash account posting]
    L --> M[Create transaction]
{{< /mermaid >}}

### Formula for cash account posting
The cash account posting is calculated as follows, where the gross amount equals the product of units and price:
- **Buy**: Cash account posting = −(gross amount + tax + transaction costs)
- **Sell**: Cash account posting = +(gross amount − tax − transaction costs)

If a currency conversion is configured, the cash account posting is subsequently converted using the historical exchange rate of the execution date.

## Cost handling
For the automatic consideration of taxes and transaction costs, two configuration approaches are available, which can be chosen independently per cost type.

### Fixed amount
A fixed cost value is used with each execution. This is useful for brokers with flat transaction fees or known fixed tax amounts.

### Formula
Alternatively, a formula can be entered that is evaluated with the current values at each execution. If both a fixed amount and a formula are specified, the fixed amount takes priority. Three variables are available in the formula:

| Variable | Meaning | Example |
|---|---|---|
| `u` | Units | 10.5 |
| `q` | Price of the security | 48.30 |
| `a` | Gross amount (u × q) | 507.15 |

#### Formula examples
| Formula | Description |
|---|---|
| `a * 0.01` | 1% of the gross amount |
| `a * 0.0015` | 0.15% of the gross amount (stamp tax) |
| `u * 0.50` | 0.50 per unit |
| `25` | Flat fee of 25 |
| `MAX(a * 0.005, 5)` | 0.5% of the gross amount, minimum 5 |

{{% notice style="info" title="Formula evaluation" %}}
The formulas are evaluated using the EvalEx library, which supports standard mathematical functions such as `MIN`, `MAX`, `ABS`, `ROUND` and others. If a formula contains an error, the cost value is set to 0 and a warning is logged.
{{% /notice %}}

## Exchange rate
If the currency of the security differs from the currency of the account, a **currency pair** must be specified. At execution, the historical exchange rate on the execution date is used to convert the cash account posting into the account currency. This behaviour corresponds to the currency handling for manual [securities transactions](../../security/).

{{% notice note %}}
If no historical exchange rate is available on the execution date, the next older available rate is used.
{{% /notice %}}

## Input fields
When creating or editing a security standing order, the following information must be provided:
- **Transaction type**: Buy or sell.
- **Account**: The account through which the transaction is settled.
- **Security**: The security to be traded. This field is pre-filled when editing or when creating from a transaction.
- **Investment mode**: Fixed units or fixed amount.
- **Units** (for fixed units): The number of units per execution.
- **Invest amount** (for fixed amount): The monetary amount per execution.
- **Fractional units** (for fixed amount): Whether fractional shares may be acquired.
- **Amount includes costs** (for fixed amount): Whether costs are included in the invest amount.
- **Tax (every kind)**: Fixed tax amount per execution (optional).
- **Tax cost formula**: Formula for dynamic tax calculation (optional).
- **Transaction cost**: Fixed fee amount per execution (optional).
- **Transaction cost formula**: Formula for dynamic fee calculation (optional).

Additionally, the common fields of the [repeat configuration](../) are available.

Once the standing order has created transactions, the transaction-specific fields (transaction type, account, security, currency pair, investment mode settings and all cost fields) are locked. Only the scheduling fields and note can still be edited. See [Editing and deletion restrictions](../#editing-and-deletion-restrictions) for details.

## Restrictions
- **Price dependency**: Execution requires that historical price data for the security is available on the execution date. If no price is available, the date is skipped and an [execution failure](../#execution-failures) is logged.
- **Exchange rate dependency**: If a currency pair is configured, a historical exchange rate must also be available on the execution date.
- **Minimum units**: If the calculation results in a number of units less than or equal to zero (e.g. because costs exceed the invest amount), the execution is skipped and an [execution failure](../#execution-failures) is logged.
- **Costs are estimates**: The calculated costs do not necessarily correspond to the actual costs of your trading platform. It is recommended to periodically compare the created transactions with actual statements and adjust them if necessary.

## Created transactions
Each transaction created by the standing order corresponds to a normal [securities transaction](../../security/). It can be viewed in the usual transaction views and edited if necessary. Additionally, the created transactions are visible directly in the standing order table via the row expander, where they can also be edited or deleted via the context menu. See [Viewing created transactions](../#viewing-created-transactions) for details.
