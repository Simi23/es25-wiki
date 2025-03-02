---
title: S2S Strongswan - RRAS (PSK - old_syntax)
description: Site-to-site VPN between Windows and Linux using RRAS and Strongswan
published: true
date: 2025-03-02T09:52:45.989Z
tags: linux, windows
editor: markdown
dateCreated: 2025-02-17T10:44:52.793Z
---

# Information

The tunnel will be established between a Debian and a Windows host. Both share the network `10.0.0.0/24`, and they also have their own networks.

 - Debian shared: `10.0.0.101`
 - Debian private: `10.10.0.254/24`
 - Windows shared: `10.0.0.103`
 - Windows private: `10.30.0.254/24`

# Strongswan setup

This guide will use the old configuration syntax.

## Install Strongswan

```bash
apt install strongswan strongswan-starter
```

## Configure Strongswan

Edit `/etc/ipsec.conf`.

```c
config setup
        charondebug="ike 2, knl 2, cfg 2"

conn windows
       auto=start
       keyexchange=ikev2
       left=10.0.0.101
       leftsubnet=10.10.0.0/24
       right=10.0.0.103
       rightsubnet=10.30.0.0/24
       rightsourceip=10.255.255.0/24
       authby=psk
       ike=aes256-sha2_256-modp1024
       esp=aes256-sha2_256
       dpdaction=restart
```

Add the PSK to `/etc/ipsec.secrets`
```c
10.0.0.101 10.0.0.103 : PSK "Passw0rd"
```

Add the strongswan-starter daemon, and restart it.

```bash
systemctl enable strongswan-starter
systemctl restart strongswan-starter
```

# RRAS Setup

## Install RRAS

Install the **Remote Access** role with features ***DirectAccess and VPN (RAS)*** and ***Routing***.

Deploy **VPN only**.

When configuring RRAS, select **Custom configuration**, with **Demand-dial connections** and **LAN routing** selected.

## Tunnel configuration

Create a new demand-dial interface:

 - Enter a name
 - Select **VPN**
 - Select **IKEv2**
 - Enter the IP address of the other host, in this case, `10.0.0.101`.
 - Only **Route IP packets** is checked
 - Add a route for the other private network, in this case, `10.10.0.0/24`.
 - Skip **Dial-out credentials**.

Open the properties of the new interface:

 - **Options** > **Persistent connection**
 - In **Security**, select **Use preshared key for authentication** and enter the key, in this case, `Passw0rd`.

Open RRAS Properties window:

 - In **Security**, check **Allow custom IPSec policy for L2TP/IKEv2** connection and enter the pre-shared key, in this case, `Passw0rd`.

Restart the RRAS service for good measure.