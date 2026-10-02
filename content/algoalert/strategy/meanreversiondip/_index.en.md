---
title: "Mean Reversion Dip"
date: 2026-09-30T12:00:00+02:00
draft: false
weight: 40
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress - Target version V0.38.0" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
The Mean Reversion Dip strategy evaluates completed daily closing prices and proposes an entry after a configured price move. It supports a full stop-loss exit, a full take-profit exit, forced exits on risk breaches, taking partial profit in steps and adding to a losing position. In the main portfolio, a recommendation or notification does not book a transaction.

Two examples show a complete configuration: the [simple example](./simpleexample/) is limited to entry, profit target, stop-loss and risk limits. The [example with partial profits](./partialprofitexample/) matches the template of the dialog and additionally contains a partial-profit plan and the settings for averaging down.

## Set up the strategy
The Mean Reversion Dip is assigned under **Rule-based Trading** to a security of the tree; it is not offered on the portfolio based strategy or on an asset class. The portfolio based strategy must be linked to a watchlist, and the security must be part of that watchlist. Allocate each instrument to exactly one asset class with a weighting. The weightings of the asset classes and those of the securities within each asset class must each total 100%. This makes the asset class of the security tactical: its weighting is then only an upper limit, and a portfolio rebalancing no longer buys into it, not even for its other securities. Preferably put securities with a Mean Reversion Dip in an asset class of their own, see [Rebalancing and Mean Reversion Dip together](../../#rebalancing-and-mean-reversion-dip-together).

In the dialog **Strategy definition** select **Mean Reversion Dip** under **Strategy name**. The YAML editor is pre-filled with the [example with partial profits](./partialprofitexample/), and **Load Template** restores it. Keep **Active** selected for an executable strategy. Clear it to save a draft that still contains settings which cannot be executed yet. **Apply** saves the strategy. An active strategy is rejected when a required setting is missing or a setting is chosen that cannot be executed yet; the message names the setting concerned.

## Effective settings
The following tables describe every setting that influences a decision of the strategy. Shares are written as decimals: `0.03` means 3%, `-0.12` means a decline of 12%. Required settings are marked with an asterisk.

The instrument is not selected in the YAML; it is always the security the strategy is assigned to. Data source, execution type and selection mode are fixed: the strategy works on daily closing prices with market execution and without a fee or slippage model of its own. You can leave such settings out. Older configurations that still contain them remain valid.

### Name and direction
| Setting | Meaning | Values |
|---|---|---|
| `strategy_name`* | Name of the strategy. It is offered as a choice in the **Strategy assignment** when a transaction is recorded. | text, at most 100 characters |
| `universe.direction`* | Permitted trade direction. | `long_only`, `short_only`, `long_short` |

### Cooldowns
Cooldowns only restrict new entries; exits are never postponed.

| Setting | Meaning | Values |
|---|---|---|
| `cooldowns.after_buy_days`* | Calendar days after the last opening before a new entry is possible. | integer from 0 |
| `cooldowns.after_sell_days`* | Calendar days after the last reduction before a new entry is possible. | integer from 0 |
| `cooldowns.max_trades_per_asset_per_30d`* | Maximum number of recorded transactions of the instrument within 30 days, including partial fills. | integer from 1 |

### Entry
| Setting | Meaning | Values |
|---|---|---|
| `entry.lookback_T`* | Number of observations over which the price move is measured. | 1 to 999 |
| `entry.dip_reference.type`* | Reference price: the price T observations ago, the highest (for short the lowest) prior close of a window, or a moving average. | `price_T_ago`, `highest_in_window`, `moving_average` |
| `entry.dip_reference.period` | Length of the window or of the moving average. Not needed for `price_T_ago`. | 1 to 999 |
| `entry.dip_reference.indicator` | Kind of moving average, only for `moving_average`. | `SMA`, `EMA` |
| `entry.dip_threshold_pct`* | Minimum move against the reference price. A decline for long, a corresponding rise for short. | between -1 and 0, e.g. `-0.08` |
| `entry.initial_buy_sizing.mode`* | Entry size as a share of net equity or as a fixed amount in the tenant currency. | `pct_portfolio`, `absolute_amount` |
| `entry.initial_buy_sizing.pct` | Share of net equity for `pct_portfolio`. | above 0 up to 1 |
| `entry.initial_buy_sizing.amount` | Amount for `absolute_amount`. | above 0 |

### Profit taking
The whole `profit_management` section is optional. Without it the strategy takes no profit and closes a position only through loss or risk rules.

| Setting | Meaning | Values |
|---|---|---|
| `take_profit.mode` | Profit target as a percentage gain or as a profit amount in the tenant currency. When it is reached, the entire remaining position is closed. | `pct_gain`, `absolute_profit` |
| `take_profit.pct` / `take_profit.profit_amount` | Size of the profit target matching the mode. | above 0 |
| `take_profit.reference` | Reference price: weighted average entry price or initial entry price. | `avg_cost`, `initial_entry_price` |
| `scale_out_enabled` | Switches the partial-profit plan on. | `true`, `false` |
| `sell_fraction_basis` | What the shares refer to, see [Take partial profit](#take-partial-profit). Required when the plan is switched on. | `current_position`, `initial_position` |
| `scale_out_plan` | List of steps. Each step has a short name `id`, a trigger `trigger` and either a share `sell_fraction` or `sell_remainder: true` for the last step. | at most 20 steps |

### Loss handling
| Setting | Meaning | Values |
|---|---|---|
| `downside_management.loss_action`* | Chooses what happens on a loss: sell or add. This setting alone selects the variant. | `A_sell_loss`, `B_average_down` |
| `variant_A_sell_loss.stop_type`* | Fixed stop at a price threshold, or a stop confirmed by indicator rules. Required for `A_sell_loss`. | `hard_stop`, `indicator_stop` |
| `variant_A_sell_loss.stop_reference`* | Reference price of the stop. Required for `A_sell_loss`. | `avg_cost`, `initial_entry_price` |
| `variant_A_sell_loss.stop_threshold_pct`* | Adverse move at which the entire position is closed. Required for `A_sell_loss`. | between -1 and 0, e.g. `-0.10` |
| `trigger.down_reference`, `trigger.down_threshold_pct` | Additional loss condition. It may be omitted for a fixed stop; an indicator stop and averaging down require it. | as for the stop |
| `trigger.decision_basis` | How the loss condition is decided: price threshold only, indicator rules, z-score, or price threshold with both rule groups. | `simple_threshold`, `indicator`, `statistical`, `hybrid` |
| `trigger.indicator_rules`, `trigger.statistical_rules` | Rules for the decision bases `indicator`, `statistical` and `hybrid`. | `rsi`, `sma`, `ema`, `zscore` |
| `variant_B_average_down.*` | Settings for averaging down, see [Add to a losing position](#add-to-a-losing-position). Required for `B_average_down`. | |

### Risk limits
| Setting | Meaning | Values |
|---|---|---|
| `risk_controls.max_position_exposure_pct`* | Maximum share of the position in net equity. A larger entry or addition is blocked. This limit always applies; no setting lifts it. | above 0 up to 1 |
| `risk_controls.max_position_drawdown_pct`* | Maximum loss of the position against the average entry price. | above 0 up to 1 |
| `risk_controls.force_exit_on_risk_breach`* | Closes the entire position as soon as a risk limit is breached. | `true`, `false` |

## Daily decisions
The strategy uses the last completed trading day before the current UTC day. It skips weekends and recorded exchange holidays; cryptocurrencies use calendar days. A missing expected close or insufficient indicator history produces an unavailable decision. It does not substitute a live quote.

A long entry compares the close with the price T observations ago, the highest prior close in a window, or a prior SMA/EMA. A short entry mirrors the movement and uses the lowest prior close for the window reference. The current close is excluded from these entry references. Short positions require a margin instrument. A margin recommendation also requires an existing transaction with a consistent value per point; different contract definitions for the same instrument make evaluation unavailable.

The hierarchy ceiling, bucket allocation, instrument allocation and position exposure limit all apply; hierarchy weights such as `50` mean 50%. Proposed entries reserve capacity for other strategies in the same evaluation. Short proceeds do not increase equity, and an exit proposal does not release capacity until a trade is recorded. An entry exceeding the remaining capacity is blocked rather than reduced automatically.

## Assign actual transactions
When recording a purchase or sale, select its **Strategy assignment**. Assign both openings and exits consistently, including existing open positions. Multiple strategies may hold the same instrument with separate assignments. Closed, unassigned historical trades do not block evaluation; unassigned open quantities do. A margin closing transaction inherits the assignment of its opening, and changing that opening also updates its closes.

Position quantity, weighted average entry price and cooldown dates are reconstructed from the assigned transactions, with stock splits taken into account. Editing or deleting trades therefore changes the next decision. Removing a strategy leaves the portfolio transactions intact and clears their assignment. Receiving a notification never advances fill state.

## Exits and cooldowns
If a risk limit is breached and the forced exit is switched on, a full close is proposed first. The hard stop is evaluated next, followed by the additional loss condition when one is present. Indicator and statistical rules use their configured comparisons; every rule within a group must pass. A hybrid downside condition requires the price threshold and both rule groups. A z-score cannot be calculated from constant prices and is reported as unavailable.

Baseline exits close the strategy's entire remaining quantity and take precedence over taking partial profit. An exit suppresses a conflicting entry for the same instrument in the same evaluation. Entry cooldowns count calendar days since the last opening or reduction fill. The rolling 30-day trade limit counts actual fill transactions, including partial fills.

## Take partial profit
Instead of closing a position only when it reaches its profit target, you can reduce it in several steps. Switch on partial sales in the take-profit section and describe a plan of individual steps. Each step carries its own short name, a condition and the share that is given back when the condition is met. Instead of a share, the last step may close the entire remaining quantity. A long position is sold, a short position is covered.

You choose what the shares refer to. Measured against the opening size, each share is a part of the quantity the position was opened with, and the shares together must not exceed the whole position. Measured against the current size, each share is a part of what is still open when that step fires. Two steps of 30% each therefore give back 30% first and then 30% of what remains.

A step is triggered by a percentage gain over the initial or the weighted average entry price, by a profit amount in the tenant currency, or by indicator rules. Steps are worked through in order and never skipped. When a single price jump reaches several targets at once, the steps concerned are combined into the same evaluation.

Each step executes at most once per position. What counts is only how much of the position has already been given back; receiving a notification never advances fill state. If you record only part of a proposal, the next evaluation proposes exactly the missing quantity again, and restarting the program changes nothing. Never more than what is actually open is proposed. If the position is closed and later reopened, the plan starts over. Executed targets with the same tranche name remain fixed; other progress is reconstructed from the configured plan and recorded reductions.

## Add to a losing position
Set `loss_action` to `B_average_down`. This setting alone selects averaging down; the settings of the sell-loss variant are then not evaluated. Averaging down requires the additional loss condition under `trigger`. Long positions add at lower prices; short positions add at higher prices. Every addition requires the adverse-price threshold. Indicator or statistical modes add their respective confirmations; hybrid requires both groups. Missing history never produces an addition.

Under `variant_B_average_down` you set the size of each addition (`add_sizing`, as for the entry), the maximum number of additions (`max_adds`) and the step rule (`add_step_rule`). With the initial entry as step reference, a 10% threshold and 5% step allow additions at 10%, 15% and 20% adverse movement. Only executed additions advance the ladder. With average cost as reference, each addition requires the configured step from the current weighted average, alongside the downside trigger. Indicator-based steps (`indicator_based`) use the indicator confirmation without a percentage ladder and require the decision basis `indicator` or `hybrid`. Custom steps cannot be activated. The average entry price is recalculated after every addition.

Parent allocations, position exposure limits, cooldowns and the rolling trade limit still apply. Oversized additions are blocked. A drawdown breach blocks additions even without forced liquidation; enabling forced liquidation requests a full exit. Profit-taking and exits have priority over additions, including conflicting recommendations from another strategy on the same instrument.

The first fill of an addition signal consumes one addition; further fills of that signal consume none. Unfilled proposals do not count. Each manual increase without a signal link counts separately. Partial fills of the original entry retain their opening identity. Closing and reopening resets the addition count.

Current-position profit tranches include added units when a new tranche triggers. Its target becomes fixed when the first fill executes; subsequent additions cannot enlarge that started tranche or repeat a completed one. Manual reductions consume outstanding tranches in order, based on holdings before their first reduction. Initial-position tranches retain the opening basis.

## Review the result
Open **Alert diagnostics** from the portfolio's alert view and select the tab **Trading decisions**. Which columns it shows, what the actions **Buy**, **Sell**, **Hold** and **Blocked** mean and when the table stays empty is described in [Alert diagnostics](../../alert/#trading-decisions). **Refresh** only reads results, **Evaluate now** recalculates them. Only **Buy** and **Sell** lead to a notification; its delivery is shown in the tab **Notifications**.
The column **Reason** explains every decision. The following table lists all reasons together with the action under which they appear. With **Blocked**, some reasons resolve themselves, for example a cooldown; the others require you to complete the configuration or the data.

| Reason | Action | Meaning and remedy |
|---|---|---|
| **Dip entry condition met** | Buy or Sell | The configured price move has occurred. An entry of the size of the initial buy is proposed, as a sale for a short position. |
| **Add to the existing position** | Buy or Sell | The losing position is to be increased, see [Add to a losing position](#add-to-a-losing-position). |
| **Take profit: close position** | Buy or Sell | The profit target has been reached and the whole position is to be closed. |
| **Take partial profit: reduce position** | Buy or Sell | A profit tranche has been reached, see [Take partial profit](#take-partial-profit). |
| **Take partial profit: close the remaining position** | Buy or Sell | The last profit tranche closes the remaining position. |
| **Stop-loss: close position** | Buy or Sell | The stop-loss has been reached. |
| **Downside condition: close position** | Buy or Sell | The exit condition of the loss handling is met, see [Loss handling](#loss-handling). |
| **Risk limit breached: close position** | Buy or Sell | A risk limit is breached and the forced exit is switched on, see [Risk limits](#risk-limits). |
| **Keep position** | Hold | The position is open and neither a profit nor a loss condition is met. |
| **Entry condition not met** | Hold | There is no position, and the price move is not yet large enough for an entry. |
| **Adverse move or confirmation has not triggered** | Hold | When averaging down, the adverse price threshold or the confirmation by an indicator is still missing. |
| **Waiting for the next adverse-price step** | Hold | Since the last addition the price has not yet moved by the next step. |
| **Entry cooldown or trade limit reached** | Blocked | A cooldown after a purchase or sale is still running, or the maximum number of transactions within 30 days has been reached, see [Cooldowns](#cooldowns). The block ends by itself. |
| **Insufficient investment capacity** | Blocked | The entry would exceed the weighting of the portfolio based strategy, the asset class or the security, or the maximum share of the position in net assets. Increase the weighting or reduce the initial buy. |
| **Both entry directions triggered** | Blocked | Long and short entry are met at the same time, so GT proposes neither. |
| **An exit is pending for this instrument** | Blocked | Another strategy proposes an exit from this security on the same day. A simultaneous entry would undo that exit. |
| **Risk controls block an addition** | Blocked | The addition would breach a risk limit. |
| **Maximum executed additions reached** | Blocked | The configured number of additions has been used up. |
| **Assign existing fills to their strategies first** | Blocked | An open quantity of the security has no **Strategy assignment**, see [Assign actual transactions](#assign-actual-transactions). |
| **Instrument is outside the hierarchy watchlist** | Blocked | Add the security to the watchlist that the portfolio based strategy is linked to. |
| **Instrument has more than one allocation** | Blocked | The security is part of more than one asset class. Allocate it to exactly one. |
| **Complete the active allocation and its weights** | Blocked | The security is not allocated to an asset class, a weighting is missing, or the weightings do not each total 100%, see [Set up the strategy](#set-up-the-strategy). |
| **Position cost basis is missing** | Blocked | No cost basis can be determined for the open position, so gain and loss are unknown. |
| **Short entries require a margin instrument** | Blocked | A short entry is only possible with a margin instrument. |
| **A consistent margin contract definition is required** | Blocked | The margin instrument has no existing transaction, or its transactions use different values per point. |
| **Completed daily closing price is missing** | Blocked | There is no closing price for the last completed trading day. Check the historical prices of the security. |
| **Insufficient price history** | Blocked | There are too few closing prices for the entry reference or an indicator. |
| **Invalid or future market observation** | Blocked | A historical price is invalid or lies in the future. |
| **Statistical rule unavailable: constant prices** | Blocked | All prices of the window are equal, so a statistical rule cannot be calculated. |
| **Indicator inputs are unavailable** | Blocked | An indicator used in the configuration cannot be calculated from the available prices. |
| **Historical currency conversion is missing** | Blocked | An exchange rate for the conversion into the currency of the client is missing. |

If the column shows a technical text, the strategy could not be evaluated for another reason; the message is shown unchanged.

How the strategy would have decided in the past is shown by a [Historical replay](../../historicalrun/) in a simulation environment. There the fee models of the securities accounts apply. Averaging down uses the same daily-close decision and fill interfaces.
