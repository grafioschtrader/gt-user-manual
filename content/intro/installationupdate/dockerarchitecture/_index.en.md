---
title: "Docker Architecture"
date: 2026-09-26T12:00:00+02:00
draft: false
weight: 57
archetype: "default"
---
A Docker installation of Grafioschtrader normally consists of three containers. A fourth container is added only when DuckDNS should keep a changing public IP address of the Docker host up to date. All containers belong to the same Docker Compose project and communicate through an internal network.

{{% notice style="info" title="Three or four containers?" %}}
The `web`, `backend`, and `mariadb` containers are always required. The `duckdns` container is optional. The frontend does not require another container: the compiled Angular application is already included in the `web` container.
{{% /notice %}}

When the images are built from source, temporary Maven and Node.js build stages are also visible. They are used only to create the backend and Angular application and are not additional containers in the running installation.

## Installation structure

| Container | Purpose | Accessible from outside |
|---|---|---|
| `web` | Caddy serves the Angular application, accepts HTTP and HTTPS requests, and forwards interface requests to the backend. When a public domain is configured, Caddy automatically obtains and renews the TLS certificate. | Yes, normally through ports 80 and 443. |
| `backend` | Runs Grafioschtrader, processes its business logic, performs scheduled tasks, and connects to the database and external data and mail services. | No, only through the `web` container. |
| `mariadb` | Permanently stores users, portfolios, transactions, instruments, prices, and other application data. | No, only within the internal Docker network. |
| `duckdns` | Optionally keeps a DuckDNS subdomain pointed to the current public IP address of the Docker host. | No; this container is optional. |

The following diagram shows the normal data flow. Only the `web` container publishes ports on the Docker host. The backend and database remain inside the internal Docker network.

{{< mermaid >}}
graph LR
    U["Browser"]
    M["SMTP mail server"]
    Q["External data providers"]
    L["Let's Encrypt"]
    DNS["DuckDNS"]
    subgraph H["Docker host"]
        direction LR
        W["web<br/>Caddy and Angular"] -->|"API, GTNet and WebSocket"| B["backend<br/>Grafioschtrader"]
        B -->|"Database access"| D[("mariadb<br/>Database")]
        DD["duckdns<br/>(optional)"]
    end
    U <-->|"HTTP/HTTPS"| W
    B -->|"Email"| M
    B -->|"Prices and events"| Q
    W -.->|"TLS certificate"| L
    DD -.->|"Updates the domain"| DNS
{{< /mermaid >}}

The browser loads the user interface under `/grafioschtrader/` from the `web` container. Caddy also accepts requests for the application interfaces, GTNet, and WebSockets and forwards them internally to the backend on port 8080. The backend reaches MariaDB by its Docker service name on port 3306. These two internal ports are not published on the Docker host.

## Where are configuration and data stored?

Containers may be replaced during an update. Persistent data is therefore stored outside their writable container layers, either as a file in the Docker directory or in a volume managed by Docker.

| Storage location | Content and purpose |
|---|---|
| `docker/.env` | Contains infrastructure settings such as database passwords, the JWT secret, administrator email, mail server, domain, published ports, memory sizes, and the selected GT version. This file contains secrets and must be backed up together with the database. |
| `docker/config/application-production.properties` | Contains additional settings that control Grafioschtrader behaviour, such as schedules or logging levels. The directory is mounted read-only as `/config` in the backend container. |
| `docker/docker-compose.yml`, Dockerfiles, `Caddyfile`, `install.sh`, and `update.sh` | Define the installation structure or perform installation and updates. They contain no user or portfolio data. From version 0.37.2 on, the update script brings these files to the state of the new version. |
| Docker volume `db_data` | Contains the actual MariaDB data. Docker normally assigns the full name `grafioschtrader_db_data`. The volume remains when containers are stopped or replaced. |
| Docker volumes `caddy_data` and `caddy_config` | Contain certificates and runtime data managed by Caddy. These volumes also remain when the container is replaced. |
| `docker/gt-backup-…`, `docker/gt-env-….bak`, and `docker/gt-config-….bak` | Are created by the update script as database and configuration backups. Manually created database backups can also be stored in the Docker directory. |
| `docker/gt-deployment-….tar.gz` and `docker/.gt-deployment.sha256` | The archive holds the installation files the update script replaced, in their state before the update. The second file records which version of each installation file was delivered last; the next update uses it to recognize your own changes. It should not be edited. |
| Container images | Contain the Grafioschtrader backend or Caddy with the compiled Angular application. They contain no persistent user or portfolio data and are replaced during an update. |

The physical location of a named volume depends on Docker Engine and the operating system. Its internal directory should not be edited directly. Use the Docker Compose commands described in the Docker directory to back up and restore the data.

## Resources on Raspberry Pi and other small computers

The guide for a [classic installation on Raspberry Pi 4 or 5](https://github.com/grafioschtrader/grafioschtrader/wiki/Installation-on-Raspberry-Pi-4-or-5) provides recommendations for Java, MariaDB, swap, and storage. The same general recommendations apply to Docker, but they are configured in different places.

The installation script detects the available RAM once and writes suitable initial values to `.env`:

| Host RAM | Backend Java heap (`JAVA_OPTS`) | MariaDB buffer (`DB_INNODB_BUFFER_POOL_SIZE`) |
|---|---|---|
| About 2 GB | `-Xms128m -Xmx896m` | `384M` |
| About 4 GB | `-Xms256m -Xmx1792m` | `1G` |
| 8 GB or more | `-Xms512m -Xmx2048m` | `2G` |

If Grafioschtrader shares the host with other applications or the amount of RAM has changed, these values can be adjusted in `.env`. Recreate the affected containers afterwards:

```bash
docker compose up -d --force-recreate mariadb backend
```

The Docker host's `/etc/mysql/my.cnf` does not affect the MariaDB container. Additional MariaDB settings are supplied by mounting a separate configuration file into the container. A complete example and cautions about memory-intensive settings are provided under “Memory and storage on Raspberry Pi and SBC hosts” in the [Docker README](https://github.com/grafioschtrader/grafioschtrader/tree/master/docker#memory-and-storage-on-raspberry-pi-and-sbc-hosts). The InnoDB buffer size continues to be controlled by `DB_INNODB_BUFFER_POOL_SIZE` in `.env`.

Swap is configured on the Docker host. It can help when locally building images on a device with little RAM; the published images are preferable for normal operation. The `db_data` database volume resides in Docker's data directory. To place the database on SSD or NVMe storage, Docker's storage location or this volume must therefore be placed there. Back up and verify an existing database before moving it.

## Startup and updates

During startup, the backend waits until MariaDB is ready. It then performs any required database migrations automatically. The `web` container can already accept connections while the backend is still starting. During an update, mainly the images and containers are replaced; `.env`, the `config` directory, and the named volumes remain in place.

### Installation files during an update

A new image does not change the files on the Docker host. If a new version comes with a changed `docker-compose.yml`, for example a renamed setting for the backend or a new health check, that change would never reach the host by itself; in the worst case the new backend then does not start. That is why `update.sh` first fetches the installation files of the requested version: the content of the Docker directory and two helper scripts the update uses to rename settings and to spread the daily download times. Only then does it back up the database and switch the images. If `update.sh` itself changes in the process, the new version takes over and runs the update from the beginning.

A file is replaced only while it is still unchanged, exactly as an earlier version delivered it. If an installation file was edited by hand on the host, the update decides as follows:

{{< mermaid >}}
graph TD
    A["Installation file on the host"] --> B{"Does it match<br/>the new version?"}
    B -->|"Yes"| K["Nothing to do"]
    B -->|"No"| C{"Edited by hand?"}
    C -->|"No"| E["Replaced by the<br/>new version"]
    C -->|"Yes"| D{"Does the new version<br/>change this file?"}
    D -->|"No"| F["Your edit<br/>is kept"]
    D -->|"Yes"| G["Update stops,<br/>nothing is changed"]
{{< /mermaid >}}

When the update stops, it names the affected files. Your own settings belong in `.env` or in the `config` directory whenever possible, because an update never touches those two. After moving the change there, repeat the update with the option `--replace-modified`; the named files are then replaced by the new version. Before replacing anything, the script saves the previous state in a `gt-deployment-….tar.gz` backup, so no edit is lost. With the option `--skip-files`, all installation files stay as they are and only the images are switched.

{{% notice style="warning" title="One-time step when updating from version 0.37.1 or older" %}}
The update script of these versions does not update itself yet. Before updating to 0.37.2, fetch the new update script once, then update as usual. Run the commands in the Docker directory:

```bash
git -C .. fetch --depth 1 origin tag V0.37.2
git -C .. checkout V0.37.2 -- docker/update.sh
./update.sh 0.37.2
```
{{% /notice %}}

The installation, update, backup, and diagnostic commands are documented in the [Docker directory of the Grafioschtrader project](https://github.com/grafioschtrader/grafioschtrader/tree/master/docker).
