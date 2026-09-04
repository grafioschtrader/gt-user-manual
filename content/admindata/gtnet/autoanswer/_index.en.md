---
title: "Automatic answer"
date: 2026-08-26T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
GTNet enables automatic responses to incoming messages based on configurable rules. This allows frequent requests to be processed without manual intervention.

### How It Works
For each message type that requires a response, multiple response rules can be defined. These rules are evaluated in order of their priority. As soon as a condition is met, the corresponding response is sent and the evaluation ends. If no rule applies, the message waits for manual processing by the administrator.

### Rule Configuration
Each automatic response rule includes the following properties:
- **Request Message Type**: The type of incoming message to which this rule applies.
- **Response Message Type**: The response to be sent when the condition is met (e.g., Accept or Reject).
- **Priority**: The evaluation order of the rules. Lower values are checked first.
- **Condition**: An [EvalEx](https://ezylang.github.io/EvalEx/) formula that determines whether this rule is applied. An empty condition is always met.
- **Message Text**: An optional text sent with the response.
- **Waiting Time in Days**: A cooldown period after a rejection before the sender can request again.

Both dropdowns are filled from the server. Only the message types for which an automatic answer is meaningful can be
chosen as the request type — a request the receiving instance answers by itself, such as the synchronisation of the
exchange settings, is not among them — and only the answers that belong to that request appear as the response type — a rule that pairs a
handshake with the rejection of a data request is refused when it is saved. Refusals the server issues on its own are
not offered at all: the rejection for a domain this instance does not know is one of them, because configured as a rule
it would tell an already registered peer that it is not in the list.

### Available Variables for Conditions
The condition field supports [EvalEx](https://ezylang.github.io/EvalEx/) expressions. A context menu (right-click) in the condition input field provides easy access to all available variables and functions. Variables are grouped by category:

#### My Server
Variables about this server instance:
| Variable | Type | Description |
|----------|------|-------------|
| `MyDailyRequestLimit` | NUMBER | Daily request limit we impose on remote servers |
| `MyTimezone` | STRING | Timezone of this server |
| `MyMaxLimitLastPrice` | NUMBER | Max instruments per last price request from this server |
| `MyMaxLimitHistorical` | NUMBER | Max instruments per historical request from this server |

#### Remote Server
Variables about the requesting remote server:
| Variable | Type | Description |
|----------|------|-------------|
| `RemoteDailyRequestLimit` | NUMBER | Daily request limit the remote server imposes |
| `RemoteTimezone` | STRING | Timezone of the remote server |
| `RemoteDomainRemoteName` | STRING | URL of the remote server |
| `RemoteMaxLimitLastPrice` | NUMBER | Max instruments per last price request from remote server |
| `RemoteMaxLimitHistorical` | NUMBER | Max instruments per historical request from remote server |

#### Message
| Variable | Type | Description |
|----------|------|-------------|
| `Message` | STRING | Free-text message content from the request |

#### Connections
| Variable | Type | Description |
|----------|------|-------------|
| `TotalConnections` | NUMBER | Total number of active GTNet connections |
| `ConnectionsLastPrice` | NUMBER | Number of connections for last price data |
| `ConnectionsHistorical` | NUMBER | Number of connections for historical data |

#### Calculated
| Variable | Type | Description |
|----------|------|-------------|
| `TimezoneOffsetHours` | NUMBER | Hours difference between remote and local timezone |
| `hour` | NUMBER | Current hour (0-23) in UTC |
| `dayOfWeek` | NUMBER | Day of week (1=Monday, 7=Sunday) |
| `dailyCount` | NUMBER | Number of requests received today from this remote, including the one being decided |
| `dailyLimit` | NUMBER | Daily query limit this server grants each peer; identical to `MyDailyRequestLimit` |

#### Request Parameters
Besides the variables listed above, a condition can also read the parameters the incoming request itself carries. These are addressed with the prefix `param.`, for example `param.entityKinds` on a request for data exchange. Which parameters exist depends on the request message type.

{{% notice warning %}}
The `param.` prefix is mandatory. These parameters come from the requesting peer and are therefore deliberately kept in a namespace of their own, so that a peer can never overwrite the value of one of the variables listed above and decide on its own admission. Without the prefix the name cannot be resolved and the condition does not match. Existing rules that use such a parameter without the prefix have to be adjusted.
{{% /notice %}}

### String Functions
For conditions involving string comparisons, EvalEx string functions are available. These can be inserted via the context menu under "String Functions":

| Function | Description |
|----------|-------------|
| `STR_STARTS_WITH(string, prefix)` | Returns true if string starts with prefix (case-sensitive) |
| `STR_ENDS_WITH(string, suffix)` | Returns true if string ends with suffix (case-sensitive) |
| `STR_CONTAINS(string, substring)` | Returns true if string contains substring (case-insensitive) |
| `STR_MATCHES(string, pattern)` | Returns true if string matches the RegEx pattern |
| `STR_LENGTH(string)` | Returns the length of the string |
| `STR_LOWER(string)` | Converts string to lowercase |
| `STR_UPPER(string)` | Converts string to uppercase |
| `STR_TRIM(string)` | Removes leading and trailing whitespace |
| `STR_LEFT(string, n)` | Returns the first n characters |
| `STR_RIGHT(string, n)` | Returns the last n characters |
| `STR_SUBSTRING(string, start, end)` | Extracts substring from start to end position |

### Context-Aware Input Assistance
When entering conditions, the context menu automatically filters available variables based on cursor position:
- Inside a string function parameter expecting a STRING, only STRING variables are shown
- Inside a function parameter expecting a NUMBER, only NUMBER variables are shown
- Outside any function, all variables and functions are available

### Example Conditions

**Accept requests only during business hours (UTC):**
```
hour >= 8 && hour <= 18 && dayOfWeek >= 1 && dayOfWeek <= 5
```

**Reject before the daily limit is reached:**
```
dailyCount >= dailyLimit * 0.8
```

{{% notice tip %}}
The [Daily query limit](../requestlimits/) is enforced by GT itself, no rule is needed for it. A request beyond it is refused before any rule is evaluated, so the condition `dailyCount >= dailyLimit` only ever holds for the very last still-permitted request. Rules are useful here when you want to react **earlier** than the hard limit, as in the example above from four fifths of the allowance onwards.
{{% /notice %}}

**Accept only from trusted domains:**
```
STR_STARTS_WITH(RemoteDomainRemoteName, "https://trusted-partner")
```

**Check message content for specific keywords:**
```
STR_CONTAINS(Message, "urgent") || STR_CONTAINS(Message, "priority")
```

**Combined condition - accept from known domains during business hours:**
```
STR_STARTS_WITH(RemoteDomainRemoteName, "https://partner") && hour >= 9 && hour <= 17
```

**Accept based on timezone compatibility:**
```
TimezoneOffsetHours >= -3 && TimezoneOffsetHours <= 3
```

### Waiting Time After Rejection
To avoid repeated requests from rejected servers, a waiting time in days can be set. During this time, renewed requests of the same type from the same instance are automatically ignored or delayed. This gives the administrator time for a manual review of persistent requests.
