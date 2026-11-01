---
title: "Linux installer"
date: 2026-10-12T15:00:00+02:00
draft: false
weight: 10
archetype: "default"
---
The Linux installer sets up Grafioschtrader (GT) as a classic installation on Debian or Ubuntu: the backend runs as a system service, a web server delivers the user interface, and MariaDB stores the data. The result corresponds to a manual installation following the wiki, with the differences described below. How the installer is started and operated is described in the wiki under [Installation with the Linux installer](https://github.com/grafioschtrader/grafioschtrader/wiki/Installation-with-the-Linux-installer); this page explains the structure.

## Structure of the installation

| Component | Task | Reachable from outside |
|---|---|---|
| Service `grafioschtrader` | Runs the GT backend as user `grafioschtrader`, processes the business logic, runs scheduled tasks and connects to the database, the mail server and external data sources. | No, the backend listens only on `127.0.0.1`. |
| Web server nginx or Apache | Delivers the user interface under `/grafioschtrader/` and forwards the interfaces, GTNet and WebSockets to the backend. One own site serves the LAN address over HTTP, a second one the domain over HTTPS; alternatively the routes are included into an existing HTTPS site. | Yes, through port 80 and, with a domain, 443. |
| MariaDB | Stores users, portfolios, transactions, instruments, prices and further application data. An existing MariaDB is reused. | No, local only. |
| certbot | Obtains the certificate from Let's Encrypt and renews it; after every renewal the web server reloads it. Not used with an existing certificate or an upstream proxy. | No |
| DuckDNS timer | If needed, keeps the DuckDNS subdomain on the current address of the computer every five minutes. | No, optional |
| Build tools | Java and Maven build the backend from the source code, Node.js and Angular CLI also the user interface from about 4 GB of memory; on smaller computers the user interface is downloaded ready-made. This applies to the installation as to every update. | No |

The following diagram shows the data flow in operation. Only the web server is reachable from outside.

{{< mermaid >}}
graph LR
    U["Browser"]
    M["SMTP mail server"]
    Q["External data sources"]
    L["Let's Encrypt"]
    DNS["DuckDNS"]
    subgraph H["Computer with Debian or Ubuntu"]
        direction LR
        W["nginx or Apache<br/>user interface"] -->|"API, GTNet and WebSocket"| B["Service grafioschtrader<br/>backend on 127.0.0.1"]
        B -->|"Database access"| D[("MariaDB")]
        C["certbot"]
        T["DuckDNS timer<br/>(optional)"]
    end
    U <-->|"HTTP/HTTPS"| W
    B -->|"E-mail"| M
    B -->|"Prices and events"| Q
    C -.->|"Certificate"| L
    T -.->|"Updates the domain"| DNS
{{< /mermaid >}}

## Course of the installation
The installer first examines the computer and then asks only the questions that apply to it. Before changing anything, it shows the complete plan with all packages, files and services; only after confirmation does it carry it out. Every step is recorded in a journal. If the installation is interrupted, for example by a power failure or a failed step, running it again continues at the same point, with the same answers and passwords.

{{< mermaid >}}
graph TD
    I["Examination of the computer"] --> F["Questions in dialogs"]
    F --> P["Plan with all changes"]
    P -->|"Confirmation"| K["Core: packages, user, source code,<br/>database, encrypted configuration"]
    K --> A["Application: build, service,<br/>first start with database migration"]
    A --> W["Web and TLS: sites, certificate, DuckDNS"]
    W --> E["Mail check"]
    E --> R["Result"]
    K -.->|"Interruption"| N["Running again continues"]
    A -.->|"Interruption"| N
    W -.->|"Interruption"| N
{{< /mermaid >}}

## Where are configuration and data?

| Location | Content and meaning |
|---|---|
| `/home/grafioschtrader` | Start script of the service, `gtvar.sh`, the update scripts and the built backend `grafioschtrader-server-<version>.jar`. The subdirectory `build/grafioschtrader` holds the source code from which every update builds. |
| `application.properties` and `application-production.properties` | In the source code under `backend/grafioschtrader-server/src/main/resources/`. The first holds the encrypted credentials and your settings, the second your own keys and settings of the installer such as the local backend address. See [Settings on application.properties](../../applicationproperties/). |
| `<document root>/grafioschtrader` | The built user interface. The document root is `/var/www/<domain>` or `/var/www/gt` unless you choose another one. |
| `/etc/nginx` or `/etc/apache2` | The own sites `grafioschtrader` and `grafioschtrader-domain`, with an included HTTPS site the snippet `grafioschtrader.conf`. |
| `/etc/systemd/system`, `/etc/sudoers.d`, `/etc/logrotate.d` | Service, the permission for user `grafioschtrader` to stop and start the service, and the weekly rotation of the log. |
| `/var/log/grafioschtrader.log` | The backend log. |
| `/var/lib/gt-install` | Journal and result of the installer. From them, running it again recognises what is already done. |
| `/root/.gt-install` | Passwords and secrets of the installation, readable by root only. |
| MariaDB | The database `grafioschtrader` in the MariaDB data directory, the buffer and lock settings in `/etc/mysql/mariadb.conf.d/60-grafioschtrader.cnf`. |
| `/opt` | A JDK or Maven obtained by the installer as well as `/opt/nodejs-gt` and `/opt/gt-build-tools` when it needs them for the build. |

## Living with existing services
The installer changes nothing it does not own. An existing MariaDB is reused, also when root can log in only with a password; the password is asked once. Other websites on the same nginx or Apache, also those with PHP, answer after the installation as before; the installer checks this before and after every reload of the web server. Other Java and Node.js versions remain the default versions of the computer, and occupied ports are replaced by free ones. An already running DuckDNS updater is recognised, so that no second one is created. If another proxy such as Caddy owns ports 80 and 443, the installer sets up no web server of its own and writes the routes as a proposal for that proxy to `/root/gt-install-webserver.conf`.

## Update
An update works as for every classic installation with `./gtupdate.sh` as user `grafioschtrader`, see [Update Grafioschtrader](../../newversion/). It fetches the new source code, carries your values from `application.properties` over into the new template, leaves `application-production.properties` unchanged and rebuilds backend and user interface.

## Differences to the manual installation
The manual installation following the wiki and the Linux installer lead to the same result. In some points the installer takes another way, so that it does not change other programs on the computer and can be removed completely again.

| | Manual installation (wiki) | Linux installer | Reason |
|---|---|---|---|
| Java | archive in `/usr/local/jdk-25`, registered as the computer's default | distribution package or archive in `/opt/jdk-<version>-<vendor>`; the service uses it directly | Other Java applications keep their version. |
| Node.js | NodeSource package for the whole computer | a suitable existing version or an own Node.js in `/opt/nodejs-gt` | Other Node.js applications keep their version. |
| Angular CLI | global with `npm install -g` | in `/opt/gt-build-tools` | No global npm packages. |
| DuckDNS | cron script | systemd timer that updates IPv4 and IPv6 according to your choice | Also works on IPv6 connections such as DS-Lite. |
| Log | without rotation | rotated weekly, eight copies | The log does not grow without limit. |
| TLS | `certbot --nginx` or `--apache` rewrites the site | certbot through the document root, own HTTPS site | Foreign sites stay unchanged. |
| Backend | with nginx on all addresses, with Apache through AJP | only on `127.0.0.1`; the addresses of upstream proxies are in `application-production.properties` | Only the web server reaches the backend, and the lockout after failed logins counts the user's real address. |
| Installer only | | swap file of 2 GB when the computer has less than 4 GB of memory and no swap; firewall rules for ufw; continuing after an interruption; uninstall | |
