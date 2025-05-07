---
title: "Security and derived instrument"
date: 2021-04-01T10:54:47+01:00
draft: false
weight: 8
archetype: "default"
---
A **security** and a **derived security** have many things in common, which is why they are documented together here.

### Security split
If a security split is entered whose date is before the "Full data load" date, the historical data for this security is read in again.

### Create and edit security or derived instrument
A security or instrument is created, edited and deleted in the main area in one of the **watchlist views**. These views can be accessed in the navigation area via the name of the **watchlist**.
- Create a security via the context menu by selecting "**Add new security**".
- Create a derived instrument by selecting "**Add new derived instrument**".
- Edit a security or instrument via the context menu with the menu item "**Edit instrument**" for the selected instrument or security.
- Delete a security or instrument via the context menu with the menu item "**Remove and delete instrument**" on the selected instrument.

### Properties
Most of the common properties of **securities** and **derived securities** are self-explanatory and are not documented further.
- **ISIN** and **ticker/symbol**: The **ISIN** property can only be set when creating an instrument. If the **ISIN** is displayed, a value must be entered. Together with the currency, a security is identified in GT. The **ticker/symbol** should be entered if possible; these may be used in the **user-defined additional fields** for **identification** with a **data provider**.
- **Private security**: An instrument can be defined as private. This means that it is not visible to other users. A **private security** cannot have **an ISIN** and **ticker/symbol**.
- **Currency**: The currency of the instrument can no longer be changed after it has been saved for the first time.
- **Trade from**: If this date is set to an earlier time for an existing **active** security, the historical price data is reloaded.
- **Stock exchange hyperlink**: This hyperlink is intended for calling up a main trading center for the instrument. This functionality is offered in different views via the context menu.
- **Product hyperlink**: This can be used to enter a hyperlink for the product. This hyperlink is offered in different views via the context menu. This hyperlink will have different characteristics depending on the financial instrument. In the case of an ETF or investment fund, the link to the product of the corresponding provider is usually entered. In the case of a share, the link could refer to the **investor relations** of the corresponding company.
