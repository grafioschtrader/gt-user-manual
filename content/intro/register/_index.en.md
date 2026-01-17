---
title: "Register"
date: 2021-03-23T11:00:00+01:00
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

{{% notice style="info" title="Change JWT and password" %}} 
A JWT loses its validity when the expiration date is exceeded, therefore several JWTs of one user can be valid at the same time. Changing the password does not invalidate the previous JWT, i.e. the JWT should not fall into the "wrong hands". We believe that invalidating JWTs is not necessary for GT as we do not see any major risk of cross-site scripting (XSS) or cross-site request forgery (CSRF) attacks 
{{% /notice %}}.

### Authentication
GT currently only requires the e-mail address and a password to log in. The administrator can set certain defaults for the password strength, see "[Global settings](../../admindata/globalsettings)".

### Brute force attack
After **5** failed login attempts on the same IP address, the GT backend will not make any login attempts for this blocked IP address for a certain period of time.
