---
title: "Alert"
date: 2026-02-12T12:00:00+01:00
draft: false
weight: 30
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of alerts and rule-based trading is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
The alert system in GT informs the user when configured conditions on securities are met. Alerts can be created in two contexts: as standalone security alerts or as strategy alerts within a portfolio strategy.

## Standalone Security Alerts
Standalone alerts are created directly on a security in a watchlist without requiring a portfolio strategy. Via the context menu of a security in the watchlist, select **Add alert** to configure a new alert. These alerts are evaluated independently of the AlgoTop hierarchy and are suitable for simple monitoring of individual securities.

When creating a standalone alert, a security entry (AlgoSecurity) without a parent asset class is automatically created. The selected strategy is then assigned to this entry.

## Strategy Alerts
Within the hierarchical portfolio strategy (AlgoTop), strategies can be assigned at various levels. When the conditions of such a strategy are met, the system generates an alert. These alerts exist in the context of the overall portfolio strategy and take the hierarchical structure of asset classes and securities into account.

## Alert Overview (Tenant Alerts)
The overview of all alerts for a tenant is displayed in a tree table. At the top level, the securities (AlgoSecurity) appear, with the assigned strategies (AlgoStrategy) as child nodes below them. The **Context** column indicates whether it is a standalone alert or an alert from a portfolio strategy.

Each strategy has an activatable checkbox. When activated, the strategy will be considered during the next alert evaluation. This allows individual strategies to be temporarily deactivated without having to delete them. Strategies can also be edited or deleted via the context menu.

## Alert Evaluation
Alert evaluation in GT operates in two tiers.

**Tier 1 (event-driven)** evaluates simple price alerts immediately after intraday price data is updated. This includes the **Absolut price gain/lose alert**, the **Holdings percentage gain/lose price Alert** and the **Gain/loss warning in a period**. These alerts do not require historical price data and can therefore be computed immediately.

**Tier 2 (scheduled)** runs periodically as a background task and evaluates indicator-based alerts. This includes the **Moving average crossing alert**, the **RSI threshold alert** and the **Custom expression alert**. These alerts load historical price data as needed for computing technical indicators.

## Alert Lifecycle
When an alert condition is met, the system creates an alert record with details of the triggered condition. The user is then notified via GT's internal messaging system. The user can read the message, assess the situation and then independently decide on the desired action. GT does not execute automatic transactions.

## Deduplication
To avoid duplicate alert messages, the system checks before creating a new alert whether an alert has already been generated on the same day for the same security with the same message type. In this case, no new alert is created. This ensures that a user is not notified multiple times on the same day about the same condition.
