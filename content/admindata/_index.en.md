---
title: "Administrator data and message system"
date: 2021-03-29T22:54:47+01:00
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
In the **Message** view, **initial messages** can only be sent to one **user role**. All messages in such a **message topic** are visible to all users of the addressed user role. However, it is also possible to reply privately in such a message topic, i.e. invisible to the other users of the user role. This is the main reason for the existence of the "**Number of responses**" table column.

#### Message topic by system message to user
Messages from the system should be responded to and the possible action carried out. It is possible to **reply** to the system message to a specific user role. This can be used if the recipient is not able to solve the problem described in the system message themselves.

### Setting message
The assignment between **message type** and **communication channel** is made here. GT currently supports the two communication channels **GT message** and **e-mail**. If no individual settings are made by the user, the default settings apply. Only the "Administrator" user role has the option of forwarding the message to another user.
- A message that is sent to several users always goes to the **GT Message** communication channel. The user can also set whether they wish to receive an **e-mail**.

| From           | To            | Message type                                                                                                                                                                           | Standard channel      |
| -------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| System channel | User role     | **Holding a non-active position**: An instrument has the property "Active until or maturity date" if such an instrument is held after this date.                                         | GT message            |
| System         | User role   | **Possible missing interest/dividends**: GT attempts to determine possible missing entries regarding interest or dividends. Unfortunately, this algorithm also generates false warnings. | GT message            |
| User/System    | Administrator | **Blocked user external data/query limit**: The blocked user wants to return to GT. This is also visible in the user settings.                                                           | -                     |
| Administrator  | Every user    | **Announcement to all by administrator**: Only the administrator can send a message to everyone.                                                                                       | GT message and e-mail |
| Administrator  | user          | **Personal message from the administrator**: This allows the administrator to communicate directly with a user.                                                                        | GT message and e-mail |
