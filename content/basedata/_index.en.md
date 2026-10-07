---
title: "Base Data - Data change request"
date: 2021-04-18T22:54:47+01:00
draft: false
weight: 25
archetype: "default"
---
## Base data
**Base data** is **shared data** that is used and modified by the user of a **GT instance**.

### Static main element "Base data"
The function **Data change request** is implemented on the static main element **Base data** in the **navigation area**. Although the **entities** are shared by certain information classes, they cannot be changed at will by every user.

## Data change request
The interaction of **shared data**, the **owner of an entity** and **user rights** can lead to you creating or receiving a **data change request**.
+ Users in the **Limits** or **without Limits** group create a **data change request** if they edit and save a **shared entity**. You receive a **data change request** if someone in the **Limits** or **without limits** group wants to change one of the **shared entities** they have created.
+ A user with **privileged or administrative user rights** never creates a **data change request**. However, they will receive all **data change requests**, even if the **entity** in question was originally created by another user.

In the following video we see this in a practical example: 
Unfortunately only in German:
{{< youtube zNXjOknUZjo >}}

### Restrictions on data change requests
Certain data change requests are not supported for certain entities.

### Derived instrument
No data change request can be made for the **price calculation formula**.
