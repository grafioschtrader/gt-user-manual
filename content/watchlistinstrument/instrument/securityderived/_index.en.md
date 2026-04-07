---
title: "Security and derived instrument"
date: 2026-01-20T10:54:47+01:00
draft: false
weight: 8
archetype: "default"
---
A **security** and a **derived security** have many things in common, which is why they are documented together here.

## Security split
If a security split is entered whose date is before the "Full data load" date, the historical data for this security is read in again.

## Create and edit security or derived instrument
A security or instrument is created, edited and deleted in the main area in one of the **watchlist views**. These views can be accessed in the navigation area via the name of the **watchlist**.
- Create a security via the context menu by selecting "**Add new security**".
- Create a derived instrument by selecting "**Add new derived instrument**".
- Edit a security or instrument via the context menu with the menu item "**Edit instrument**" for the selected instrument or security.
- Delete a security or instrument via the context menu with the menu item "**Remove and delete instrument**" on the selected instrument.

## Properties
Most of the common properties of **securities** and **derived securities** are self-explanatory and are not documented further.
- **ISIN** and **ticker/symbol**: The **ISIN** property can only be set when creating an instrument. If the **ISIN** is displayed, a value must be entered. Together with the currency, a security is identified in GT. The **ticker/symbol** should be entered if possible; these may be used in the **user-defined additional fields** for **identification** with a **data provider**.
- **Private security**: An instrument can be defined as private. This means that it is not visible to other users. A **private security** cannot have **an ISIN** and **ticker/symbol**.
- **Currency**: The currency of the instrument can no longer be changed after it has been saved for the first time.
- **Trade from**: If this date is set to an earlier time for an existing **active** security, the historical price data is reloaded.
- **Stock exchange hyperlink**: This hyperlink is intended for calling up a main trading center for the instrument. This functionality is offered in different views via the context menu.
- **Product hyperlink**: This can be used to enter a hyperlink for the product. This hyperlink is offered in different views via the context menu. This hyperlink will have different characteristics depending on the financial instrument. In the case of an ETF or investment fund, the link to the product of the corresponding provider is usually entered. In the case of a share, the link could refer to the **investor relations** of the corresponding company.

## GTNet Security Search
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet security search is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}

When creating a new security, the **GTNet security search** can be used to import security data from other GTNet instances. This significantly simplifies the creation of new securities, as metadata and connector settings are automatically pre-filled.

### How it Works
In the dialog for creating a new security, the **GTNet Search** button is available. After entering a search term (e.g., name, ISIN, or ticker symbol), a request is sent to all connected GTNet instances.

### Search Results
The search results are displayed in a table with the following information:
- **Name**: Name of the security
- **ISIN**: International Securities Identification Number
- **Currency**: Trading currency
- **Ticker/Symbol**: Stock exchange symbol
- **Stock Exchange**: Name and MIC code of the exchange
- **Asset Class**: Category of the security (e.g., Stock, ETF)
- **Financial Instrument**: Type of instrument
- **Source Domain**: GTNet instance from which the data originates

### Connector Matching
Each table row can be expanded to display the matching connector settings. Four categories are distinguished:
- **Historical Prices**: Connector and URL extension for historical price data
- **Intraday Prices**: Connector and URL extension for daily prices
- **Dividends**: Connector and URL extension for dividend data
- **Splits**: Connector and URL extension for split data

The connectors are only displayed if a matching connector is available in the local GT instance. This enables automatic adoption of the settings. The matching rules differ between built-in and generic connectors.

#### Built-in Connectors
Built-in connectors (e.g., Yahoo, Finnhub) are identical across all GT installations. A match is found when the local instance has the **same connector installed**. No further configuration comparison is needed.

#### Generic Connectors
Generic (user-defined) connectors are configured individually per GT instance. Two instances may have a generic connector with the same short ID but completely different API configurations. For this reason, the matching is stricter. A generic connector from a remote instance is only matched if **all** of the following conditions are met:
1. A local generic connector with the **same short ID** exists.
2. The **domain URL** of the local connector is identical to the one reported by the remote instance.
3. The **regex URL pattern** is identical (or both are empty).
4. The local connector has **endpoints** for the required feed types (e.g., historical and/or intraday).

If any of these conditions is not met, the generic connector will **not** be shown in the expanded row details and its settings will not be transferred.

{{% notice tip %}}
If you want generic connector settings to transfer between two GT instances, make sure that both instances define the generic connector with identical **short ID**, **domain URL**, and **regex URL pattern**. The easiest way to achieve this is to use the same SQL script to create the generic connector definition on both instances.
{{% /notice %}}

### Data Import
After selecting a search result and clicking **Apply Selected**, the security data is transferred to the creation form. The user can still adjust the data before saving.

The following data is considered during import:
- **Master data**: ISIN, name, currency, ticker/symbol
- **Classification**: Asset class and financial instrument
- **Exchange information**: Name, MIC code and hyperlink of the stock exchange
- **Time period**: Trade from and trade to (if available)
- **Specific properties**: Denomination, distribution frequency and leverage factor (depending on instrument type)
- **Hyperlinks**: Product hyperlink (if available)
- **Connectors**: Data sources for historical prices, intraday prices, dividends and splits
