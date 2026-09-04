---
title: "Shared access – basics"
date: 2026-06-15T22:54:47+01:00
draft: false
weight: 5
archetype: "default"
---
Until now a user was tied to exactly one **client** and saw only their own **portfolios**. Beyond that, Grafioschtrader offers two related functions that build on the same access model and both work through the **Client** menu and a read-only mode:
- [Managing multiple clients]({{< relref "/tenantportfolio/client/managedclients" >}}): an advisor creates several **separate** clients, maintains them and switches between them.
- [Share read access to your portfolio]({{< relref "/tenantportfolio/client/sharereadaccess" >}}): an owner gives another person read-only access to their **own** portfolio.

This page describes what the two functions have in common. Their specifics are on the two linked pages.

## The **Client** menu
As soon as the function is enabled for the Grafioschtrader installation, an additional **Client** menu appears in the top menu bar after the **Settings** menu. Which entries it contains depends on the role and on whether you are currently in your own or in someone else's client.

| Entry | Shown to |
| --- | --- |
| **Create client** | advisor (at least **User without limits**) |
| **Switch to client** | any non-read-only user with somewhere to switch to |
| **Share read access** | any non-read-only user, in their own client |
| **Shared viewers** | any non-read-only user, in their own client |
| **Back to my client** | while inside someone else's client (including read-only) |

## Switching and returning
**Switch to client** opens the **My clients** dialog, which lists every client you may access – both those you manage and those shared with you. The client you are currently in does not appear in the list. After a click on the switch symbol the application reloads and shows the chosen client's portfolio. With **Back to my client** you return to your own **client** at any time.

## Read-only mode
Whether someone may only read a **client** is decided afresh on every request, so that revoking access takes effect immediately. In read-only mode the actions for creating, editing and deleting are not shown in the menus at all, and the **Client** menu stays hidden apart from the **Back to my client** entry. Independently of the interface, the server additionally rejects every change attempt, so that the data remains protected in any case. Control of your own account, on the other hand, remains in your hands: the password and the language can be changed at any time via the **Settings** menu.

{{< mermaid >}}
graph LR
    A[Advisor] -->|manages separate| K1[Client A]
    A -->|manages separate| K2[Client B]
    O[Owner] -->|shares their own| P[Portfolio]
    P -->|read access| R[Reader]
{{< /mermaid >}}

{{% notice note %}}
The **Client** menu only appears when the function has been enabled for the Grafioschtrader installation. If it is not available, the operator has not activated it.
{{% /notice %}}
