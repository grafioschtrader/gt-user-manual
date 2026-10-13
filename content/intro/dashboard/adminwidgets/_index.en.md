---
title: "Widgets for administrators"
date: 2026-09-08T15:00:00+01:00
draft: false
weight: 20
archetype: "default"
---
In addition to the [Widgets]({{% relref "/intro/dashboard/widgets" %}}) of every role, a user whose most privileged role is **Administrator** sees the following card. The **Privileged user** role does not receive it. If the administrator has no client yet, the investment cards are absent as for the other roles; the administrator card remains visible.

## Limit-change requests
The card collects requests in which a user asks for a change of their daily limit and needs an administrator for that. The full workflow is described in the section [Change Proposals]({{% relref "/admindata/user" %}}#change-proposals) of the user settings. The stored setting **Maximum rows** limits the table, defaulting to five rows and allowing 1 to 20.

The heading names the number of open requests. Below it stands how many **Affected users** are behind them. The table contains **Nickname**, **information object**, **Requested daily limit**, **Valid until**, **Remark data change** and **Creation time**. If there are no entries, **No pending items.** is shown.

**Open full view** leads to [user administration]({{% relref "/admindata/user" %}}). There a limit-change request appears in column **L** of the user table. Such a request is an indicator, not proof that the user is already blocked; blocking after repeated violations is a separate procedure on the same page.
