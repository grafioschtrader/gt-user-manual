---
title: "Installation and Update"
date: 2026-10-12T15:00:00+02:00
draft: false
weight: 50
archetype: "default"
---
The following information is not relevant for the "normal" user of Grafioschtrader. In the [wiki of the Grafioschtrader project on GitHub](https://github.com/grafioschtrader/grafioschtrader/wiki) there are instructions on how to install, update and make changes to application.properties.

## Installation
The client-server model makes installing GT a more extensive process. The videos in the [Installation and Update](https://www.youtube.com/playlist?list=PLHbd4\_zqbsPJZjoZktsKBQVx7JR\_9AuGR) YouTube playlist can help you with it. Before installation, we recommend that you read [Different types of installation](./installationbefore/). The easiest way to install is the Linux installer or Docker; [Easy installation](./easyinstall/) compares the two ways and describes their structure. After installing GT, some shared data is already available by default; see [After the installation of GT](./installationafter/).

## Uninstall
How an installation made with the Linux installer or with Docker is removed is described in [Easy installation](./easyinstall/#uninstall).

## Update Grafioschtrader
With most software systems, after the installation is before the update. This is no different with Grafioschtrader.

## Settings in application.properties
Certain system-wide settings must be adjusted or extended via `application.properties` or in `application-production.properties`.
