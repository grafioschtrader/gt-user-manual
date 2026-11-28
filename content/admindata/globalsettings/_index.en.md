---
title: "Global settings"
date: 2026-11-16T12:00:00+01:00
draft: false
weight: 30
archetype: "default"
---
This table shows the **global settings** of GT. Changing the properties is reserved for the administrator.

{{% callout note %}}
The **limits** — for example the number of possible portfolios per client or the number of changes per day — are no longer part of this table. They are managed in the [Limit information class]({{% relref "/admindata/entitylimit" %}}) view. What remains in the global settings are the thresholds for blocking a user, `g.max.security.breach.count` and `g.max.limit.request.exceeded.count`; these are not limits on data, see [User rights]({{% relref "/intro/userrights" %}}).
{{% /callout %}}

## Edit global settings
Only the administrator can edit these, and only the **property value** of the existing settings can be changed.

## Properties and table columns
The properties are self-explanatory and are not explained further.

## Special settings
The meaning of certain settings is described elsewhere in this document. Only those settings are mentioned here that are more extensive or are not explained elsewhere in this documentation.

### Password setting (gt.password.regex.properties)
This property can be used to set a specific password strength for the user password. This setting has an expandable table row in the view, from which the current setting can be taken. A regular expression can be used to define a specific minimum requirement for a valid password. Here are some examples:

- "^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$": At least eight characters, at least one letter and one number.
- "^(?=.*[A-Za-z])(?=.*\d)(?=.*[@$!%*#?&])[A-Za-z\d@$!%*#?&]{8,}$": At least eight characters, at least one letter, one number and one special character.
- "^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$": At least eight characters, at least one upper case letter, one lower case letter, one number and one special character.
- "^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,10}$": At least eight and at most 10 characters, at least one uppercase letter, one lowercase letter, one number and one special character.

By default, the first example is set in GT. Users are not prompted to enter a new password.

#### Parameters for the password setting
The following parameters must be available for the password setting:
- **regex**: Assign the regular expression here.
- **forceRegex**: If a password no longer meets the requirements of the **regular expression**, the user can be forced to enter a new password that meets the requirements. If "**forceRegex**" is set to "**true**", the dialog for changing the password is opened after logging in if the **password strength** is not met. After successfully entering the new password, the user must log in again. If "**forceRegex**" is set to "**false**", the existing passwords are still accepted and the regular expression is only used when the password is changed.
- **de** and **en**: GT currently supports these two languages. Therefore, a hint or error text must be available for the corresponding language and the regular expression. The user must be able to understand the requirements of the regular expression from this text.

## Validation of input rules for global parameters
To prevent invalid entries, there is validation for some property values. This also serves as an indication of which input values are accepted. The same syntax is used for the values of the [Limit information class]({{% relref "/admindata/entitylimit" %}}); there, however, the rule belongs to the limit itself and not to the individual row.

Supported rules

| Rule    | Syntax            | Description                                       | Applies to     |
|---------|-------------------|---------------------------------------------------|----------------|
| min     | min:N             | Minimum allowed value                             | propertyInt    |
| max     | max:N             | Maximum allowed value                             | propertyInt    |
| enum    | enum:N1,N2,N3,... | Value must be one of the specified values         | propertyInt    |
| pattern | pattern:REGEX     | Value must match the regex pattern                | propertyString |

Syntax

Rules are separated by commas: rule1:value1,rule2:value2

Note: For enum, the values are also separated by commas, but the parser handles this correctly by only splitting at `,` when a rule name and `:` follow.

Examples

| inputRule            | Description                         | Valid values  | Invalid values  |
|----------------------|-------------------------------------|---------------|-----------------|
| min:1,max:99         | Value between 1 and 99              | 1, 50, 99     | 0, 100, -5      |
| min:0,max:100        | Percentage value                    | 0, 50, 100    | -1, 101         |
| enum:1,7,30,365      | Specific day intervals              | 1, 7, 30, 365 | 2, 14, 60       |
| enum:0,1             | Boolean-like integer                | 0, 1          | 2, -1           |
| min:1                | At least 1 (no upper limit)         | 1, 100, 9999  | 0, -1           |
| max:10               | At most 10 (no lower limit)         | -5, 0, 10     | 11, 100         |
| pattern:^[A-Z]{2,3}$ | 2-3 uppercase letters               | "CH", "USD"   | "ch", "EURO"    |
| pattern:^[a-z0-9.]+$ | Lowercase letters, digits and dots  | "gt.test"     | "GT.Test"       |

Combined rules

Multiple rules can be combined:

| inputRule                         | Description                                                     |
|-----------------------------------|-----------------------------------------------------------------|
| min:1,max:99                      | Range validation                                                |
| min:0,max:100,enum:0,25,50,75,100 | Range + specific allowed values (enum takes precedence)         |

## Language models and tenant keys
**gt.ai.tenant.providers** determines which providers users may configure under **Settings > Language-model keys**. The default is `anthropic,openai,gemini`; `none` allows no providers. Add `ollama` only if tenants may use the instance's hardware. Saved entries for a provider that is no longer allowed remain stored but are skipped.
**gt.ai.providers** describes only the providers and models associated with instance keys. **gt.ai.instance.keys.propose.rule** defaults to `0`. Setting it to `1` lets rule proposals fall back to instance keys after trying the user's providers. Value suggestions never use this fallback. **gt.ai.daily.requests.instance** limits only attempts using instance keys; tenant-funded attempts do not consume it. In addition, all attempts and key tests count against the daily limit per user that you set under [Entity limit]({{% relref "/admindata/entitylimit" %}}) (500 requests by default), which protects the computing time of the instance. Tenants cap the cost of their own keys themselves with the daily query limit of each key.
## AI assistants (MCP)
With **gt.mcp.use** you open (`1`) or close (`0`) the MCP server through which AI assistants read the users' portfolios. After installation it is closed. When it is closed, every access token is refused immediately. **g.public.base.url** holds the public HTTPS address of the instance, for example `https://gt.example.org`, without a trailing slash. The endpoint users see in the dialog and the addresses of the OAuth sign-in that assistants such as ChatGPT require are derived from it. With `none` only personal access tokens work. The same address supplies the link in the [password reset]({{% relref "/intro/register#forgot-password" %}}) email and is a prerequisite for [passkeys]({{% relref "/intro/settings#passkeys" %}}). Passkeys require a host name; with an IP address GT does not offer them. A passkey is bound to this host name: if you change it, all registered passkeys of the users become unusable, while passwords and authenticator apps keep working. Both settings take effect without a restart. More under [AI assistants (MCP)]({{% relref "/intro/settings/aiassistant" %}}).
## Visibility of the nickname
With **g.user.nickname.visible** you decide whether the users of your instance see the **nickname** of other users. With `0` only the user ID appears, and only the administrator sees the nickname. With `1` all users see it. After installation `0` is set. Only these two values are allowed.
The nickname appears where a user would otherwise see only an ID. Today that is the column **By user** in the table **Data change request for user**. If no nickname is known for a user, the ID remains. Note that the nickname is then visible to all users of the instance. The setting reaches a user at sign-in; someone who is already signed in sees the change after the next sign-in. More about the user ID under [Data and user rights]({{% relref "/intro/userrights" %}}).
