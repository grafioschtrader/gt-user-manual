---
title: "Getting Started with GTNet"
date: 2026-08-31T22:54:47+01:00
draft: false
weight: 3
archetype: "default"
---
{{% notice style="warning" icon="fa fa-wrench" title="Work in Progress" %}}
The implementation of GTNet is not yet complete. This documentation describes the planned and partially implemented functionality.
{{% /notice %}}
This page is a step-by-step guide to setting up your own GT instance for participation in the GTNet network. What GTNet is, which roles an instance can take and where in the program it takes effect is described in the [overview](../).

## Prerequisites
To participate in GTNet, your own Grafioschtrader server must be accessible from outside via HTTPS. Ensure that your server is properly configured for machine-to-machine (M2M) communication. If you encounter issues with external connectivity, consult the [Problem Solving Guide](https://github.com/grafioschtrader/grafioschtrader/wiki/Problem-solving) in the wiki.

## Step 1: Enable GTNet in Global Settings
First, activate GTNet functionality via the [Global Settings GTNet](../globalsettings/):
- Set **g.gnet.use** to a value greater than 0 to enable GTNet
- Optionally enable **g.gnet.use.log** to track data exchanges

## Step 2: Register Your Own Instance
Before communicating with other participants, you must register your own server in the [GTNet Setup](../setup/) view:
1. Open the GTNet Setup view
2. Create an entry for your own server with your domain URL
3. After saving, the system stores your instance ID in the global setting **g.gnet.my.entry.id**

{{% notice tip %}}
If you are migrating an existing database to a new server, you should export the GTNet data on the old server before migration and then import it on the new server. This preserves authentication tokens, peer connections, and message history. See [Export and Import of GTNet Data](../setup/#export-and-import-of-gtnet-data) for details.
{{% /notice %}}

## Step 3: Add Remote Instances
Add one or more remote GTNet instances to establish communication:
1. In the [GTNet Setup](../setup/) view, add entries for remote servers
2. **Recommendation**: Prefer counterparts operating in "Push Open" mode, as they maintain an active data pool and enable bidirectional exchange without requiring local instruments
3. Configure the daily query limit and other settings as needed

## Step 4: Establish Initial Connection
Request data exchange directly, which automatically handles the handshake process:
1. Select the remote server entry
2. Use the context menu to send a **Request for Data Exchange** message
3. The system automatically performs the handshake and token exchange if this is the first contact
4. Wait for the response (acceptance or rejection)
5. Once accepted, the data exchange is activated and authentication tokens are stored for future communication

## Step 5: Configure Automatic Answers (Recommended)
To avoid manually responding to every incoming request, configure automatic response rules in the [Automatic Answer](../autoanswer/) view:
1. Define rules for different message types (e.g., first contact, data exchange requests)
2. Set conditions using variables like time of day, daily request count, or domain patterns
3. Specify whether to accept or reject requests automatically
4. Configure waiting times after rejections to prevent repeated requests

{{% notice tip %}}
Without automatic answer rules, every incoming request requires manual approval by the administrator. Setting up appropriate rules significantly reduces administrative overhead.
{{% /notice %}}

## Step 6: Configure Securities for Exchange
Define which instruments should participate in data exchange:
1. Open the [Exchange Price Data](../exchange/) view
2. For each security or currency pair, configure the four exchange options:
   - Receive intraday price
   - Receive historical price data
   - Send intraday prices
   - Send historical price data
3. Save your changes

## What Comes Next
Once the exchange is running, [Monitoring GTNet](../monitoring/) shows where to see whether data is actually flowing, and with whom.
