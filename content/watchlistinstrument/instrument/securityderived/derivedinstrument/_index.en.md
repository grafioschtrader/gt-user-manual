---
title: "Derived instrument"
date: 2022-04-10T22:54:47+01:00
draft: false
weight: 8
archetype: "default"
---
A derived instrument allows the price data of one or more securities to be calculated using a **formula** from at least one other instrument. A derived instrument is therefore always dependent on at least one other instrument. A derived instrument is assigned to a **stock exchange** and an **asset class** and can therefore be traded in GT.

## Formula
A formula consists of numbers, variables, mathematical and Boolean operators and possible functions. GT uses the framework [EvalEx - Java Expression Evaluator](//github.com/uklimaschewski/EvalEx), therefore see the website for the possible functions.

### Variable the assignment to an instrument
A maximum of **5** variables with the letters **o, p, q, r, s** can be used in a formula. The variables must be assigned to a security. These variables are the placeholders for the corresponding **historical** or **intraday price**.

## Derived instrument in practice
A practical example with a derived instrument. From one troy ounce of gold in USD to 100 grams of gold in CHF. A security, currency pair and a formula for calculating the historical and intraday rates are used.

Unfortunately only in German:
{{< youtube iGJWAh55VkY >}}

Additional information can be found in the video of [price data](../../../).
