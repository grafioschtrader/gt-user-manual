---
title: "Data and user rights"
date: 2021-03-26T22:54:47+01:00
draft: false
weight: 20
archetype: "default"
---
## Data
In GT, there is **private** and **shared data**. The visibility and changeability of **shared data** is regulated by **user rights**. Data such as "Portfolios" and "Transactions" are **private data** and cannot be accessed by other users. The information classes "asset classes" and "stock exchange", for example, are **shared data** and visible to all users, but the user's **authorization** determines their **modifiability**.

### Private data
Account, transaction etc. are **private data** and cannot be seen or changed by another user.

### Shared data
In the case of **shared data**, the **access rights** of the user or whether the user is the owner of the **entity** determine whether an entity **can be changed**.

#### Owner of an entity
The **user** who enters an **entity** of an **information class** of shared data automatically becomes the **owner** of this **entity**. They can edit this **entity** regardless of their **user rights**. Users in the **privileged or administrator group** can also change this **entity** without restriction; all other users can suggest a change via a [data change request](../../basedata/).

#### Optimistic locking
GT uses **optimistic locking** based on a version number. With **optimistic locking**, each entity has an attribute that acts as a version number. Each entity has a **version number** which is compared with the version number in the database when updating or saving. If these version numbers do not match, this means that another user has changed the entity before you. The update fails because you modified an outdated version of the entity. If this is the case, repeat the process by retrieving the entity again and updating a new version. The optimistic lock prevents you from accidentally **overwriting** **changes** **made** by other **users**. It also prevents other users from inadvertently overwriting your changes.

## User rights
When a user is **fully registered**, they are automatically assigned to the user role **User with limits**. The exception to this is the **main administrator**, see ["GT after installation](../installationupdate/installationafter)". GT defines the following four **user roles**:

### Users with limits
User with limit can only create or change a **certain number** of **entities** of an **information class** or **its shared entities** per day. This prevents a user from causing major deliberate damage to the **shared data**. This limit also includes the **data change request**, for more information see ["Global settings](../../admindata/globalsettings)".

### User without limits
This user is not subject to any restrictions regarding the creation or modification **of their shared data**.

Unfortunately only in German:
{{< youtube pNwCed37m8Q >}}

### Privileged user
This user has the same user rights as users without limits plus they can also change the **shared data** of all users. The user in this group is not allowed to edit the **global trading calendar** or the **global settings**.

### Administrator
They can change all **shared data**. They can also change global and some user settings.

#### Main administrator {#mainadmin}
The **main administrator** is defined with the e-mail address in `application.properties` under the property `gt.main.user.admin.mail`. He receives the alarm messages from GT directly as an e-mail.

{{% notice style="info" title="User ID instead of nickname" %}}
 When registering with GT, a nickname is requested. This is currently only visible to the administrator. When referencing a user, the user ID is displayed instead of the nickname. This ID is used, for example, for data change requests and messages 
 {{% /notice %}}

## Protection of external data and the request limit
The GT **client** communicates with the **back end** via a **REST API**. This REST API is a general interface and could also be used by another service. However, the **private data** must remain specially protected. Targeted access to the private data of other users is recognized and penalized. In the event of multiple breaches, the user is blocked; the global setting of **gt.max.security.breach.count** determines this limit.

### Request limit
GT implements a **request limit** using a **token bucket** on a minute and hour basis. This means that only a limited number of requests per user can be successfully issued to the **back-end** in the specified time periods. This rate limit is most frequently used to avoid a lack of resources and thus improve the availability of API-based services. In daily use, this request limit is rarely exceeded, otherwise the user receives a message. In the event of repeated violations and if the global setting of **gt.max.limit.request.exceeded.count** is exceeded, this user is locked out of GT.
