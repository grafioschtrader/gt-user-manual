---
title: "Easy installation"
date: 2026-10-12T15:00:00+02:00
draft: false
weight: 56
archetype: "default"
---
There are two guided ways to install Grafioschtrader (GT): the Linux installer for a classic installation on Debian or Ubuntu, and the installation with Docker. Both ask the necessary questions themselves, generate the passwords on request, set up the database and, for a public domain, obtain the TLS certificate from Let's Encrypt. This page compares the two ways; their structure is described on the subpages [Linux installer](./linuxinstaller/) and [Docker Architecture](./dockerarchitecture/). The individual commands are in the wiki under [Installation with the Linux installer](https://github.com/grafioschtrader/grafioschtrader/wiki/Installation-with-the-Linux-installer) and [Grafioschtrader with Docker](https://github.com/grafioschtrader/grafioschtrader/wiki/Grafioschtrader-with-Docker).

## Common prerequisites
Without a domain, GT is only reachable in the local network over HTTP. Access from the internet requires a domain that points to the computer. If you use the free dynamic domain of [DuckDNS](https://www.duckdns.org/) for this, create the account and the subdomain on duckdns.org yourself before the installation and keep the token ready; neither way can create a subdomain. Both ask through which address family the computer is reachable from outside: IPv4 with port forwarding in the router, IPv6 only (for example on a DS-Lite connection) or both. In every case, the router must let TCP ports 80 and 443 through to this computer, as port forwarding for IPv4 and as a firewall allowance for IPv6. Port 80 is needed to issue and renew the certificate.

## Linux installer and Docker compared

| | Linux installer | Docker |
|---|---|---|
| Computer | Debian 12 and 13, Ubuntu 24.04 and 26.04 (also Raspberry Pi OS and Armbian on these bases) with systemd | Linux server with Docker Engine and the Compose plugin, on x86-64 or 64-bit ARM such as the Raspberry Pi |
| What is created | GT as a system service; the backend is built on the computer from the source code, the user interface too from about 4 GB of memory, otherwise it is downloaded ready-made | three containers from ready-made images, optionally a fourth for DuckDNS |
| Web server | nginx or Apache; an existing HTTPS site can take in GT | Caddy in the `web` container |
| TLS certificate | certbot, or an existing certificate, or an upstream proxy | Caddy obtains and renews it itself |
| DuckDNS | own systemd timer; an existing updater is recognised | own container, on request |
| Local network | always additionally through the LAN address over HTTP | with a domain only through the domain |
| Other services on the computer | an existing MariaDB, websites, Java and Node.js versions and occupied ports are recognised and stay unchanged | GT runs separately in containers; only ports 80 and 443 are occupied |
| Update | `gtupdate.sh` as user `grafioschtrader` | `update.sh` in the Docker directory |
| Uninstall | `gt-install.sh --uninstall` | `uninstall.sh` |

## Which way fits?

{{< mermaid >}}
graph TD
    A["Where should GT run?"] --> B{"Debian or Ubuntu<br/>with systemd?"}
    B -->|"No"| D["Docker"]
    B -->|"Yes"| C{"Should GT run beside other services<br/>such as MariaDB or websites?"}
    C -->|"Yes, and they should<br/>be shared"| L["Linux installer"]
    C -->|"No"| E{"Is Docker already<br/>in use?"}
    E -->|"Yes"| D
    E -->|"No"| L
    L --> F{"Does another proxy such as<br/>Caddy own ports 80 and 443?"}
    F -->|"Yes"| G["Linux installer without its own web server;<br/>the proxy takes over the routes"]
    F -->|"No"| H["Linux installer with nginx or Apache"]
{{< /mermaid >}}

The Linux installer suits a computer that GT shares with other applications, or when you want the individual components in front of you as with a manual installation. Docker suits GT running separately from everything else, or a computer that already uses Docker. On a Raspberry Pi with little memory, Docker also saves building the backend, because the images are obtained ready-made.

## Uninstall
An installation made with the Linux installer can be removed with the same program. The command `sudo bash gt-install.sh --uninstall` first shows what will be removed and, after confirmation, removes only what the installer itself set up. Other websites, services and Java versions on the server stay unchanged, as do files that were changed by hand after the installation. The database with your data and the installed program packages are kept by default. Only the options `--remove-database` and `--remove-packages` remove them as well. Before deleting the database, the installer writes a backup to the directory `/root` and additionally asks you to type `delete-database`.
{{% callout info %}}
As long as the kept database remains on the server, Grafioschtrader cannot be installed there again. In this case, restore your data from the backup or from [Export personal data](../../settings/#export-personal-data).
{{% /callout %}}
An installation with Docker is removed with the script `uninstall.sh` in the Docker directory. It keeps the database and the certificates unless they are deleted explicitly with the option `--purge`.
