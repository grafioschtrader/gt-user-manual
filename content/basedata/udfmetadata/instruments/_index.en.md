---
title: "Custom field Instrument"
date: 2024-06-23T20:54:47+01:00
draft: false
weight: 12
archetype: "default"
---
There is a **special implementation** for user-defined properties for **instruments**. This takes into account the properties of an instrument's asset class. For example, the user defines an additional field for the costs of an ETF. It would make no sense if this additional field also appeared when entering additional data for a share security.

## General fields
- **Yield to maturity**: The additional field "Yield to maturity" is available to all users for **bonds**. The user cannot change this additional field. This additional field is calculated from the interest rate, the current date and the maturity of the bond. The interest rate is determined from the name of the bond if the **name** contains **the interest rate as a prefix**. For most bond investors, this figure is a decision-making aid for buying or selling a bond.

### Yahoo content for general additional fields
The following contents all relate to Yahoo. There are **two links** and an **extracted date with time**. Since GT does not know the Yahoo symbol for every security, it may have to be determined. This determination can be very time-consuming, as the Yahoo website has to be contacted. For this reason, there is also a [background job](../../../admindata/taskdatachangemonitor/taskdescription/#JOB19) that performs this task. However, the same can also be done interactively in the user interface, which can lead to longer waiting times.

- **Yahoo statistics link**: The Yahoo statistics provide a good overview of the various information on a security.
- **Next earning**: Often the balance sheet date has a significant impact on the share price. Both before and after the publication of the figures. In addition, the balance sheet figures of other companies can also move the share price, as there are competitors, dependencies, etc. This information is read from the website for each security. This can take some time.
- **Yahoo earnings Link**: Depending on the security, this link provides a history of earnings/losses per share and an indication of expected earnings. In most cases, the future balance sheet dates are also indicated.

## Properties and table columns
The processing and properties of the "Additional field instrument" are largely identical to those of the "Additional field general". It has been expanded to include the two properties of **asset class** and **financial instrument**. The selection is made via a list field with multiple selection. The additional field defined in this way appears when it is **filled in** if **both criteria** of the instrument's asset class are met.
