---
title: "Widgets for administrators"
date: 2026-11-13T15:00:00+01:00
draft: false
weight: 20
aliases: ["/intro/dashboard/adminwidgets/"]
archetype: "default"
---
In addition to the [Widgets]({{% relref "/dashboard/widgets" %}}) of every role, a user whose most privileged role is **Administrator** sees the following card. The **Privileged user** role does not receive it. If the administrator has no client yet, the investment cards are absent as for the other roles; the administrator card remains visible.

## Limit-change requests
The card collects requests that need an administrator. On the one hand these are requests in which a user asks for a change of their daily limit. On the other hand they are unlock requests of users who were logged out for too many violations. Both kinds share one table, the newest first. The full workflow is described in the section [Change Proposals]({{% relref "/admindata/user" %}}#change-proposals) of the user settings. The stored setting **Maximum rows** limits the table, defaulting to five rows and allowing 1 to 20.

The heading names the number of open requests. Below it stands how many **Affected users** are behind them. The table contains **Nickname**, **Creation time**, **information object**, **Requested daily limit**, **Valid until** and **Remark data change**. If there are no entries, **No pending items.** is shown.

For an unlock request, **information object** names the reason for the logout, such as **Too many violations of the request limit** or **Too many violations of external data/data limit**. **Requested daily limit** and **Valid until** stay empty; the justification of the user stands in **Remark data change**.

The **Creation time** cell has a red background so that new requests stand out. On the day of creation the red is strongest. It becomes weaker with every day and disappears completely after five days; older requests have no coloured background any more.

**Open full view** leads to [user administration]({{% relref "/admindata/user" %}}). There a limit-change request appears in column **L** of the user table, an unlock request in column **U**. A limit-change request is an indicator, not proof that the user is already blocked; blocking after repeated violations is a separate procedure on the same page. An unlock request, by contrast, requires that the user has already been logged out.
