---
title: "EOD as line chart"
date: 2026-01-19T22:54:47+01:00
draft: false
weight: 30
archetype: "default"
---
In GT, the **end-of-day prices** of an **instrument** can be displayed in the **additional area** as a **chart**. In addition to the classic **line chart**, **candlestick charts** and **OHLC charts** are also available, provided the corresponding price data is present. This allows the performance of the prices from a selectable date of an instrument to be read off. The functionality offered differs slightly with regard to the **security** including the **derived security** and the **currency pair**.

{{% notice info %}} 
The line graph is based on the historical price data plus the last intraday price if available. 
{{% /notice %}}

{{% notice warning %}}
### Price chart or performance chart
As mentioned in the [glossary](../../glossar/), a distinction must be made between ETFs or funds **that distribute** or **reinvest** **dividends** and between **price** and **performance indices**. There is a similar difference between **shares** with or without **dividends**.

GT offers a selectable currency for the problem of different currencies of instruments. However, the daily **closing prices** are used in the chart, which may or may not include the returns, depending on the instrument. Therefore, the user should always be aware of whether **distributing** or **accumulating** ETFs or price or performance indices are being compared in this chart.

### GT and this problem
With this version, GT does not have the appropriate qualitative data sources to accurately calculate the performance of an instrument. An implementation will be considered as soon as the returns can be reliably obtained from the data sources. 
{{% /notice %}}

## Activating this view
The **line chart** in the **additional area** can be activated from different views of the **main area**. In practice, it is probably mostly used by the watchlist functionality. However, there are also other views in GT with one instrument per table row, where the following functionality is offered via the context menu.
- **End of day data as line graph**: Creates a **new line chart** with the selected **instrument**.
- **Add as line graph**: The selected **instrument** is added to the **existing line chart**. This allows you to compare the performance of different instruments.

## Chart types
GT supports three different chart types for displaying price data. The selection of the **chart type** is only available when a **single instrument** is displayed and the required OHLC data (open, high, low, and close prices) is present.
- **Line chart**: The classic chart type that displays closing prices as a continuous line. This type is always available and is used by default.
- **Candlestick**: Shows a candle body for each trading day representing the range between open and close prices, as well as wicks for high and low prices. Rising prices are typically displayed in green, falling prices in red.
- **OHLC**: Similar to the candlestick chart, but the price data is displayed as horizontal bars. The left bar marks the opening price, the right bar marks the closing price.

When displaying **multiple instruments** simultaneously, the **line chart** is automatically used, as candlestick and OHLC charts are not suitable for comparing different instruments.

## Transactions in the line chart
When mapping transactions, a distinction is made between a **security** or **derived security** and a **currency pair**.

- **Security and derived security**: **Transactions** such as **buy**, **sell** and **interest/dividend** are shown as dots of the same size in the line chart.
- **Currency pair**: The diameter of the dot is a reflection of the total amount of the transaction. In addition, the transaction of the **reciprocal currency pair** is also shown. For example, the line chart of the EUR/USD currency pair also contains the transactions of the USD/EUR currency pair. The size of the dot is standardized as soon as the selection box **Percent** is marked and/or several instruments or currency pairs are displayed. Securities transactions with the currency pair involved are transformed accordingly into **buys** or **sells** of the currency in question.

## Holdings
For an **individual instrument**, the **position** can be viewed over the selected period. The scaling of the **Y-axis** of the **position** is shown on the **right-hand side**. On the left-hand side, as always, is the scaling of the data series of the rates with the price or the percentage. Of course, negative stocks can also be displayed. The stock display can be **switched on or off** in the **header** of the chart.

## Functions
In general, a [chart has a certain basic functionality](../../intro/userinterface/user_setting_ui_controls/) that is not described further here. In addition, this chart has an **interactive header** and a **context menu** for rudimentary technical analysis.

### Interactions in the header
This chart offers additional interaction options in the **header**:
- **Date from**: The price data is displayed from this date. There are **buttons for different time periods** for quick adjustment.
- **Chart type**: Allows selection between line chart, candlestick chart, and OHLC chart. This option only appears when displaying a single instrument with available OHLC data.
- **Percent**: As soon as two or more instruments are displayed as a line chart, it only makes sense to display the chart with absolute values in a few cases. This is why there is this checkbox, which is automatically selected if the "**Add as line graph**" function is executed for the first time. The system does not override the state of this checkbox set by the user. The percentage display of the different instruments from a certain date allows a better comparison of the exchange rate changes.
- **Connect gaps**: If this checkbox is selected, the existing course data is connected with a line. If the checkbox is not selected, the possible gaps in missing price data are displayed. Missing price data is determined using the trading calendar of the corresponding instrument.
- **Currency**: GT supports instruments in different currencies. The currency must therefore be taken into account when comparing the performance of instruments. Based on the **client's currency**, any currency of the displayed instruments can be selected. The choice of currency has no effect on the line graph of the **currency pairs**.

{{% notice note "Currencies and non-existent currency pairs" %}} 
Currency pairs are required for the **currency** function to work. If a required currency pair is not available to the system, it will be created by the system and the **historical exchange rate data** will be **downloaded** directly. This may delay the creation of the line chart by a few seconds. 
{{% /notice %}}

### Context menu for technical indicators
GT supports various technical indicators that can be activated and deactivated via the context menu. The indicator parameters can be configured in a dialog.
#### Moving Averages (SMA/EMA)
The **simple moving average** (SMA) and the **exponential moving average** (EMA) are displayed directly on the price chart as overlays. By default, three periods can be configured.
#### Relative Strength Index (RSI)
The **RSI** is a momentum oscillator that measures the speed and magnitude of price changes. It oscillates between 0 and 100, with values above 70 typically indicating overbought conditions and values below 30 indicating oversold conditions. The RSI is displayed in a separate panel below the price chart and is only available when viewing a single instrument. By default, two periods can be configured.
{{% notice info %}}
GT will not be a tool for **technical analysis**, but some **technical indicators** will be implemented in the future.
{{% /notice %}}
### Drawing tools
The line chart offers interactive drawing tools for manual chart analysis. Various shapes can be drawn via the toolbar above the chart: lines, open and closed paths, circles, and rectangles. An eraser tool allows the removal of drawn elements. 

## End-of-day prices as line graph in practice
Unfortunately only in German:
{{< youtube qNaBNDIAnZ0 >}}
