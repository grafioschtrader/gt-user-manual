---
title: "Instrumente search dialog"
date: 2026-08-13T22:54:47+01:00
draft: false
weight: 45
archetype: "default"
---
This dialog can be used to select existing instruments. There is a single and multiple selection:
- **Single selection**: Only a single **instrument** can be selected and transferred from the search result set. This is used, for example, when **importing transactions**.
- **Multiple selection**: **Several instruments** can be selected from the results of the search from a table. This multiple selection is used in the **watchlist** and **correlation matrix** for adding instruments.
- The search criteria are an AND operation and therefore an instrument found must fulfill all criteria.
## Instruments already contained
If the dialog is opened from a **watchlist** or a **correlation set**, the instruments already contained there are left out of the result table. An instrument can therefore not be added twice, and the table only shows what is still missing.
If the dialog instead serves to assign a **single** instrument, there is no collection to compare against and every matching instrument is offered every time. This concerns the base and the additional instruments of a [derived instrument](../securityderived/derivedinstrument/), the transaction import, the standing order and the ISIN change.
## Search criteria
All criteria that are filled in are combined with **and**, so an instrument that is found must fulfill every one of them. As long as not a single criterion has been entered, the **Search** button stays inactive. The input fields are described below grouped by topic; in the dialog itself they appear one below the other.
### ISIN
The **ISIN** identifies an instrument uniquely and is therefore matched exactly and not as a partial text. An incomplete ISIN consequently produces no hit. When the dialog is opened the cursor is placed in this field, because searching by ISIN is the most precise way. How this field behaves towards the remaining criteria is described under [Fields being shown and hidden](#fields-being-shown-and-hidden).
### Name and Ticker/symbol
These two fields search for the designation of the instrument when the ISIN is not known.
- **Name**: Any part of the name is searched for, upper and lower case does not matter. The entry "gold" therefore also finds "Goldpreis" and "Barrick Gold".
- **Name as regular expression**: With this option the name entered is no longer evaluated as a partial text but as a **search pattern**. This allows criteria to be expressed that are contained in the name itself, see [Name search with a search pattern](#name-search-with-a-search-pattern).
- **Ticker/symbol**: Any part of the symbol is searched for, whereby the entry is always converted to capital letters.
### Classification
The classification of an instrument narrows the result set considerably, even without knowing the name.
- **Asset class**: This restricts the search to an asset class such as equities or fixed-income securities. If **Currency pair** is chosen here, the dialog searches for currency pairs instead of securities; in that case the **Name** is matched against the two currency codes and not against an instrument name.
- **Sub-asset class**: This differentiates further within the asset class. The selection corresponds to the sub-classes recorded in the [asset class]({{% relref "/basedata/instrumentbased/assetclass" %}}).
- **Financial instrument**: This differentiates by the kind of instrument, for example direct investment, ETF or CFD.
### Stock exchange, currency and validity
These criteria concern the trading of the instrument.
- **Stock exchange**: This restricts the search to a single [stock exchange]({{% relref "/basedata/instrumentbased/stockexchange" %}}).
- **Currency**: This restricts the search to the currency of the instrument, in which its prices are kept.
- **Active at**: Instruments are searched for whose term covers the date entered. A bond that has already been repaid is therefore only found when a date within its term is chosen.
### Data sources
These two criteria compile all instruments of a particular data source, which is helpful for example before replacing a data source that no longer works.
- **Historical data source**: This restricts the search to the data source of the historical price data.
- **Intraday data source**: This restricts the search to the data source of the intraday prices.
### Holdings and visibility
These criteria do not refer to the instrument itself but to its relation to your client.
- **With active holding**: Only instruments for which your client currently holds a position are found. This is the fastest way to find an instrument from your own portfolio without knowing its name exactly. This option is needed for example for the [ISIN change]({{% relref "/basedata/securityaction/securityisinrename" %}}).
- **Privat security**: This selection field has three states. If it is not set, public instruments and your own private instruments are found. If it is set, only your own private instruments are found, and if it is explicitly cleared, only the public ones.
- **Levered Inverse**: The leverage factor is matched exactly, which is only useful for leveraged and inverse products.
## Use case: filling the performance watchlist
In the navigation area one of your watchlists is marked as the [performance watchlist]({{% relref "/watchlistinstrument/watchlist" %}}), and it should contain all of your open positions. The reason is the price update: the [performance view]({{% relref "/watchlistinstrument/watchlist/performance" %}}) of a watchlist is the only place where GT refreshes the intraday prices, and together with the instruments the currency pairs depending on them are updated as well. Whatever is not in that watchlist is not brought up to date during the day.
This search dialog is the quickest way to fill or complete that watchlist. Open the watchlist in question, choose **Add existing instrument**, set only the selection field **With active holding**, start the search without any further criterion, mark all rows of the result table and transfer them with **Add**. You do not have to know a single instrument name for this.
Because instruments already contained are no longer offered, the same search can be repeated at any time later, after new positions have been opened; only what is missing is then offered.
## Fields being shown and hidden
The dialog hides input fields as soon as another entry makes them pointless. A hidden field is not lost, it reappears as soon as the entry that caused it is withdrawn.
{{% notice note "ISIN excludes the remaining criteria" %}}
As soon as something is entered in the **ISIN**, all remaining criteria disappear, because the ISIN already identifies the instrument uniquely. Conversely the **ISIN** disappears as soon as one of the remaining criteria is entered. If the corresponding field is emptied again, the hidden fields reappear.
{{% /notice %}}
If the entry **Currency pair** is chosen under **Asset class**, all criteria that do not exist for a currency pair are hidden. This affects **Ticker/symbol**, **Privat security**, **Levered Inverse**, **Active at**, **Stock exchange**, **Financial instrument**, **Sub-asset class** and **Name as regular expression**. If a **Currency** is chosen in addition, the **Name** disappears as well, because the currency alone is already precise enough.
The **Search** button stays inactive as long as no actual criterion has been entered. The selection field **Name as regular expression** does not count as a criterion here, because it only determines how the **Name** is evaluated.
## Name search with a search pattern
If **Name as regular expression** is set, the **Name** no longer means "contains this text" but describes a pattern the name has to match. This allows searches that are not possible with an ordinary text search. The pattern is searched for at any position of the name, unless it is tied to the beginning with `^` or to the end with `$`. Upper and lower case is not distinguished here either.
The following examples show the most common cases:
- `Const.* software` finds **Constellation Software**, even when the exact spelling is not known. The character pair `.*` stands for any characters in between and thus replaces the uncertain part of the name.
- `^[23]([.,]\d+)?(?=\s)` finds bonds whose name begins with a coupon between 2 and 3.99 percent, for example **2.75 % Kanton Zürich 2019-2031**. Such ranges cannot be expressed with a text search.
- `^(Apple|Alphabet)` finds names that begin either with **Apple** or with **Alphabet**. The vertical bar separates alternatives.
- `ETF$` finds names that end with **ETF**.
If the pattern entered cannot be evaluated, because for example a bracket has not been closed, the search is rejected with an error message. You therefore do not get a silently empty result set. The search with a pattern is only available for securities and not for currency pairs.
Unfortunately only in German:
{{< youtube XIL4XNHsJM0 >}}
