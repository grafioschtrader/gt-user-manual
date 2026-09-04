---
title: "Installation und Update"
date: 2026-08-29T12:00:00+02:00
draft: false
weight: 50
archetype: "default"
---
Die folgenden Informationen sind für den "normalen" Benutzer von Grafioschtrader nicht relevant. Im [Wiki des Projekt Grafioschtrader auf GitHub](https://github.com/grafioschtrader/grafioschtrader/wiki) gibt es Anleitungen beim Vorgehen von Installation, Update und Änderungen an application.properties.

## Installation
Aufgrund des Client-Server-Modells ist die Installation von GT umfangreich. Die Videos der YouTube-Playliste [Installation und Update](https://www.youtube.com/playlist?list=PLHbd4_zqbsPJZjoZktsKBQVx7JR_9AuGR) können Ihnen dabei helfen. Vor der Installation empfehlen wir Ihnen den Inhalt von [Unterschiedliche Installationsarten](./installationbefore/). Für eine Installation mit Docker zeigt [Docker-Architektur](./dockerarchitecture/), welche Container benötigt werden und wo Konfigurationen sowie dauerhafte Daten abgelegt sind. Nach der Installation von GT sind einige geteilte Daten standardmässig schon vorhanden, siehe dazu [Nach der Installation von GT](./installationafter/).

## Update Grafioschtrader
Bei den meisten Softwaresystemen ist nach der Installation vor dem Update. Dies ist bei Grafioschtrader nicht anders.

## Einstellungen an application.properties
Gewisse systemweite Einstellungen müssen über `application.properties` bzw. in `application-production.properties` angepasst bzw. erweitert werden. 
