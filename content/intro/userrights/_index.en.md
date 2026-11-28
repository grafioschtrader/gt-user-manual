---
title: "Data and user rights"
date: 2026-11-19T12:00:00+01:00
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
User with limit can only create or change a **certain number** of **entities** of an **information class** or **its shared entities** per day. This prevents a user from causing major deliberate damage to the **shared data**. This limit also includes the **data change request**, for more information see "[Limit information class]({{% relref "/admindata/entitylimit" %}})". If such a user reaches a day limit, they can request an increase via an automatically displayed dialog; the full workflow is described in the section [Change Proposals]({{% relref "/admindata/user" %}}#change-proposals) of the user settings.

### User without limits
This user is not subject to any restrictions regarding the creation or modification **of their shared data**.

{{% callout note %}}
The daily limits are predefined for the role **User with limits** only, but they are not restricted to that role: the administrator can record a daily limit for every user role and also for a single user. In addition, the **shared data** is subject to a total number per creator alongside the daily limit. Both are described under [Limit information class]({{% relref "/admindata/entitylimit" %}}).
{{% /callout %}}

Unfortunately only in German:
{{< youtube pNwCed37m8Q >}}

### Privileged user
This user has the same user rights as users without limits plus they can also change the **shared data** of all users. The user in this group is not allowed to edit the **global trading calendar** or the **global settings**.

### Administrator
They can change all **shared data**. They can also change global and some user settings.

#### Main administrator {#mainadmin}
The **main administrator** is defined with the e-mail address in `application.properties` under the property `gt.main.user.admin.mail`. He receives the alarm messages from GT directly as an e-mail.

{{% callout style="info" title="User ID instead of nickname" %}}
 When registering with GT, a nickname is requested. By default it is only visible to the administrator. In that case the user ID is displayed instead of the nickname when a user is referenced. The administrator can make the nickname visible to all users of the instance in the [global settings]({{% relref "/admindata/globalsettings" %}}). This ID is used, for example, for data change requests and messages 
 {{% /callout %}}

## Visibility in the user interface
In many web applications, especially SaaS offerings, an ordinary user sees only a small part of the software. Settings, monitoring and administration live in a separate back-office application that only the operator can access. There are thus two applications, one for the back office and one for the users. GT deliberately takes a different path: for the sake of **transparency**, as much data as possible is shown to all user roles. In GT, the **user role** therefore mainly determines what a user may **change**, and only in a few cases what they may **see**.

For a new user, the wealth of functionality offered in the user interface may feel somewhat overwhelming at first. Much of what lies under the main element **Administrative data - Messages** concerns the operation of the instance and is not needed for managing one's own assets. This openness has an advantage, though: since a user has a view similar to the administrator's regardless of their role, they can spot problems themselves, for example incomplete price data or a failed background task, and point the administrator to them with a message.

### Visible to all, changeable by the administrator
The areas [Global trading calendar]({{% relref "/admindata/tradingcalendar" %}}), [Completeness of securities EOD]({{% relref "/admindata/historyquotequality" %}}), [Global Settings]({{% relref "/admindata/globalsettings" %}}), [GT Net and Message]({{% relref "/admindata/gtnet" %}}), [Batch processing monitor]({{% relref "/admindata/taskdatachangemonitor" %}}) and [Tax data]({{% relref "/admindata/taxdata" %}}) are open to all roles. Only the administrator can change the global trading calendar, the global settings and the tax data. In the batch processing monitor only they can create, copy or interrupt background tasks, and in GTNet only they set up the connections to other instances. Likewise, only the administrator records the system-wide [security actions]({{% relref "/basedata/securityaction" %}}) for an ISIN change. The other roles merely lack the corresponding menu items in these areas.

### Visible to the administrator only
A few data remain reserved for the administrator because they are confidential or concern the users of the instance:
- In the main tree, only the administrator sees the elements [Connector-API-Key]({{% relref "/admindata/connectorapikey" %}}), [User settings]({{% relref "/admindata/user" %}}) and [Limit information class]({{% relref "/admindata/entitylimit" %}}), as well as the element [Automatic message]({{% relref "/admindata/gtnet/autoanswer" %}}) under GT Net and Message.
- Admin messages in GTNet that are intended for administrators only are not shown to the other roles.
- Only the administrator gets the [Limit-change requests]({{% relref "/dashboard/adminwidgets" %}}) card on the dashboard.
- Without release, the nickname of other users is visible to the administrator only, as described above under "User ID instead of nickname".
- Only the administrator can send a message to an individual user and see the list of all users for that purpose. Likewise, only they can choose the roles "User without limits", "User with limits" and "Every user" as recipients, whereas every user can write to the administrator or the privileged users.
- Only the administrator gets a link to a data provider whose address contains the instance's API key.

### Data change requests
The administrator and the **Privileged user** see all open [data change requests]({{% relref "/basedata" %}}#data-change-request), since they can accept or reject them. The other roles see only the requests for entities they own. The same applies to the **Open data-change requests** card on the dashboard, which additionally shows every role their own requests. This is the only difference in visibility that does not concern the administrator alone. There is no difference in visibility between the **User with limits** and the **User without limits**; they differ only in the limits on changes.

## Protection of external data and the request limit
The GT **client** communicates with the **back end** via a **REST API**. This REST API is a general interface and could also be used by another service. However, the **private data** must remain specially protected. Targeted access to the private data of other users is recognized and penalized. In the event of multiple breaches, the user is blocked; the global setting of **g.max.security.breach.count** determines this threshold.

### Request limit
GT implements a **request limit** using a **token bucket** on a minute and hour basis. This means that only a limited number of requests per user can be successfully issued to the **back-end** in the specified time periods. This rate limit is most frequently used to avoid a lack of resources and thus improve the availability of API-based services. In daily use, this request limit is rarely exceeded, otherwise the user receives a message. In the event of repeated violations and if the global setting of **g.max.limit.request.exceeded.count** is exceeded, this user is locked out of GT.
