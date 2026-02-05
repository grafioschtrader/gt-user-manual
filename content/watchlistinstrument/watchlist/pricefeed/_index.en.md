---
title: "Quote data feed view"
date: 2021-05-01T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
This watchlist is primarily used to monitor **historical** and **intraday price data**.

## Functions
The specific functions of this **watchlist view** require the **user rights** of a **privileged user**, otherwise they will not be offered.

### Problems with price data
It is not always clear whether a connector for a corresponding instrument is no longer functioning or whether it is no longer being traded. In either case, one or both repeat counters will reach their limit. ETFs in particular tend to disappear from the market, in which case the "**Active until or maturity date**" should be adjusted accordingly.

#### Dialog "Add price data problems" 
This allows instruments whose **repetition counter** has reached the **limit** to be added to an **empty watchlist**. If both the intraday and historical price data for a security are no longer updated, this is an indication that this security may no longer be traded. If this is not the case, you can use the following two functions, for example, to set the repetition counters to 0 with a successful update of the intraday or historical price data.
- The security must still be trading, i.e.**"Active until or maturity date**" must be younger than the current date. This restriction does not apply to currency pairs.
- **Days since last successful**: The connector must have last worked between the current date minus this number of days. This can help to minimize the number of instruments found.

#### Repairing historical data
An attempt is made to update the **historical price data** of all instruments in the watchlist whose historical **repetition counter** is **greater than 0**. The limit 'gt.history.retry' of the "**Global settings"** is applied, for further information see [Global settings](../../../admindata/globalsettings). This function ignores this limit of possible attempts.

#### Repairing intraday data
An attempt is made to update the **intraday price data** of all instruments in the watchlist whose intraday **retry counter** is **greater than 0**. The limit 'gt.intra.retry', for more information see [Global settings](../../../admindata/globalsettings), of possible attempts is ignored.

## Expanding table row
The expanded table row shows almost all the properties of the instrument. The display is grouped for a better overview. Depending on the instrument, both the properties displayed and their grouping may vary.
