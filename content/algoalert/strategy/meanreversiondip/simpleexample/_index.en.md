---
title: "Simple example"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 10
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.38.0" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
This example contains only what a [Mean Reversion Dip](../) strictly needs: an entry rule, a profit target, a stop-loss and the risk limits. Partial profits and averaging down are deliberately left out. It is a good starting point for your own experiments, for example in a [Historical replay](../../../historicalrun/). The values are editable example settings, not an investment recommendation.

In the dialog **Strategy definition**, replace the content of the YAML editor with the following configuration and save it with **Apply**.

```yaml
strategy_name: simple_dip
universe:
  direction: long_only
cooldowns:
  after_buy_days: 5
  after_sell_days: 5
  max_trades_per_asset_per_30d: 4
entry:
  lookback_T: 20
  dip_reference:
    type: price_T_ago
  dip_threshold_pct: -0.08
  initial_buy_sizing:
    mode: pct_portfolio
    pct: 0.02
profit_management:
  take_profit:
    mode: pct_gain
    pct: 0.06
    reference: avg_cost
downside_management:
  loss_action: A_sell_loss
  variant_A_sell_loss:
    stop_type: hard_stop
    stop_reference: avg_cost
    stop_threshold_pct: -0.08
risk_controls:
  max_position_exposure_pct: 0.05
  max_position_drawdown_pct: 0.15
  force_exit_on_risk_breach: true
```

## What the settings do
The strategy only buys (`long_only`). It compares every daily close with the close 20 observations, roughly four trading weeks, earlier. If the price is at least 8% lower, it proposes a purchase of 2% of net equity.

An open position is sold entirely as soon as it is 6% above the weighted average entry price. If it falls 8% below that price, the stop-loss also sells it entirely. An additional loss condition under `trigger` is not needed here, because the fixed stop alone decides the exit.

After a purchase or sale the strategy waits five calendar days before it proposes a new entry. Within 30 days at most four recorded transactions of this instrument are allowed. The risk limits act as a safety net: the position may make up at most 5% of net equity, and it is closed immediately on a loss of 15% or when a limit is breached.

## A run in figures
Net equity is 100,000, and the security traded at 100 twenty observations ago.

1. The price closes at 92, which is 8% lower. The strategy proposes a purchase of 2,000, roughly 21.7 units.
2. You record the purchase at 92 and select `simple_dip` in the **Strategy assignment**. The average entry price is now 92.
3. If the price rises to 97.52 or higher (92 plus 6%), the strategy proposes selling the entire position.
4. If the price instead falls to 84.64 or lower (92 minus 8%), the stop-loss proposes selling the entire position.
5. Once the sale is recorded, the five-day cooldown begins; afterwards a new entry is possible again.

The 15% limit is not reached in this run, because the stop at 8% fires earlier. It only takes effect when a single price jump crosses both thresholds at once, and the result is then the same: the entire position is sold.

## Extending it
The [example with partial profits](../partialprofitexample/) shows the options left out here. Every setting is described under [Effective settings](../#effective-settings).
