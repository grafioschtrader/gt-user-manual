---
title: "Update Grafioschtrader"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 70
archetype: "default"
---
Due to the architecture of GT, installation is somewhat time-consuming. However, importing new versions is very simple. The existing data is automatically migrated to the new version. Further information can be found under["Update GT](https://github.com/grafioschtrader/grafioschtrader/wiki/Updating-GT)". Implementation and notes on the update
- How to update GT with a shell script
- How GT handles versioning
- Is your previous data safe?

Unfortunately only in German:
{{< youtube l2etk4rNcfk >}}

## Existing data after the time-zone correction
The time-zone correction prevents new date shifts. Values stored incorrectly by older versions are not automatically repaired using your time zone. Compare affected transactions, transfers and corporate actions with your statements. If a transfer or corporate action was booked on the wrong day, reverse its execution before entering it again to avoid duplicate bookings.

Historical prices changed by an earlier split application are not automatically restored. Do not simply apply the split again; first check and, where necessary, restore the price series. Original creation times that were overwritten cannot be recovered reliably either. Older system timestamps may reflect different former server and database settings and are not shifted retrospectively as a group.

Also check [standing orders entered in advance]({{% relref "/transaction/standingorder" %}}) whose next execution date is still missing.
