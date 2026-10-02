---
title: "Dashboard"
date: 2026-09-08T15:00:00+01:00
draft: false
weight: 2
archetype: "default"
---
After sign-in, GT opens the main view on the **Dashboard**. It is the first root node of the **navigation tree**, on the same level as the portfolios. The question mark in the menu bar opens this page; the two following pages describe the individual widgets.

The dashboard is a personal overview: which messages and requests need attention, and how the instruments you hold and the client have moved over the last trading days. Each card summarises; where the full workflow is needed, **Open full view** leads to the existing screen. The arrangement of the cards belongs to the user, not to the client, and is included when you [export your personal data]({{% relref "/intro/settings" %}}).

{{% notice info %}}
An administrator can turn the dashboard off for the whole installation with `g.use.dashboard=false` in `application.properties`. The root node then disappears from the navigation tree, and after sign-in the previous main view is shown again.
{{% /notice %}}

## Layout of the view
Above the cards stand the title **Dashboard**, the **Active client** as the internal number of the client, and the buttons **Edit dashboard** and **Refresh all**. The time **Loaded at** applies to the whole view. Each card carries the same time for its own data, together with the widget title, the stored row or instrument count, and its own **Refresh** button.

The label **Simulation context** appears when you are not working in your own client, for example after switching to a [managed client]({{% relref "/tenantportfolio/client/managedclients" %}}) or in a simulation of the [rule-based strategies]({{% relref "/algoalert" %}}). The investment cards then follow the **active client**. Unread messages and data-change requests stay with the signed-in user, so a client switch does not put another person's inbox on the dashboard.

{{% notice note %}}
Opening the dashboard does not mark a message as read. Messages are marked as read only in the full view of the [messaging system]({{% relref "/admindata" %}}).
{{% /notice %}}

An empty dashboard asks you to choose **Edit dashboard** and add one of the available widgets. A widget that is no longer available with your current access stays hidden; the card can still remain in the stored arrangement.

## Editing the arrangement
**Edit dashboard** switches to draft mode; the same command is in the **Edit** menu once the dashboard is the active panel. In the draft the cards show no current figures, only the hint to save the layout so that the data can be loaded. On the right appears the **Widget catalogue** with the **Available widgets**. Each widget may appear at most once; if every available card is already on the dashboard, the catalogue says so.

Add a card by dragging it from the catalogue onto the grid or by choosing **Add widget**. Remove a card with **Remove** or by dragging it back into the catalogue; the catalogue remembers width and settings until this editing session ends. Change the order with the drag handle, **Move earlier** or **Move later**. The **Width** of a card is **One third**, **Half**, **Two thirds** or **Full width**. **Configure** opens the dialog with the widget's help text and the stored settings, for example the maximum number of rows.

**Save dashboard** applies the draft. **Cancel editing** discards it. **Reset to defaults** sets the draft to the default arrangement of the widgets currently available; only the subsequent save makes it take effect, **Cancel editing** keeps the previous arrangement. If you leave the view with unsaved changes, GT asks whether those changes should be discarded.

{{< mermaid >}}
graph TD
    A[View dashboard] --> B[Edit dashboard]
    B --> C[Add, remove, arrange or configure widgets]
    C --> D{Apply the draft?}
    D -->|Save dashboard| E[Layout is loaded]
    D -->|Cancel editing| A
    C --> F[Reset to defaults]
    F --> D
    E --> A
{{< /mermaid >}}

A dashboard holds at most 24 widgets, and each widget only once. A widget that you no longer see because of changed rights remains in the stored arrangement and still counts toward capacity. If capacity is exhausted, you must first remove cards or reset to the default arrangement.

If another session saves first, you are asked to reload the saved layout; your own draft remains under **Previous unsaved draft** for inspection. A layout with an unsupported version cannot be edited, so that the stored version is kept.

## Refreshing
**Refresh all** reloads every card. **Refresh** on a single card reloads only that card. If the refresh fails, the previously loaded data stay visible and the card notes that the refresh failed.

Settings you change on the card itself, such as the chosen trading day or the portfolio, are not part of the stored arrangement. An ordinary **Refresh** restores the stored defaults for those.

{{% notice note %}}
The cards **Biggest winners** and **Biggest losers** measure the price move of held instruments. The card **Last trading days** shows the period result of the client or of a portfolio. Those are two different questions; the figures need not add up to the same statement. The differences are described on the page [Widgets]({{% relref "/intro/dashboard/widgets" %}}).
{{% /notice %}}

## Which widgets you see
Which cards the catalogue offers depends on the [user role]({{% relref "/intro/userrights" %}}) and on whether a client already exists. Messages and data-change requests are available to every signed-in user. The investment cards appear only after a client has been set up. Additional cards for the **Administrator** role are described under [Widgets for administrators]({{% relref "/intro/dashboard/adminwidgets" %}}).
