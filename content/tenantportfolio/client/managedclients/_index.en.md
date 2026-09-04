---
title: "Managing multiple clients"
date: 2026-06-15T22:54:47+01:00
draft: false
weight: 10
archetype: "default"
---
Until now a user was tied to exactly one **client**. A user can now look after several **clients** and switch between them. This makes it possible, for example, to model the situation of an asset advisor who manages the **portfolios** of several customers. In addition, a **client** can be given its own read-only login so that the customer can view their own **portfolios** while the advisor maintains them. If, on the other hand, you only want to give a person read access to your **own** portfolio without creating a separate client, use [Share read access to your portfolio]({{< relref "/tenantportfolio/client/sharereadaccess" >}}). The common basics of both functions are described under [Shared access – basics]({{< relref "/tenantportfolio/client/sharedaccess" >}}).

## The two roles
This function involves two opposing roles. The **advisor** is an ordinary user who creates, maintains and switches between several **clients**. The **client with read-only access** is the customer who signs in solely to view their own **portfolios**. One and the same **client** is therefore managed by the advisor and read by the customer.

{{< mermaid >}}
graph LR
    B[Advisor] -->|manages| K1[Client A]
    B -->|manages| K2[Client B]
    B -->|manages| K3[Client C]
    K1 -->|read-only login| C1[Customer A]
    K2 -->|read-only login| C2[Customer B]
{{< /mermaid >}}

## From the advisor's perspective
For a user to act as an advisor, an administrator must grant them at least the role **User without limits**. The advisor's activities are reached through the **Client** menu, whose structure is described under [Shared access – basics]({{< relref "/tenantportfolio/client/sharedaccess" >}}).

### Creating a client
1. In the **Client** menu choose **Create client**.
2. In the dialog enter the customer's **e-mail address** and assign a **password**; for safety the password has to be entered twice.
3. Confirm with the **Create client** button.

A new **client** is then created in the advisor's currency, to which the advisor immediately has full access. The customer automatically receives the login details – login name and password – by e-mail. The advisor then sets up the new client's **portfolios** and accounts just as for their own client, by first switching into it.

### Switching to a client
1. In the **Client** menu choose **Switch to client**.
2. The **My clients** dialog lists the managed clients with the columns **E-mail** and **Client name**. The client you are currently in does not appear in the list.
3. In the row of the desired client click the switch symbol.

The application reloads and now shows the chosen client's portfolio. The advisor works in it with full editing rights, exactly as if they were signed in with their own **client**.

### Deleting a client
1. In the **Client** menu choose **Switch to client** to open the **My clients** dialog.
2. In the row of the client concerned click the delete symbol – the waste basket.
3. Confirm the safety prompt.

This permanently removes the entire portfolio, all data and the customer's read-only login. The **client** you are currently in cannot be deleted; to do so, first switch to another client or back to yourself.

### Back to your own client
With the **Back to my client** entry in the **Client** menu the advisor can return from a managed client to their own **client** at any time.

## From the client's perspective
The customer receives an e-mail with the login name, which matches their e-mail address, together with a password. With these details they sign in through the usual login. They then have a pure read-only view of their own **portfolios**. How read-only mode behaves is described under [Shared access – basics]({{< relref "/tenantportfolio/client/sharedaccess" >}}).

{{< mermaid >}}
sequenceDiagram
    participant B as Advisor
    participant GT as Grafioschtrader
    participant K as Customer
    B->>GT: Create client with e-mail and password
    GT->>K: E-mail with login details
    GT-->>B: Full access to the client
    K->>GT: Sign in with e-mail and password
    GT-->>K: Read-only view of the portfolios
{{< /mermaid >}}

## Permissions in detail
The following overview shows which activities are available to the two roles. In each **client** they manage, the advisor has the same rights as in their own.

| Activity | Advisor | Client with read-only access |
| --- | :---: | :---: |
| View portfolios, reports, transactions, accounts and watchlists | ✓ | ✓ |
| Create, change or delete data | ✓ | ✗ |
| Switch between managed clients | ✓ | ✗ |
| Create a new client | ✓ | ✗ |
| Delete a managed client | ✓ | ✗ |
| Change own password | ✓ | ✓ |
| Change own language | ✓ | ✓ |
| **Client** menu visible | ✓ | ✗ |
