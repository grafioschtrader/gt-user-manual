---
title: "Derived instrument"
date: 2026-08-13T22:54:47+01:00
draft: false
weight: 8
archetype: "default"
---
A derived instrument allows the price data of one or more securities to be calculated using a **formula** from at least one other instrument. A derived instrument is therefore always dependent on at least one other instrument. A derived instrument is assigned to a **stock exchange** and an **asset class** and can therefore be traded in GT.
## Price calculation
A derived instrument has no data source of its own, its prices come from the linked instruments. The historical prices are not recalculated on every query but stored as ordinary, calculated price rows. Charts, reports and statistics therefore work as they do for any other instrument. These prices cannot be edited or imported by hand, however; the corresponding menu items are missing for a derived instrument.
A day is only calculated when **every** linked instrument has a price for that day. If the price of one of them is missing, it is missing for the derived instrument as well; as soon as it is delivered, GT adds the missing day on its own. Conversely, if a price of an underlying instrument is deleted, the derived price of that day disappears too. Because the derived prices are always calculated after the underlying ones within a run, a newly created derived instrument may only receive its first intraday price in the next run.
## The editing dialog
The dialog is opened in a watchlist via **Add new derived instrument**. The three input fields below are what distinguishes it from an ordinary security; all remaining fields are described under [Security and derived instrument](../).
- **Base instrument (o)**: This field is mandatory. The instrument is chosen with the button next to the field, whereupon the [search dialog for instruments](../../searchdialog/) opens under the title **Set security**; the choice is transferred with **Assign selected**. If you choose a **security**, GT transfers its name, currency, asset class, stock exchange and the two date fields into the form, which can be adjusted afterwards. For a **currency pair** only the currency is set.
- **Pricing formula**: This field is optional and takes at most 255 characters. Without a formula the derived instrument takes over the price of the base instrument unchanged.
- **Additional Instrument**: For every variable **p**, **q**, **r** or **s** that occurs in the formula, such a field appears with the corresponding letter in brackets. If the variable is removed from the formula, the field disappears again.
There are no fields for data sources, and likewise no **ISIN** and no **ticker/symbol**; a derived instrument is addressed by its name.
## What can be used as a building block
Both **securities** and **currency pairs** can serve as the base and as an additional instrument. A further derived instrument is not permitted, which is why the search in this dialog offers no derived instruments. In total at most five instruments are involved, the base instrument and four additional ones.
Two additional variables must not point to the same instrument. The search dialog does not prevent you from choosing the same instrument again, see [Instruments already contained](../../searchdialog/#instruments-already-contained); the attempt only fails when saving.
Conversely, a derived instrument is a completely ordinary instrument everywhere else: it can be included in watchlists and correlation sets, and it can be traded.
## Formula
A formula consists of numbers, variables, mathematical and Boolean operators and possible functions. GT uses the framework [EvalEx - Java Expression Evaluator](https://github.com/uklimaschewski/EvalEx), therefore see the website for the possible functions.
### Variable the assignment to an instrument
A maximum of **5** variables with the letters **o, p, q, r, s** can be used in a formula. The variables must be assigned to a security. These variables are the placeholders for the corresponding **historical** or **intraday price**.
A variable has to stand on its own: `o * 2` and `(o + p) / 2` are recognised, `op` is not.
### Check when saving
If a formula is entered, it must contain the variable **o** as well as the variable of every additional instrument assigned. When saving, GT reads the formula and evaluates it once with the value 1 in all variables. A faulty formula is therefore rejected immediately, for example with the message "Formula must contain the variable "o"!" or "Variable "x" is unknown!".
Decimal numbers may be entered with the decimal separator of your language setting; GT converts them when saving and later shows them to you in your own notation again.
## Making a currency pair tradable
A [currency pair](../../currencypair/) cannot be traded directly in GT. A derived instrument whose base instrument is a currency pair and which has no formula produces a tradable instrument for it. The selection of the asset class is then limited to the entry for currency pairs.
## What a derived instrument cannot do
Neither **splits** nor **dividends** can be recorded on a derived instrument, and it carries no trading volume. A split of the underlying instrument takes effect through its prices anyway and does not have to be recorded a second time.
As long as a derived instrument refers to an instrument, that instrument cannot be deleted.
## Cannot be changed after saving
The **base instrument** and the **currency** are fixed after the first save. The **formula** and the additional instruments can still be changed, whereby GT recalculates the entire stored price history after each such change, see [Changing the connector and reloading the price data](../#changing-the-connector-and-reloading-the-price-data).
Only the creator of the instrument or a user with more extensive rights may change these fields; for everyone else they are displayed but locked.
## Derived instrument in practice
A practical example with a derived instrument. From one troy ounce of gold in USD to 100 grams of gold in CHF. A security, currency pair and a formula for calculating the historical and intraday rates are used.
The gold instrument quoted in USD per troy ounce serves as the **Base instrument (o)**, the currency pair USD/CHF as the **Additional Instrument (p)**. The formula is:
```
o * 3.2150746569 * p
```
The factor 3.2150746569 is 100 divided by 31.1034768 and thus the number of 100-gram units in one troy ounce; the multiplication with **p** converts the result from USD into CHF.
Unfortunately only in German:
{{< youtube iGJWAh55VkY >}}
Additional information can be found in the video of [price data](../../../).
