---
title: "Register"
date: 2026-11-17T12:00:00+01:00
draft: false
weight: 30
archetype: "default"
---
For registration, the user must fill out two forms. The first for his data and the second for the client's data. In between he has to confirm an email and log in to GT for the first time. The registration process ends with the login form.
{{< mermaid >}}
sequenceDiagram
    autonumber
    participant User
    participant A as Sign in form
    participant R as Register form
    participant K as Clinet form
    participant Backend
    User->>A: Select register
    A-->>R: Redirect
    User->>R: Register user
    R-->>Backend: Check input
    Backend-->>User: Email with confirmation URI  
    User->>Backend: Confirms URI
    Backend-->>User: Redirect Sign in form
    User->>A: User logs in
    A-->Backend: Check authentication
    Backend-->>User: Redirect Client form
    User->>K: Enter client
    K-->>Backend: Check input
    Backend-->>A: Redirect Sign in form
{{< /mermaid >}}
It looks easier in the video...

Unfortunately only in German:
{{< youtube C9MKBfUXLPA >}}

## Eigenschaften
- **Nickname**: This nickname is unique per GT instance, otherwise you could not be distinguished from the other users. Currently, this is not yet used between users.
- **E-Mail**: Must be one unique to the GT instance, otherwise GT could not distinguish you from other users.

## Logging in
You can **log in** to GT multiple times, even with the same web browser. However, you are responsible for ensuring that the corresponding views of the user interface display the current status. The **GT backend** will not propagate the changes of one **user session** to the other user session.

## Authentication and security
["Different installation types](../../intro/installationupdate/installationbefore)" explains why GT uses a JSON Web Token (JWT). This JWT has an expiry time that can be set by the administrator under "[Global settings](../../admindata/globalsettings)". There is no automatic renewal of the JWT, i.e. the user must log in to GT again after the expiry time. Therefore, the expiry time of the JWT should not be set too short, we recommend 1440 minutes, which corresponds to 24 hours. This JWT is stored in the session memory of the web browser. This session memory is deleted when you log out of GT or close the browser. To protect the JWT from theft, you should therefore **log out** of GT or close the web browser.

Changing your password or another sign-in method ends previous browser sessions at their next request. You remain signed in in the session where you make the change. An already open WebSocket connection remains until it reconnects.

## Forgot password
Choose **Forgot password** on the sign-in page, enter your email address and choose **Request reset code**. If the account is eligible, an email arrives with a code that can be used once. The page does not reveal whether an address is registered. Unconfirmed and demo accounts cannot use this function.
The email always contains a code to copy. If the administrator configured the public address under `g.public.base.url`, it also contains a link. Open that link or enter the code on the reset page, fill in **New password** and **Confirm new password**, then choose **Reset password**. The instance password policy applies. The code is valid for 30 minutes. Another request within five minutes sends no second email and keeps the first code valid. After that, a new request replaces the previous code.
A successful reset ends all browser sessions and revokes all personal access tokens and AI-assistant authorizations. Sign in with your new password and reconnect any assistants you need. A security notice is sent by email. A reset does not lift a block caused by usage or security limits. The phone application has no reset page; use the browser.

The reset changes only the password. An enabled authenticator app, the recovery codes and your passkeys remain; after the reset the sign-in therefore still asks for the app code.

## Authentication
Every account has a password with which you sign in together with your email address. The administrator can set certain defaults for the password strength, see "[Global settings](../../admindata/globalsettings)". In the [Sign-in security]({{% relref "/intro/settings#sign-in-security" %}}) dialog under **Settings** you can additionally register passkeys and enable an authenticator app as second factor. The following diagram shows what the sign-in asks for.
{{< mermaid >}}
flowchart TD
    A[Sign-in page] --> B{Sign-in method}
    B -->|Sign in with passkey| C[Device verifies by fingerprint, face or PIN]
    C --> Z[Signed in]
    B -->|Email and password| D{Authenticator app enabled?}
    D -->|No| Z
    D -->|Yes| E[Enter app code or recovery code]
    E --> Z
{{< /mermaid >}}

### Sign in with passkey
When a passkey is possible, the sign-in page shows the button **Sign in with passkey**. You do not enter an email address: the browser offers the passkeys you saved for this instance, and your device verifies you with fingerprint, face or PIN. This completes the sign-in; no password and no authenticator code is asked for. If you cancel the browser dialog, no error message appears. The phone application offers the same button under the same conditions. A passkey is available on the phone if it was created there or a password manager synchronized it from another device.
The button is missing when the browser does not support passkeys or when GT is not opened under its public address with a host name, for example through an IP address. A passkey sign-in does not check whether your password still meets the current password rule; that happens at the next sign-in with the password. If your account is locked because of exceeded limits, sign in with email and password; only there can you request its release.

### Second factor
When an authenticator app is enabled, the field **Authenticator code or recovery code** appears after email and password. Email and password stay filled in; you add the six-digit app code or one of your recovery codes and choose **Sign in** again. Spaces and hyphens in the code do not matter. Each code is valid only once. If the sign-in has to be repeated with a note, for example for a release request, enter a new code there, because the first one has been used up. The phone application asks for the code as well.

### Brute force attack
After **5** failed login attempts on the same IP address, the GT backend will not make any login attempts for this blocked IP address for a certain period of time.
Wrong second-factor codes, by contrast, count per account and do not block the IP address. After five wrong codes within 15 minutes, GT accepts no code for this account for 15 minutes. A correct code resets the count.
