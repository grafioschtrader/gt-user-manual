---
title: "Correlation matrix"
date: 2025-06-01T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
The knowledge gained from a correlation matrix can certainly be helpful in diversifying a portfolio. After all, it is important to be prepared for any weather situation on the markets with a certain mix of asset classes. This can be achieved by selecting asset classes with low correlation. This reduces the risk of loss without reducing returns. Most correlations change over time. GT therefore also implements a **rolling correlation** as a line graph. The calculations are based on the **closing price** of the **end-of-day data**, using different sample periods. In addition, a **time period** can optionally be limited with dates.

{{% notice note "Warning!" %}}
#### Correlation and dividend
As mentioned in the [glossary](../../glossar/), a distinction must be made between ETFs or funds **that distribute** or **reinvest** **dividends** and between **price** and **performance indices**. There is a similar difference between **shares** with or without **dividends**.

#### GT and this problem
The GT version does not take dividends into account for the correlations. This can lead to slight distortions in the results {{% /notice %}}

## Correlation matrix and correlation set
A correlation matrix is determined by the settings of a single **correlation set**. A user can create and edit several correlation sets.
- An instrument **without price data cannot be added** to the correlation matrix. Such an instrument is defined by a **stock exchange without price data**.
- The maximum number of correlation sets is given by the value of "`gt.max.correlation.set`" in Global Settings.
- The maximum number of instruments per correlation set is defined by the value of "`gt.max.correlation.instruments`" in Global Settings.

## Functionality
The **correlation matrix** is accessed by selecting the static element "**Watchlist**" in the **navigation tree**. We differentiate the functionality with regard to the **basic definition** and the **instruments** of a correlation set. The **correlation matrix** is created by applying the basic definition to the corresponding instruments.

#### Creating and editing correlation sets
The **basic definition of** a correlation set can be edited as follows:
- Create a **correlation set** via the **context menu**.
- Edit a **correlation set** via the **context menu** when **a correlation set is selected**.
- Delete a **correlation set** via the **context menu** when **a correlation set is selected**.

Most of the properties of an existing correlation set can be edited directly in the correlation matrix view. The "**Save and calculate**" button updates the correlation matrix accordingly.

{{% notice note "Calculation of the correlation matrix not possible!" %}}
 It is possible that there are no overlapping closing prices of the instruments. In this case, no correlation matrix is displayed. Instead, the instruments are displayed in the table with the data of the oldest or most recent daily closing price. This makes it easier to check whether the properties `Date from` or `Date to`, no overlapping closing prices or missing closing prices may make it impossible to calculate the correlation matrix. 
 {{% /notice %}}

#### Functions for instruments
With the functions for instruments, a distinction is made between selected instrument or no selection. In addition, a **cell click** in the correlation matrix may result in further functionality for the **rolling correlation**.

##### Function with selected correlation set
Existing **instruments** are added to a correlation set to which the **basic definition** is then applied.
- **Add existing instrument**: The functionality for adding an instrument can be accessed via the context menu. This opens a corresponding [search dialog for instruments](../instrument/searchdialog/). The selection of instruments is implicitly restricted by the selection of the other instruments in this correlation set. This means that the "**Trade from**" and "**Active until or Maturity date**" properties of the other selected instruments can restrict the selection.

##### Selected instrument functions
- **Remove instrument**: The selected instrument can be removed from the correlation matrix.
- **Hyperlink stock exchange and product**: See [security and derived instrument](../instrument/securityderived/).

##### Selected instrument and cell click function
**Clicking** in a **cell** of the correlation matrix expands the View menu or the context menu with the function for the rolling correlation graph of the two instruments. An existing rolling correlation graph can also be expanded with further correlations.

## Properties
{{% notice note "Limit of sampling period period "Daily return"" %}} 
A sampling period based on the **daily return** is not always meaningful. ETFs that track the same index are expected to have a correlation close to 1. Nevertheless, there can be considerable differences, for example with the S&P 500, which is offered by different providers in different regions. The time zones result in different trading times and therefore also different closing prices. This is not the case with **"Monthly return"**, where the different closing prices of the stock exchange have hardly any effect on the correlation. 
{{% /notice %}}

- **Date from** and **date to**: This can be used to define a time period for the correlation. Both or just a single date can be set.
- **Sampling period**:
  - With "**Annual return**", the calculations are based on the available **closing prices** on the first trading day of each **year**. The end-of-day prices of all relevant instruments are available for this date determined by the system.
  - For the "**monthly return**", the calculations are based on the available **closing prices** on the first trading day of each **month**. The end-of-day prices of all relevant instruments are available for this date determined by the system.
- **Rolling windows**: This property influences the **rolling correlation** in the graphical display.
- **Currency-adjusted**: If instruments with different currencies are available, the calculation can optionally be carried out with currency adjustment. The corresponding currency pairs are determined by the system and cannot be viewed by the user. This setting has no influence on possible currency pairs in the existing correlation matrix.

{{% notice note "Currency-adjusted and non-existent currency pairs" %}}
**Currency pairs** are or may be required so that the correlation matrix can be created currency-adjusted. If a required currency pair is not available to the system, it will be created by the system and the **historical exchange rate data** will be downloaded directly. This can delay the creation of the correlation matrix by a few seconds.
{{% /notice %}}

## Expanding table row
Click on the closed **expander symbol** to display the "Return and statistical data", see [Return and statistical data](./returnstatdata/).

## Rolling correlation
A desired rolling correlation can be displayed in the additional area. Further correlations can be added to an existing graph. Clicking the "**Save and calculate**" button also updates the rolling correlation.
- If a **date from** is defined and the corresponding closing prices are available, this is selected as the start date of the rolling correlation.

## Video correlation matrix and statistical data
Here you can see the correlation matrix in action:
Video will follow...