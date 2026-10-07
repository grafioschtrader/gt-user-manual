---
title: "Share read access to your portfolio"
date: 2026-06-15T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
With this function you give another person read-only access to your **own** portfolio. You keep your **client** and continue to maintain it yourself; the other person may only read. This is what distinguishes the function from [Managing multiple clients]({{< relref "/tenantportfolio/client/managedclients" >}}), where an advisor creates a **separate** client and builds the portfolio for the customer. The common basics – the **Client** menu, switching and the read-only mode – are described under [Shared access – basics]({{< relref "/tenantportfolio/client/sharedaccess" >}}).

## Who may share
Any registered user can share read access to their own portfolio, as long as they are in their own **client** and do not themselves have read-only access. The **Share read access** entry in the **Client** menu is used for this.

## Two outcomes
Depending on whether the recipient already has an account in this Grafioschtrader installation, there are two outcomes:
- **Already registered**: the person is granted read access to your portfolio. They then switch into your **client** via **Switch to client** and see your portfolio in read-only mode.
- **Not yet registered**: a read-only login is created and the assigned password is sent to the person automatically by e-mail. After signing in they immediately see your portfolio in read-only mode, without owning a portfolio of their own.

## Granting read access
1. In the **Client** menu choose **Share read access**.
2. Enter the **e-mail address** of the desired person and click **Check e-mail**.
3. If the person is already registered, no password is requested. If they are not yet registered, the fields for the **password** appear, which for safety has to be entered twice.
4. Finish with the **Grant read access** button.

Your own e-mail address as well as people who already have read access are rejected; in that case a message appears and the entry starts again from the beginning.

## Managing shared viewers
The **Shared viewers** entry in the **Client** menu opens the **Who can read my portfolio** dialog. It lists everyone with their **e-mail** and the kind of access:
- **Registered user**: a person with their own account who was granted read access.
- **View-only login**: a purpose-made read-only login without a portfolio of its own.

With **Revoke read access** the access is withdrawn again. For a **registered user** only the read access is removed; the person's account remains. For a purpose-made **view-only login** that login is deleted entirely.

{{< mermaid >}}
sequenceDiagram
    participant O as Owner
    participant GT as Grafioschtrader
    participant R as Reader
    O->>GT: Share read access with e-mail
    GT-->>O: Check e-mail – not yet registered
    O->>GT: Assign password and grant read access
    GT->>R: E-mail with login details
    R->>GT: Sign in with e-mail and password
    GT-->>R: Read-only view of the shared portfolio
{{< /mermaid >}}

{{% notice note %}}
The **Share read access** entry only appears when the function has been enabled for the Grafioschtrader installation. If it is not available, the operator has not activated it.
{{% /notice %}}
