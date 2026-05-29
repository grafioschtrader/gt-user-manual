---
title: "Mean Reversion Dip"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 40
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
The Mean Reversion Dip strategy is a complex trading strategy based on the principle of buying on price declines. It is created via a YAML configuration and offers modular components for entry, profit management and loss management.

## Overview
The strategy operates on the following principle: when the price of a security falls by a configured threshold relative to a reference point, a buy signal is generated. After a position is opened, the strategy follows two possible paths — a profit path with staged selling (scale-out) and a loss path with loss limitation or averaging down.

{{< mermaid >}}
graph TD
    A["Entry: Price decline detected"] --> B{"Position opened"}
    B --> C{"Price rising?"}
    C -->|Yes| D["Profit Management:<br/>Staged selling"]
    D --> E["Take-Profit:<br/>Close position"]
    C -->|No| F{"Loss threshold<br/>reached?"}
    F -->|No| C
    F -->|Yes| G{"Variant?"}
    G -->|A| H["Stop-Loss:<br/>Sell position"]
    G -->|B| I["Averaging Down:<br/>Add to position"]
    I --> C
{{< /mermaid >}}

## YAML Configuration
The Mean Reversion Dip strategy is configured via the **Strategy Configuration (YAML)** field. The configuration consists of several modules, each defining an aspect of the strategy. Here is a complete example:

```yaml
strategy_name: "My Dip-Buying"
version: "1.0"
universe:
  mode: single_asset
  direction: long_only
data:
  price_field: close
execution:
  order_type: market
cooldowns:
  after_buy_days: 5
  after_sell_days: 3
entry:
  type: dip_buy
  lookback_T: 20
  dip_threshold_pct: 10.0
  dip_reference:
    type: price_T_ago
    period: 20
  initial_buy_sizing:
    mode: pct_portfolio
    pct: 2.5
profit_management:
  scale_out_enabled: true
  sell_fraction_basis: initial_position
  scale_out_plan:
    - id: "tranche_1"
      trigger:
        type: pct_gain
        value: 5.0
        reference: avg_cost
      sell_fraction: 0.25
    - id: "tranche_2"
      trigger:
        type: pct_gain
        value: 10.0
        reference: avg_cost
      sell_fraction: 0.50
      sell_remainder: true
  take_profit:
    mode: pct_gain
    pct: 15.0
    reference: avg_cost
downside_management:
  loss_action: A_sell_loss
  trigger:
    down_reference: avg_cost
    down_threshold_pct: 8.0
    decision_basis: simple_threshold
  variant_A_sell_loss:
    enabled: true
    stop_type: hard_stop
    stop_reference: avg_cost
    stop_threshold_pct: 10.0
    order_type: market
risk_controls:
  max_position_exposure_pct: 0.05
  max_position_drawdown_pct: 0.15
  force_exit_on_risk_breach: true
  block_entry_if_exposure_exceeded: true
outputs:
  emit_events: ["entry", "exit", "tranche"]
  track_metrics: ["return", "drawdown"]
```

## Configuration Modules in Detail
The following sections describe all available keywords, grouped by module.

### Root Level
At the top level of the YAML configuration, the strategy name and version are defined.

| Key | Type | Required | Description |
|---|---|---|---|
| `strategy_name` | Text | Yes | Name of the strategy (max. 100 characters). |
| `version` | Text | No | Version number of the configuration (max. 10 characters). |

### universe — Asset Universe
Defines which securities the strategy applies to and the trading direction.

| Key | Type | Description |
|---|---|---|
| `mode` | Choice | `single_asset` — Single security. `watchlist` — All securities in a watchlist. |
| `assets` | List | List of security identifiers (only relevant in `watchlist` mode). |
| `direction` | Choice | `long_only` — Buy positions only. `short_only` — Short sales only. `long_short` — Both directions. |

### data — Data Source
Configures the price data used.

| Key | Type | Description |
|---|---|---|
| `price_field` | Choice | Which price value is used: `close` (closing price), `last` (last traded price), `mid` (midpoint). |
| `timeframe` | Text | Timeframe for price data, e.g. `1d` (daily). |

### execution — Order Execution
Parameters for executing buy and sell orders.

| Key | Type | Description |
|---|---|---|
| `order_type` | Choice | `market` — Market order. `limit` — Limit order. |
| `slippage_model` | Text | Model for slippage calculation (optional). |
| `fees_model` | Text | Model for fee calculation (optional). |

### cooldowns — Cooldown Periods
Time restrictions between consecutive transactions.

| Key | Type | Description |
|---|---|---|
| `after_buy_days` | Integer | Minimum number of days to wait after a purchase (min. 0). |
| `after_sell_days` | Integer | Minimum number of days to wait after a sale (min. 0). |
| `max_trades_per_asset_per_30d` | Integer | Maximum number of transactions per security within 30 days (min. 1). |

### entry — Entry Module
The entry module defines the conditions for buying. The central parameter is the percentage price decline relative to a reference point.

| Key | Type | Description |
|---|---|---|
| `type` | Choice | Entry signal type: `dip_buy` (price decline), `breakout` (breakout), `indicator_signal` (indicator signal). |
| `lookback_T` | Integer | Lookback period in days for entry calculation. |
| `dip_threshold_pct` | Decimal | Percentage price decline that triggers the entry. |
| `dip_reference` | Object | Configuration of the reference point for decline calculation (see below). |
| `initial_buy_sizing` | Object | Configuration of buy size (see Position Sizing). |

#### entry.dip_reference — Decline Reference Point

| Key | Type | Description |
|---|---|---|
| `type` | Choice | `price_T_ago` — Price T days ago. `highest_in_window` — Highest price in lookback window. `moving_average` — Moving average. |
| `period` | Integer | Number of days for reference calculation. |
| `indicator` | Text | Indicator name (only for `moving_average`, e.g. `SMA` or `EMA`). |

#### Position Sizing (sizing)
This reusable object is used in `entry.initial_buy_sizing` and `downside_management.variant_B_average_down.add_sizing`.

| Key | Type | Description |
|---|---|---|
| `mode` | Choice | `pct_portfolio` — Percentage of portfolio value. `absolute_amount` — Fixed amount. |
| `pct` | Decimal | Percentage of portfolio (for `pct_portfolio`). |
| `amount` | Decimal | Absolute amount (for `absolute_amount`). |

### profit_management — Profit Management
Configures staged profit-taking and the final take-profit.

| Key | Type | Description |
|---|---|---|
| `scale_out_enabled` | Boolean | Enables staged partial selling. |
| `sell_fraction_basis` | Choice | Reference for sell fraction: `initial_position` (original position size) or `current_position` (current position size). |
| `scale_out_plan` | List | List of tranches for staged selling (see below). |
| `take_profit` | Object | Final take-profit configuration (see below). |

#### profit_management.scale_out_plan[] — Tranches
Each tranche defines a selling stage.

| Key | Type | Description |
|---|---|---|
| `id` | Text | Unique identifier for the tranche (e.g. `"tranche_1"`). |
| `trigger` | Object | Trigger condition (see Trigger Object). |
| `sell_fraction` | Decimal | Fraction of position to sell (0.0 to 1.0, e.g. `0.25` for 25%). |
| `sell_remainder` | Boolean | If `true`, sells the entire remaining position. |

#### Trigger Object
The trigger object is used in tranches and in take-profit.

| Key | Type | Description |
|---|---|---|
| `type` | Choice | `pct_gain` — Percentage gain. `absolute_profit` — Absolute profit amount. `indicator` — Indicator-based. |
| `value` | Decimal | Threshold value for the trigger. |
| `reference` | Choice | Reference price: `avg_cost` (average cost basis) or `initial_entry_price` (first entry price). |

#### profit_management.take_profit — Final Take-Profit

| Key | Type | Description |
|---|---|---|
| `mode` | Choice | Trigger type: `pct_gain`, `absolute_profit` or `indicator`. |
| `pct` | Decimal | Profit target in percent (for `pct_gain`). |
| `profit_amount` | Decimal | Profit target as absolute amount (for `absolute_profit`). |
| `reference` | Choice | Reference price: `avg_cost` or `initial_entry_price`. |
| `action` | Text | Action when reached (e.g. `"sell_all"`). |

### downside_management — Downside Management
Configures the behavior when prices fall after entry.

| Key | Type | Description |
|---|---|---|
| `loss_action` | Choice | Selected variant: `A_sell_loss` (stop-loss) or `B_average_down` (averaging down). |
| `trigger` | Object | Conditions that activate downside management (see below). |
| `variant_A_sell_loss` | Object | Stop-loss configuration (Variant A). |
| `variant_B_average_down` | Object | Averaging down configuration (Variant B). |

#### downside_management.trigger — Loss Trigger

| Key | Type | Description |
|---|---|---|
| `down_reference` | Choice | Reference price: `avg_cost` or `initial_entry_price`. |
| `down_threshold_pct` | Decimal | Percentage loss that activates downside management. |
| `decision_basis` | Choice | Decision basis: `simple_threshold` (simple threshold), `indicator` (indicator-based), `statistical` (statistical) or `hybrid` (combined). |
| `indicator_rules` | List | Indicator-based rules (for `indicator` or `hybrid`). |
| `statistical_rules` | List | Statistical rules (for `statistical` or `hybrid`). |

#### Indicator and Statistical Rules
Objects in `indicator_rules` and `statistical_rules` have the following structure:

| Key | Type | Description |
|---|---|---|
| `id` | Text | Unique identifier for the rule. |
| `type` | Choice | Indicator type: `rsi`, `zscore`, `sma` or `ema` (for indicator rules). Free text for statistical rules. |
| `params.length` | Integer | Calculation period (min. 1). |
| `params.condition` | Text | Comparison condition (max. 10 characters, e.g. `"<"`, `">"`, `"<="`, `">="`) |
| `params.value` | Decimal | Threshold value for the condition. |
| `params.lookback` | Integer | Lookback period (min. 1). |

#### downside_management.variant_A_sell_loss — Stop-Loss

| Key | Type | Description |
|---|---|---|
| `enabled` | Boolean | Enables the stop-loss variant. |
| `stop_type` | Choice | `hard_stop` — Fixed stop at threshold. `indicator_stop` — Indicator-based stop. |
| `stop_reference` | Choice | Reference price: `avg_cost` or `initial_entry_price`. |
| `stop_threshold_pct` | Decimal | Percentage loss that triggers the stop. |
| `order_type` | Choice | `market` or `limit`. |
| `action` | Text | Action (e.g. `"sell_all"`). |

#### downside_management.variant_B_average_down — Averaging Down

| Key | Type | Description |
|---|---|---|
| `enabled` | Boolean | Enables the averaging down variant. |
| `add_sizing` | Object | Position size for each additional purchase (see Position Sizing above). |
| `max_adds` | Integer | Maximum number of additional purchases (min. 1). |
| `add_step_rule` | Object | Rule for additional purchase steps (see below). |
| `recalculate_avg_cost` | Boolean | If `true`, recalculates the average cost basis after each additional purchase. |

#### add_step_rule — Additional Purchase Steps

| Key | Type | Description |
|---|---|---|
| `type` | Choice | `each_n_pct_drop` — Purchase on each further N% decline. `indicator_based` — Purchase on indicator signal. `custom` — Custom rule. |
| `drop_pct_step` | Decimal | Percentage price decline per additional purchase step (for `each_n_pct_drop`). |
| `reference` | Choice | Reference price: `avg_cost` or `initial_entry_price`. |

### risk_controls — Risk Controls
Overarching limits to protect against excessive risk.

| Key | Type | Description |
|---|---|---|
| `max_position_exposure_pct` | Decimal | Maximum exposure per position as fraction of portfolio (0.0 to 1.0, e.g. `0.05` for 5%). |
| `max_position_drawdown_pct` | Decimal | Maximum allowed drawdown per position (0.0 to 1.0, e.g. `0.15` for 15%). |
| `force_exit_on_risk_breach` | Boolean | If `true`, closes the position immediately when risk limits are breached. |
| `block_entry_if_exposure_exceeded` | Boolean | If `true`, no new positions are opened when the exposure limit is reached. |
| `block_add_if_exposure_exceeded` | Boolean | If `true`, no additional purchases are executed when the exposure limit is reached. |

### outputs — Outputs
Configures which events and metrics the strategy produces.

| Key | Type | Description |
|---|---|---|
| `emit_events` | List | List of event types to log (e.g. `["entry", "exit", "tranche", "stop_loss"]`). |
| `track_metrics` | List | List of metrics to track (e.g. `["return", "drawdown", "sharpe"]`). |

{{% notice style="info" title="Note" %}}
This strategy is configured via YAML. In future versions, a visual editor with syntax highlighting will be available to simplify configuration.
{{% /notice %}}
