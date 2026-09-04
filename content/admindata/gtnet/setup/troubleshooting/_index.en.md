---
title: "Restoring a Connection"
date: 2026-08-28T22:54:47+01:00
draft: false
weight: 25
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet fully completed. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
This page describes how to restore an existing connection to a counterpart after your own instance has lost its credentials for that counterpart. This typically happens after rebuilding the database, after restoring an older backup, or after moving the instance to another server without exporting and importing the GTNet data.

### How to recognise the problem
Your entry for the counterpart no longer shows a tick in the **Authorized** column, and a new **"First contact"** is refused by the counterpart. The reason given is the message "A handshake with this domain is already established". The same procedure also helps when a message about an invalid authentication token appears instead, because both cases have the same cause: the two instances no longer agree on their shared credentials.

If the message instead states that your instance is not in the counterpart's server list, a different situation applies. The counterpart then does not know your domain at all and does not admit unknown servers automatically. In that case the counterpart's administrator has to add your domain first or activate the **Allow server creation** setting.

### Why your own instance cannot solve this by itself
**"First contact"** is the only message that reaches the recipient without credentials, since the sender does not have any at that point. It may therefore open a new relationship, but it must never overwrite an existing one. Otherwise anyone quoting the domain of another instance could reset that instance's credentials at all of its counterparts. For the same reason a **"Token renewal"** does not help either: that message requires valid credentials, and those are exactly what is missing.

The connection can therefore only be restored on the side that still holds the credentials. Re-admitting the counterpart is a deliberate decision of the administrator there.

{{< mermaid >}}
flowchart TD
    A["Own instance has lost its credentials"] --> B["First contact sent to the counterpart"]
    B --> C{"Does the counterpart still hold credentials for us?"}
    C -->|Yes| D["Refused: A handshake with this domain is already established"]
    D --> E["Counterpart's administrator: Allow new handshake"]
    E --> B
    C -->|No| F["Contact successful, connection restored"]
{{< /mermaid >}}

### How the counterpart learns about it
The instance that lost its credentials is caught in a bind: its **"First contact"** is refused, and it cannot send an administrative message either, because that would require the very connection it is being denied. It therefore has no way of notifying the administrator on the other side.

For that reason the refusal now leaves a trace on the side that still holds the credentials. In the server overview, a highlighted icon appears in the **Reconnect requested** column on the row of the counterpart concerned, as soon as that counterpart has tried in vain to reconnect. The tooltip names the time of the last attempt. Clicking the icon runs the same prompt and the same action as the menu item **"Allow new handshake"**, so the case can be settled right where it becomes visible.

The entry is kept up to date with the most recent attempt and disappears once the new handshake has taken place or the administrator has allowed it. A counterpart that keeps retrying produces no additional entries.

{{% notice info %}}
Anyone who regularly exchanges data with other instances should check the server overview for this icon from time to time. It is the only indication that a counterpart is looking to reconnect.
{{% /notice %}}

### Allowing a new handshake
On the instance that still holds the credentials, the context menu of the server overview offers the menu item **"Allow new handshake"**. It discards the credentials shared with the selected counterpart, so that its next **"First contact"** is accepted again. The affected domain is named in a confirmation prompt before anything is discarded. Clicking the icon in the **Reconnect requested** column does the same.

The procedure comprises the following steps:
1. On the instance that still holds the credentials, select the row of the counterpart that can no longer connect.
2. Call **"Allow new handshake"** from the context menu and confirm the prompt.
3. The row then shows no tick in the **Authorized** column, and the online status changes to **Unknown**.
4. On the instance that lost its credentials, select the counterpart's row and send a **"First contact"** from the context menu.
5. After the answer **"Contact successful"**, both instances show a tick in the **Authorized** column again.
6. Finally, use **"Check status now"** to confirm that the connection really carries traffic again.

The last step is worthwhile because a successful handshake alone says nothing about whether the subsequent communication works. Only a status check sends a request using the new credentials and thereby proves that both directions are usable again.

### What is kept and what changes
Only the credentials are discarded. The counterpart's entry remains with its complete message history, as do the settings for the information objects and the permissions granted to that counterpart, such as **Pass on server list** or the **Daily request limit**. This is precisely what distinguishes this route from deleting the entry, where all of the above is lost.

Until the new contact is completed, the counterpart is shown as **Unknown** and the exchange of its information objects is closed. No data exchange takes place with that instance in the meantime. Why an entry without credentials carries the status **Unknown** is described under [GTNet and Messages](../).

{{% notice note %}}
The menu item is visible to administrators only. It is disabled for your own server entry, and likewise for entries with which a connection has never been established.
{{% /notice %}}

### When the counterpart belongs to another operator
If the other instance is not under your own responsibility, the problem cannot be solved from your side. Your unsuccessful contact does become visible there, though: the icon appears in the **Reconnect requested** column on the row of your domain. An attentive administrator therefore sees for themselves that someone is looking to reconnect. If another channel of contact exists as well, they can additionally be asked to call **"Allow new handshake"**. Once they have confirmed this, a new **"First contact"** from your instance is all that is needed.

### How to avoid the problem
When an instance is moved to another server or its database is rebuilt, the GTNet data should be exported beforehand and imported afterwards. The credentials of all counterparts are then preserved and no connection is lost. The procedure is described under [GTNet and Messages](../) in the section on exporting and importing GTNet data.
