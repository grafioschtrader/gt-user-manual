---
title: "Security without price data"
date: 2026-06-05T10:54:47+01:00
draft: false
weight: 8
archetype: "default"
---
No public prices are provided for certain securities such as fixed-term deposits. For this purpose, there is "**Historical prices for period**". This **tab** is automatically activated when editing the **security** if a **stock exchange** without price data is selected. If the price does not change over the **entire term**, it corresponds to the **denomination**.

## Historical prices for period" tab
If the rate has fluctuated, it can be entered under this **tab**, where the rate and the date for the new rate are entered.

A period is added by entering the **Date for new price** and the price and then clicking **Apply**. A price is valid from its date until the next entered date or, if none follows, until the end of the term. Over the entire term the **denomination** is used as the price by default, so only a deviating price needs its own period. Selecting a row lets you edit or delete the entry; an already-existing date overwrites the existing entry. The changes are only persisted when the **security** is saved.

{{% notice style="info" title="Maximum number of periods" %}}
Up to 20 periods per instrument can be entered by default. An administrator can adjust this limit via the global setting `gt.max.instrument.historyquote.periods`. The table footer shows the current count and the allowed maximum.
{{% /notice %}}

Additional information can be found in the [price data](../../../) video.

Unfortunately only in German:
{{< youtubestartend Zij10hSO9OQ 96 220 >}}
