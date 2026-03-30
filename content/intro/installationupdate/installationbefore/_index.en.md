---
title: "Different installations"
date: 2021-04-16T18:54:47+01:00
draft: false
weight: 55
archetype: "default"
---
Grafioschtrader (GT) can be installed for different uses.

The following video answers these questions:
+ Repository of GT data, tables are shared among users
+ On which infrastructure can GT be operated
+ Why is GT based on JWT?
+ Why does it need a DNS and TLS certificate

Unfortunately only in German:
{{< youtube sF8iGgKRupE >}}

### Local installation without DNS and SSL/TLS certificates
It is perfectly possible to install GT locally or in a local network. Software developers usually work with a local version. You can also use GT in a local network without Domain Name System (DNS), but without an SSL/TLS certificate.
+ **Advantages**:
  + Installation is easier, no DNS and TLS certificate.
  + Safe from attacks outside the local network.
  + The shared data also belongs to you and you have the administrator's user rights.
+ **Disadvantages**:
  + You have to manage all your **shared data** yourself.
  + In the **future**, GT will make it possible to send **historical course data** between **GT instances**, thus reducing dependencies on external data sources.
  + A future smartphone application will not be able to access your GT instance.

### Installation with DNS and SSL/TLS certificates
You can install a **GT instance** for your sole or general use. You use **DNS** and an **SSL/TLS certificate**.
+ **Advantages**:
  + You can **benefit from the other users** as there is shared management of the **shared data**.
  + You can enjoy the benefits of GT further development with regard to communication between GT instances.
+ **Disadvantage**:
  + Installation is more complicated.

#### Self-hosted with DDNS/DNS and TLS certificate
A small installation with a few users already runs on a private [Raspberry Pi 4](//www.raspberrypi.org/products/raspberry-pi-4-model-b/). You can get a TLS certificate free of charge from [Let's Encrypt](//letsencrypt.org/). Free dynamic DNS is available from [DuckDNS](//www.duckdns.org/), for example. Unfortunately, the upload speed of many Internet providers is still very low, which can lead to delays when accessing your home server connected to the Internet.

Alternatively, there are now inexpensive virtual private servers (VPS), which do not have the disadvantage of upload speeds or a changing IP address. In addition, you do not have to worry about the firewall of your home router.

#### Provider as (SaaS)
If you have the appropriate computer resources on the Internet, you can also offer GT as **Software as a Service** (SaaS). There are a number of options available today, such as root and virtual servers.

## Why a TLS certificate
GT's **user authentication** is based on the **JSON Web Token**. A **TLS server certificate** is required for GT to communicate on the Internet, otherwise a secure connection between the client and server or server and server cannot take place. This also protects your sensitive financial data.

### JSON Web Token (JWT)
For a long time, cookies were used for user authentication on the web. This works very well for certain applications, but has the disadvantage that the authentication process requires a web browser. GT is a single-page web application (SPA) that primarily communicates with the back end via a REST API. This REST API can also be used by a non-web browser. GT's authentication is therefore based on the **JSON Web Token** (JWT). Whoever has this **JWT** has the **authentication** key and therefore **access** to the data that is protected with it. To ensure that this **JWT** does not fall into the "wrong hands", communication in the public network must take place via an **HTTPS connection**.
