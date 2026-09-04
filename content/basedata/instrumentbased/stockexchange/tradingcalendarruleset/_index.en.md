---
title: "Trading calendar rules"
date: 2026-07-19T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
A **rule set** describes the public holidays of one or more stock exchanges as rules. From these rules GT calculates the closed days of the trading calendar for each year. Unlike the [index for trading calendar](../), a rule set also works for future years for which no price data exists yet, and it needs no index traded on the exchange. A stock exchange uses either an index **or** a rule set, never both at the same time.

Rule sets are shared data: every user can view and edit them. If a user only has restricted rights, the change becomes a change proposal that an administrator confirms. The number of creations, changes and deletions per day is limited.

## Reaching the view
Rule sets are managed in the main area of the **stock exchange** view. This view is split into two tabs: the **stock exchange** itself and the **trading calendar rules**. The second tab leads to the table of rule sets; creating, editing and deleting are done as usual via the **context menu**.

## Table columns
- **Rule**: The unique name of the rule set, for instance the name of the exchange.
- **MIC**: The optional Market Identifier Code. When a stock exchange is created with a MIC for which a rule set with the same MIC exists, GT assigns that rule set automatically.
- **Extends rule set**: The parent rule set whose rules this rule set inherits.
- **Used by exchanges**: The number of stock exchanges that calculate their trading calendar from this rule set.

## Create and edit
When creating or editing a rule set the following details are entered:
- **Rule** (name): Mandatory, two to sixty-four characters, unique across all rule sets.
- **MIC**: Optional, exactly four characters. A MIC may be assigned to a single rule set only.
- **Extends rule set**: Optional. A rule set can extend another one and inherit its rules. Related exchanges thus share a common base: the German regional exchanges, for instance, extend Xetra, and the Swiss exchanges extend the SIX. A rule set cannot extend itself, not even indirectly.

A rule set that is used by at least one exchange or extended by another rule set cannot be deleted. A rule set without any rules of its own must extend another rule set.

## Rules in the YAML editor
The actual holiday rules are entered in the **YAML editor**. As you type, the editor suggests the possible fields and values and shows a short help for each entry. Before saving, the button validates the rules and lists any errors together. Because these details are entered directly in the editor, the English keywords below are part of the operation and are named here deliberately.

A rule set has the following top-level entries:
- **note**: Free text recording the sources and known limitations of this calendar for the next person editing it. It has no effect on the calculated dates.
- **authoritativeFrom** and **authoritativeThrough**: The range of years for which the rule set is backed by an authoritative exchange calendar. Asking for an earlier or later year is rejected rather than answered with a guess.
- **rules**: The list of recurring closures. If a rule set extends another one, it lists only its deviations here; the inherited rules are evaluated first.
- **additionalClosures**: Announced or historical closures that cannot be expressed safely as a recurring rule, for instance a national day of mourning. These dates are added to the calculated closures.
- **openDates**: Dates on which a rule produces a closure but the exchange nevertheless held a trading session. These dates are removed from the calculated closures and are the only way to cancel a single occurrence of a rule.

### Rule types
Every rule must have a **name** and a **type**. Which further fields are required depends on the rule type; a rule missing a field it needs is rejected when saving.

| type | Meaning | Required fields |
|------|---------|-----------------|
| **FIXED** | The same month and day every year, e.g. 1 January. | month, day |
| **NTH_WEEKDAY** | The n-th weekday of a month, e.g. the third Monday in January. `nth: -1` selects the last one. | month, nth, dayOfWeek |
| **WEEKLY** | Every occurrence of a weekday, for an exchange weekend that differs from Monday to Friday. | dayOfWeek |
| **EASTER_RELATIVE** | An offset in days from Western Easter Sunday: -2 Good Friday, 1 Easter Monday, 39 Ascension, 50 Whit Monday, 60 Corpus Christi. | offset |
| **ORTHODOX_EASTER_RELATIVE** | Like EASTER_RELATIVE, but based on Orthodox (Julian) Easter. | offset |
| **HIJRI** | A date of the Islamic calendar, converted to the Gregorian one. | month, day |
| **EXPLICIT_DATES** | A fixed list of dates that cannot be calculated, e.g. Chinese New Year. | dates |

The following fields are available in addition:
- **validFrom** and **validTo**: First and last year in which the closure applies. This models holidays that were only introduced from a given year or abolished from a given year on; it is also how an inherited rule is switched off from a given year.
- **observance**: How a closure falling on a weekend is transferred. `NONE` drops it, which is correct for most continental European exchanges; `NEAREST_WEEKDAY` (US exchanges), `SUNDAY_TO_MONDAY`, `NEXT_MONDAY` (UK substitute days) and `NEXT_WEEKDAY` move it to a working day. When omitted, `NONE` applies.
- **halfDay**: `true` when the exchange only shortens its trading hours instead of closing. Such a day is recorded so the rule set describes the exchange completely, but it never produces a closure, because a half day is a trading day.
- **offsetTolerance**: Only for HIJRI. The number of days by which the observed Islamic date may deviate from the conversion, because the start of the month depends on the sighting of the moon.

### Example
The following shortened rule set shows the structure. Each rule is on its own line; weekends need not be listed, because the trading calendar handles Saturday and Sunday separately anyway.
```yaml
note: >
  Holidays of the example exchange. Holidays are not transferred when they fall
  on a weekend.
authoritativeFrom: 2000
rules:
  - {name: NewYear, type: FIXED, month: 1, day: 1}
  - {name: GoodFriday, type: EASTER_RELATIVE, offset: -2}
  - {name: EasterMonday, type: EASTER_RELATIVE, offset: 1}
  - {name: LabourDay, type: FIXED, month: 5, day: 1}
  - {name: Ascension, type: EASTER_RELATIVE, offset: 39}
  - {name: WhitMonday, type: EASTER_RELATIVE, offset: 50}
  - {name: NationalDay, type: FIXED, month: 8, day: 1}
  - {name: Christmas, type: FIXED, month: 12, day: 25}
  - {name: StStephen, type: FIXED, month: 12, day: 26}
additionalClosures:
  - 2024-04-15
openDates:
  - 2023-12-26
```

## Preview closures
The collapsible **preview closures** area resolves the rule set for a single year before saving. The preview also takes the inherited rules of the extended rule set into account and lists, for each calculated day, the **rule** that caused it. Use **previous year** and **next year** to switch between years. This lets you check whether the rules produce the expected holidays before the trading calendar is recalculated.

The trading calendar is not recalculated immediately but through a background task. For more on this, see [Background tasks](../../../../admindata/taskdatachangemonitor/taskdescription/#JOB53).
