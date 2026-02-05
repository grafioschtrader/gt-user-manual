---
title: "Automatic answer"
date: 2025-12-23T22:54:47+01:00
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

### Available Variables for Conditions
The condition field supports [EvalEx](https://ezylang.github.io/EvalEx/) expressions. A context menu (right-click) in the condition input field provides easy access to all available variables and functions. Variables are grouped by category:

#### My Server
Variables about this server instance:
| Variable | Type | Description |
|----------|------|-------------|
| `MyDailyRequestLimit` | NUMBER | Daily request limit we impose on remote servers |
| `MyDailyRequestLimitRemote` | NUMBER | Daily limit set by remotes for us |
| `MyTimezone` | STRING | Timezone of this server |
| `MyMaxLimitLastPrice` | NUMBER | Max instruments per last price request from this server |
| `MyMaxLimitHistorical` | NUMBER | Max instruments per historical request from this server |

#### Remote Server
Variables about the requesting remote server:
| Variable | Type | Description |
|----------|------|-------------|
| `RemoteDailyRequestLimit` | NUMBER | Daily request limit the remote server imposes |
| `RemoteDailyRequestLimitRemote` | NUMBER | Daily limit set by others on the remote server |
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
| `dailyCount` | NUMBER | Number of requests received today from this remote |
| `dailyLimit` | NUMBER | Daily request limit for this remote |

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

**Reject when daily limit is exceeded:**
```
dailyCount >= dailyLimit
```

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
