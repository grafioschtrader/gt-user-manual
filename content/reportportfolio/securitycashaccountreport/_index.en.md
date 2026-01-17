---
title: "Asset Classes with Cash"
date: 2025-11-18T22:54:47+01:00
draft: false
weight: 40
archetype: "default"
---
The "Asset Classes with Cash" report provides a comprehensive portfolio allocation analysis that displays both securities and cash holdings grouped by asset classes. This report is the central tool for strategic asset allocation, as it shows how total assets are distributed across different asset classes such as equities, bonds, commodities, and cash. Cash holdings are treated as special asset classes to enable a complete overview of the entire asset allocation.

## Functionality
The report groups all securities and cash holdings of the tenant by their asset classes. Securities are classified according to their actual asset class (e.g., equities, bonds, real estate, commodities), while cash holdings are represented as pseudo-securities in special asset classes. The grouping is structured as follows:

- **Securities**: Are grouped according to their asset class, for example:
  - Equities
  - Fixed Income
  - Money Market
  - Commodities
  - Real Estate
  - Multi-Asset
  - Convertible Bonds
  - Credit Derivatives
  - Currency Pairs

- **Cash holdings**: Are treated as special asset classes:
  - **Main currency**: Cash holdings in the tenant's main currency appear as a standalone asset class
  - **Foreign currencies**: Cash holdings in foreign currencies are displayed separately as another asset class

All transactions up to the selected date are considered to ensure a point-in-time accurate presentation of asset allocation. The calculation is performed across all portfolios and securities accounts of the tenant, with all values automatically converted to the main currency.

## Grouping by Asset Classes
The primary grouping is by asset classes, with each asset class displayed as a separate group with all associated securities and positions. For each group, the following information is shown:

- A **group header** with the name of the asset class
- All **securities** of this asset class with their individual positions
- A **group total** with the aggregated values across all securities of this asset class

At the end of the report, the **grand total** appears across all asset classes, representing the tenant's total assets.

## Columns in the Report
The report displays the following columns for each security or cash holding:

- **Name**: The name of the security or the designation of the cash holding. For cash holdings, the corresponding currency is displayed
- **I**: Icon for the financial instrument. This symbol visualizes the type of instrument (e.g., stock, bond, ETF, currency pair) and facilitates quick identification in the table
- **Holding**: The number of units held. For bonds, the value is multiplied by 100 to represent the nominal value. For cash holdings, this value is not displayed
- **Currency**: The currency of the security or cash holding
- **Timestamp**: The time of the last price. Shows the exact time for intraday prices, and the date for end-of-day prices
- **Last**: The last available price of the security in its currency. For cash holdings, this is typically 1.0
- **Gain security**: The gain or loss from this security in the main currency. This includes both realized gains from sold positions and unrealized gains from open positions, including currency gains
- **Gain security (Main currency)**: The gain or loss in the tenant's main currency
- **Total**: The current total value of the position in the main currency. This is the number of units multiplied by the current price, converted to the main currency

{{% notice note %}}
All columns with monetary amounts in the main currency show the currency as a suffix in the column header, for example "CHF" or "EUR".
{{% /notice %}}

### Group Totals and Grand Totals
Subtotals are displayed for each asset class. These summary rows are highlighted in color and show the aggregated values of all securities and cash positions within the asset class. The group total includes:

- The sum of all gains and losses of the asset class
- The total value of all positions in this asset class

At the end of the report, the grand total appears across all asset classes, showing the tenant's total assets (securities plus cash holdings) in the main currency.

## Filter Options
The report offers the following filter options to customize the view:

- **Until date**: The report can be set to a specific date, showing holdings and values at a historical point in time. This is particularly useful for year-end closings, tax evaluations, or comparisons between different time points. Clicking the replay icon next to the date field resets the date to today
- **Show closed positions**: This option can be activated via the context menu to also display securities that have been completely sold in the meantime. This is useful for analyzing historical performance or for tax purposes

## Context Menu and Interactions
By right-clicking anywhere in the table or via the menu icon, the context menu opens with the following functions:

### Turn on/off columns
This function opens a dialog for column configuration that allows showing or hiding individual columns. This is particularly useful for focusing the view on relevant information or achieving a clearer presentation with limited screen space.

The column configuration is saved and persists even after closing and reopening the report. This allows each user to configure their individual view.

Typical use cases for column configuration:
- Hiding detail columns for a compact overview
- Focus on gains and total values
- Display only the most important metrics for a quick overview
- Adaptation to different screen sizes or resolutions

### Show Chart
This function activates or deactivates the chart view below the table. The chart is generated dynamically based on current data and provides a visual presentation of asset allocation across the different asset classes.

### Show closed positions
With this option, securities that have been completely sold in the meantime can also be displayed. The option is marked with a checkmark when active. This is particularly useful for:
- Analysis of realized gains and losses
- Tax evaluations
- Historical performance analyses
- Complete transaction overview

## Expandable Rows
Each position can be expanded by clicking the expander icon. The expanded view displays different information depending on whether it is a security or a cash holding:

### Expansion for Securities
The expanded view displays all **transactions** for this security. This enables detailed tracking of all purchases, sales, dividends, and other transactions that led to the current position.

The transaction list shows for each transaction:
- The transaction date
- The transaction type (purchase, sale, dividend, etc.)
- The number of units
- The price
- The transaction costs
- The total amount

### Expansion for Cash Holdings
For cash holdings represented as pseudo-securities, the expanded view shows all **account transactions** that led to this cash holding. This includes:
- Deposits and withdrawals
- Internal transfers between accounts
- Account fees
- Interest income
- Security transactions that affected the cash holding

This enables seamless tracking of all cash movements and transparently shows how the current cash holding is composed.

### Context Menu on Transactions
For expanded rows, individual transactions can be edited. Right-clicking on a transaction opens the context menu with corresponding editing options. For more information, see [Security Transactions](../../../transaction/security/) or [Cash Account Transactions](../../../transaction/cashaccount/).

## Chart View
The chart view is a powerful tool for visual analysis of asset allocation. It is activated via the "Show Chart" function in the context menu and appears below the table view.

### Presentation Form
The chart is displayed as a stacked bar chart or pie chart, where each segment represents an asset class. The size of each segment corresponds to the proportion of that asset class in total assets. This presentation enables direct visual recognition of asset distribution.

### Color Coding
Each asset class receives a unique color that remains consistent across all presentations. This color coding enables at-a-glance recognition of which asset classes make up the largest share of assets.

Typical color assignments:
- Equities: Blue tones
- Bonds: Green tones
- Cash holdings: Yellow tones
- Commodities: Brown tones
- Real Estate: Red tones

### Interactive Functions
The chart is fully interactive and offers the following possibilities:
- **Hover function**: When hovering over a segment with the mouse, detailed information is displayed, including the exact value and percentage share of the asset class
- **Zoom**: By clicking and dragging, you can zoom into the chart
- **Pan**: In zoomed state, the chart can be moved
- **Reset**: Double-clicking on the chart restores the original view
- **Legend**: The legend explains the color coding and can be used for filtering by clicking on individual elements

### Chart Interpretation
The chart enables various analyses:

**Asset Allocation**: The relative size of the segments immediately shows how assets are distributed across different asset classes. A balanced distribution indicates a diversified investment strategy.

**Liquidity Level**: The proportion of cash holdings (main currency and foreign currencies) shows how much liquid funds are available. A very low proportion may indicate a high investment level, while a very high proportion suggests unused capital.

**Diversification**: An even distribution across multiple asset classes indicates a well-diversified strategy, while a strong concentration on one or two asset classes signals higher concentration risk.

**Rebalancing Needs**: Large deviations from the target asset allocation become immediately visible and can serve as a signal for necessary rebalancing.

### Updates
The chart is automatically updated when:
- The until date is changed
- The "Show closed positions" option is toggled
- The data is reloaded

The chart presentation adapts seamlessly to the new data while maintaining color coding.

## Color Display
For better readability and faster recognition of gains and losses, the report uses consistent color coding:
- **Positive values** (gains): Displayed in standard text color or green
- **Negative values** (losses): Displayed in red

This color coding applies to the gain columns, making it immediately apparent which asset classes and securities show positive performance and which show losses.

## Typical Use Cases
The "Asset Classes with Cash" report supports various analytical approaches for managing and optimizing asset allocation:

### Strategic Asset Allocation
The most important use case is reviewing and controlling strategic asset allocation. Investors often define target quotas for different asset classes (e.g., 60% equities, 30% bonds, 10% cash). This report immediately shows whether the actual allocation deviates from these targets and enables informed rebalancing decisions.

### Risk Management
Different asset classes have different risk profiles. The report enables a quick assessment of the portfolio's overall risk through the distribution across asset classes. A high concentration in risky asset classes (e.g., equities, commodities) indicates an aggressive profile, while an emphasis on bonds and cash signals a conservative profile.

### Liquidity Management
The separate reporting of cash holdings in main currency and foreign currencies enables precise liquidity planning. It is immediately apparent how much liquid funds are available and whether these are sufficient for planned investments or whether excess liquidity should be invested.

### Performance Attribution
Through grouping by asset classes, it becomes visible which asset classes contribute to overall performance and which are performance drivers. This supports the analysis of whether the investment strategy is successful or whether adjustments are necessary.

### Currency Diversification
For international portfolios, the report shows not only diversification across asset classes but also across currencies. The separate reporting of foreign currency holdings enables assessment of currency risk.

### Rebalancing Planning
Large deviations from the target allocation become immediately visible. The report shows in which asset classes capital should be withdrawn and in which it should be invested to restore the desired asset allocation.

### Tax Loss Harvesting
Through the display of closed positions and their gains/losses, the report can be used for tax-optimized selling strategies. Loss-making positions can be identified to offset them against gains.

### Transaction Tracking
By expanding rows, all transactions that led to current positions can be traced. This is valuable for:
- Verifying the correctness of imported transactions
- Analyzing trading activity per asset class
- Identifying transaction patterns
- Complete documentation for tax purposes

## Special Features
- **Complete asset overview**: Unlike pure securities reports that only show invested funds, this report provides a complete picture through the inclusion of all cash holdings as special asset classes. This enables a realistic assessment of the entire asset allocation
- **Treatment of cash holdings**: Cash holdings are represented as pseudo-securities in special asset classes. This enables consistent presentation and calculation of asset allocation across all assets
- **Main currency vs. foreign currencies**: Cash holdings in the tenant's main currency are reported separately from foreign currency holdings. This enables a differentiated view of liquidity and currency risk
- **Multi-currency support**: All values are automatically converted to the tenant's main currency using the current exchange rates at the selected date
- **Comprehensive asset classes**: The report supports all common asset classes, from traditional equities and bonds through real estate and commodities to special categories like convertible bonds and credit derivatives
- **Historical analysis**: Through the ability to set an until date, historical asset allocations can be analyzed. This is valuable for analyzing strategy development over time or for tax evaluations at specific dates
- **Closed positions**: The option to display closed positions enables a complete overview of all transactions and realized gains/losses within a period
- **Transaction details**: By expanding rows, all underlying transactions for both securities and cash holdings can be viewed, enabling complete tracking
