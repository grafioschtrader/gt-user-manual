---
title: "Administrator data and message system"
date: 2026-06-26T22:54:47+01:00
draft: false
weight: 50
archetype: "default"
---
## Administrative data
Due to the **user rights**, the static main element **Administrative data** in the **navigation area** is different. The "**User settings**" sub-element is only visible to an administrator.

## Alarms and messaging system
GT implements an alarm and message system. The alarm system is intended for GT malfunctions and is aimed at the main administrator. While the message system is intended for all users.

## Alarms
The [main administrator](../intro/userrights/#hauptadmin) receives **alerts** from GT directly to their e-mail address. For example, if the processing of background tasks no longer works. The rectification of such a fault should be resolved as soon as possible.

## Message system
The data change request can already be used to make a specific suggestion for improving the data quality for an individual entity. On the other hand, there may also be problems that are not related to an individual entity. It must therefore be possible for the **user** to communicate with one of the **privileged user roles**. In addition, the **system** can also perform certain **checks on the data** and alert the user to any missing entries. Finally, the **administrator** must be able to send a **message to everyone**. For example, to announce a maintenance interruption. These are the main reasons why GT implements a messaging system.

#### Message user <--> user
Only a user with the Administrator user role can send an **initial message** directly to another user. Indirectly, this also works for all other users by creating a message from a specific **context**. For example, a message can be sent to the creator of a specific security. As explained in this case, the recipient is not selected directly, but determined from the context.

To prevent misuse of the messaging system — for example sending messages directly through the programming interface, bypassing the user interface — the system verifies, for such a context message, that the recipient really is the **creator** of the referenced instrument and that this instrument is on one of the **sender's watchlists**. Messages that do not meet these conditions are rejected. Within an existing message topic, only the users involved in that topic can reply. Users with the **Administrator** user role are not subject to this context restriction.

As a further protection against misuse, the number of messages a user with the role **Limits** may send per day is capped. The administrator controls this in the [global settings]({{% relref "/admindata/globalsettings" %}}) via the parameter `g.limit.day.MailSendRecv` (default 200). Users with privileged roles (Administrator or without limits) are exempt from this limit.

#### Message user <--> privileged user role
Indirect communication between two users already takes place when a data change request is made. On the other hand, the user may wish to send a request to a specific user role independently of a change to an entity.

#### System message --> User
GT performs some **monitoring functions** for private and shared data at regular intervals. In most cases the completeness of the data is checked and in case of incompleteness a user interaction can correct this possible deficiency. The system therefore creates a message and sends it to the relevant user or to the responsible user role.

### Message(s)
The messages are displayed in a **hierarchy table**, with the main element representing the **initial message** of a **message topic** and the **sub-elements** containing the subsequent message exchange.
- For easier identification, a **message topic** with **sub-elements** is highlighted in color on the **initial message**.

#### Properties and table columns
Most of the properties are self-explanatory and are therefore not explained further; in addition, a tooltip is available for certain properties.
- **Number of responses**: An **initial message** is usually sent to a user role. This can be answered privately and is therefore invisible to the other users in this user role. For this reason, the number of responses is displayed so that any **unnecessary responses** are avoided.

#### Message topic of a user
Above, we described how a message topic can be created by a user. The messages in such a message topic are private and it is a communication between two users.

#### Message topic of a user role
In the **Message** view, **initial messages** can only be sent to one **user role**. All messages in such a **message topic** are visible to all users of the addressed user role. However, it is also possible to reply privately in such a message topic, i.e. invisible to the other users of the user role. This is the main reason for the existence of the "**Number of responses**" table column. Such a role message topic is only removed for good once every member of the role has deleted it; this cleanup is performed by a daily [background task]({{< relref "/admindata/taskdatachangemonitor/taskdescription/#JOB29" >}}).

#### Message topic by system message to user
Messages from the system should be responded to and the possible action carried out. It is possible to **reply** to the system message to a specific user role. This can be used if the recipient is not able to solve the problem described in the system message themselves.

### Setting message
The assignment between **message type** and **communication channel** is made here. GT currently supports the two communication channels **GT message** and **e-mail**. If no individual settings are made by the user, the default settings apply. Only the "Administrator" user role has the option of forwarding the message to another user.
- A message that is sent to several users is delivered as a **GT message** by default; the user can additionally choose to also receive an **e-mail**. For the administrator's announcement to all, the user may instead select **e-mail only** — in that case the announcement is not shown as a GT message (see the warning below).

{{% notice warning %}}
If a user sets the channel for **Announcement to all by administrator** to **e-mail only**, the announcement is no longer shown as a GT message in their inbox. Because GT does not receive incoming e-mails, such a user **cannot reply** to the announcement. Choose e-mail only when no reply is expected.
{{% /notice %}}

Requests addressed to the administrator — in practice the **Blocking user external data/query limit** request from a blocked user — reach the [main administrator](../intro/userrights/#hauptadmin), that is the person whose e-mail address is configured in `application.properties` under `gt.main.user.admin.mail`, and not every administrator. Where several people share the **Administrator** role, the main administrator can hand such a request to another administrator, either to divide the workload between them or to cover their own absence, for example during a holiday. The other administrator is selected from a drop-down that lists the remaining administrators by their nickname; leaving the selection empty keeps the request with the main administrator.

{{% notice info %}}
To prevent endless forwarding, GT does not allow a redirection that would create a cycle — for example administrator A forwarding to administrator B while B already forwards back to A, or an administrator forwarding to themselves. Such a setting is rejected.
{{% /notice %}}

| From           | To            | Message type                                                                                                                                                                           | Standard channel      |
| -------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| User           | User          | **General purpose user to user communication**: A direct message between two users. Only an administrator can start such a message directly; other users reach a recipient from a context. | GT message            |
| System channel | User role     | **Holding a non-active position**: An instrument has the property "Active until or maturity date" if such an instrument is held after this date.                                         | GT message            |
| System         | User role   | **Possible missing interest/dividends**: GT attempts to determine possible missing entries regarding interest or dividends. Unfortunately, this algorithm also generates false warnings. | GT message            |
| System         | User          | **Proposed change for shared data**: The owner of shared data is notified that a change to that data has been proposed.                                                                  | GT message            |
| User/System    | Administrator | **Blocked user external data/query limit**: The blocked user wants to return to GT. This is also visible in the user settings.                                                           | -                     |
| System         | Administrator | **Possible malfunction Data supplier historical**: GT informs the main administrator that a data source for historical prices may no longer be working.                                  | GT message            |
| Administrator  | Every user    | **Announcement to all by administrator**: Only the administrator can send a message to everyone.                                                                                       | GT message and e-mail |
| Administrator  | user          | **Personal message from the administrator**: This allows the administrator to communicate directly with a user.                                                                        | GT message and e-mail |
