---
title: "Admin Messages"
date: 2026-02-01T22:54:47+01:00
draft: false
weight: 15
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
The **Admin Messages** tab provides a dedicated view for managing administrative free-text messages between GTNet instances. Unlike the technical system messages in the main view, this view enables administrators to exchange information directly.

### Why a Separate View?
The separation of admin messages from the regular GTNet setup exists for several reasons:
- **Different purpose**: The regular view shows technical system messages such as handshake requests, data requests, and status notifications. Admin messages, on the other hand, serve free-form communication between administrators.
- **Conversations**: Admin messages support replies, enabling ongoing conversations. Technical messages have fixed response types.
- **Visibility control**: Only admin messages have the option to restrict visibility.
- **Clarity**: The separation prevents administrative communication from getting lost among technical notifications.

### Table Overview
The table shows all known GTNet instances with the following columns:
- **URL**: The address of the remote instance.
- **Time Zone**: The server's time zone.
- **Admin Messages**: Total number of admin messages with this instance.
- **To Be Answered**: Number of incoming messages that have not yet been answered.
- **Answer Expected**: Number of sent messages still awaiting a response.

Rows can be expanded using the arrow icon to display the associated message threads.

### Sending Admin Messages
Admin messages can be sent to one or multiple recipients. The system behaves differently depending on the scenario:

{{< mermaid >}}
flowchart TD
    A[User wants to send admin message] --> B{Number of recipients?}
    B -->|Single| C[Reply in message thread]
    B -->|Multiple| D[Select domains via checkboxes]
    C --> E[Message sent immediately]
    D --> F[Context menu: Send admin message]
    F --> G[Messages queued for background delivery]
    E --> H[Confirmation: Message sent successfully]
    G --> I[Background task delivers messages]
    I --> J[List updates when complete]
{{< /mermaid >}}

#### Single Recipient
When you want to reply to an existing message in a thread, right-click on the message and select **Reply**. The message is sent synchronously. After successful delivery, the confirmation "Admin message sent successfully" appears.

#### Multiple Recipients
To send an admin message to multiple instances simultaneously:
1. Enable the checkboxes for the desired domains in the table.
2. Open the context menu with a right-click.
3. Select **Send admin message**.

The messages are processed asynchronously in the background. You will receive the confirmation "Admin message queued for X recipient(s)". The list updates automatically after a few moments once the messages have been delivered. The delivery is handled by [background task 26](../../../taskdatachangemonitor/taskdescription/#job26).

{{% notice info %}}
When sending to multiple recipients, messages are delivered in the background. The list may update after a few moments.
{{% /notice %}}

### Message Visibility
Admin messages have a visibility setting that determines who can see the message on the recipient server:
- **All Users**: The message is visible to all users on the recipient server.
- **Admin Only**: The message is visible only to administrators on the recipient server.

#### Visibility Inheritance Rule
{{% notice warning %}}
Visibility can only be restricted, never expanded.
{{% /notice %}}

When replying to a message, the following rules apply:

| Original Message | Possible Reply Visibility |
|------------------|---------------------------|
| All Users | All Users or Admin Only |
| Admin Only | Admin Only |

This means: If the original message was marked as "Admin Only", all replies in this thread must also remain restricted to "Admin Only". A message that started as confidential cannot be made public retroactively.

### The Edit Dialog
When creating or replying to an admin message, a dialog appears with the following fields:
- **Message Code**: The type of message. For admin messages, this is "Admin message" or "Admin broadcast message" when sending to multiple recipients.
- **Message**: The free-text content of the message.
- **Visibility**: Dropdown to choose between "All Users" and "Admin Only". This field is only available for admin messages.

{{% notice note %}}
Only users with administrator privileges can send admin messages. Regular users do not see this view in the menu.
{{% /notice %}}
