---
title: "Global settings"
date: 2021-05-09T22:54:47+01:00
draft: false
weight: 30
archetype: "default"
---

This table shows the **global settings** of GT. Many **properties** relate to different **limits**, such as the number of possible portfolios per client. Changing the properties is reserved for the administrator.

### Edit global settings
Only the administrator can edit these, and only the **property value** of the existing settings can be changed.

### Properties and table columns
The properties are self-explanatory and are not explained further.

### Special settings
The meaning of certain settings is described elsewhere in this document. Only those settings are mentioned here that are more extensive or are not explained elsewhere in this documentation.

#### Password setting (gt.password.regex.properties)
This property can be used to set a specific password strength for the user password. This setting has an expandable table row in the view, from which the current setting can be taken. A regular expression can be used to define a specific minimum requirement for a valid password. Here are some examples:

- "^(?=.*\[A-Za-z])(?=.*\d)\[A-Za-z\d]{8,}\$": At least eight characters, at least one letter and one number.
- "^(?=.*\[A-Za-z])(?=.*\d)(?=.*\[@!%\*#?&])\[A-Za-z\d@!%#*?&]{8,}\$": At least eight characters, at least one letter, one number and one special character.
- "^(?=.*\[a-z])(?=.*\[A-Z])(?=*.\d)(?=.*\[@!%\*?&])\[A-Za-z\d@!%\*?&]{8,}\$": At least eight characters, at least one upper case letter, one lower case letter, one number and one special character.
- "^(?=.*\[a-z])(?=.*\[A-Z])(?=*.\d)(?=.*\[@!%\*?&])\[A-Za-z\d@!%\*?&]{8,10}\$": At least eight and at most 10 characters, at least one uppercase letter, one lowercase letter, one number and one special character.

By default, the first example is set in GT. Users are not prompted to enter a new password.

##### Parameters for the password setting
The following parameters must be available for the password setting:
- **regex**: Assign the regular expression here.
- **forceRegex**: If a password no longer meets the requirements of the **regular expression**, the user can be forced to enter a new password that meets the requirements. If "**forceRegex**" is set to "**true**", the dialog for changing the password is opened after logging in if the **password strength** is not met. After successfully entering the new password, the user must log in again. If "**forceRegex**" is set to "**false**", the existing passwords are still accepted and the regular expression is only used when the password is changed.
- **de** and **en**: GT currently supports these two languages. Therefore, a hint or error text must be available for the corresponding language and the regular expression. The user must be able to understand the requirements of the regular expression from this text.
