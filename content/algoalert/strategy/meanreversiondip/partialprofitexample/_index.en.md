---
title: "Example with partial profits"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.40.0" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
**Load Template** in the dialog **Strategy definition** inserts this configuration. Besides entry, profit target and stop-loss it contains a partial-profit plan that is switched off, and the settings for averaging down, which only take effect with `loss_action: B_average_down`. Each of them can therefore be tried with a single change. The values are editable example settings, not an investment recommendation. A shorter configuration without these additions is shown by the [simple example](../simpleexample/).

```yaml
strategy_name: daily_dip_with_stop
universe:
  direction: long_only
cooldowns:
  after_buy_days: 2
  after_sell_days: 2
  max_trades_per_asset_per_30d: 10
entry:
  lookback_T: 10
  dip_reference:
    type: price_T_ago
  dip_threshold_pct: -0.12
  initial_buy_sizing:
    mode: pct_portfolio
    pct: 0.03
profit_management:
  # Partial profit taking. Switch scale_out_enabled on to give the position back in the tranches below;
  # each of them executes at most once per position, and the last one closes whatever is left.
  scale_out_enabled: false
  sell_fraction_basis: current_position
  scale_out_plan:
    - id: t1
      trigger:
        type: pct_gain
        value: 0.04
        reference: avg_cost
      sell_fraction: 0.33
    - id: t2
      trigger:
        type: pct_gain
        value: 0.07
        reference: avg_cost
      sell_remainder: true
  take_profit:
    mode: pct_gain
    pct: 0.10
    reference: avg_cost
downside_management:
  # Additional loss rule. A hard stop does not need it; indicator confirmation and averaging down do.
  trigger:
    down_reference: avg_cost
    down_threshold_pct: -0.10
    decision_basis: simple_threshold
  # The loss action alone selects the variant: A_sell_loss sells at the stop, B_average_down buys more instead.
  loss_action: A_sell_loss
  variant_A_sell_loss:
    stop_type: hard_stop
    stop_reference: avg_cost
    stop_threshold_pct: -0.10
  variant_B_average_down:
    add_sizing:
      mode: pct_portfolio
      pct: 0.01
    max_adds: 3
    add_step_rule:
      type: each_n_pct_drop
      reference: initial_entry_price
      drop_pct_step: 0.05
risk_controls:
  max_position_exposure_pct: 0.10
  max_position_drawdown_pct: 0.25
  force_exit_on_risk_breach: true
```

## As shipped
The strategy enters after a decline of at least 12% over ten observations and invests 3% of net equity. When the position is 10% above the weighted average entry price, it is sold entirely. When it is 10% below, the stop-loss closes it entirely. The additional loss condition under `trigger` uses the same threshold and does not change this behaviour; it is the basis for averaging down described below. After every transaction a two-day cooldown applies to new entries, and within 30 days at most ten transactions of the instrument are allowed. The position may make up at most 10% of net equity; it is closed on a loss of 25% or when a limit is breached.

## Switch on partial profits
With `scale_out_enabled: true` the plan takes effect. When the position is 4% above the average entry price, step `t1` sells a third of the quantity still open. At 7%, step `t2` sells the entire remainder. The 10% profit target is then only reached when a single price jump skips step `t2`; it remains as an upper bound. How steps, reference sizes and partially recorded proposals work together is described in the section [Take partial profit](../#take-partial-profit).

## Add instead of sell
With `loss_action: B_average_down` the strategy does not sell on a loss but adds to the position. The settings under `variant_A_sell_loss` are then no longer evaluated. Measured against the initial entry price, 1% of net equity is bought at a loss of 10%, 15% and 20%, at most three times. The first threshold comes from `trigger`, the following ones result from the 5% step. Once the loss against the average entry price reaches 25%, no further additions are proposed, and the forced exit closes the position. The details are described in the section [Add to a losing position](../#add-to-a-losing-position).
