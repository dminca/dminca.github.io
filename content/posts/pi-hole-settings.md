---
title: "Pi-Hole Settings"
date: 2025-03-16T14:50:33+01:00
draft: false
description: "My Pi-Hole configuration"
tags: [ privacy ]
comments: true
toc: true
---

Just wiped my RaspberryPi 3b+ and figured I forgot to back-up some PiHole
settings 🤦‍♂️ (go figure). This is my attempt at keeping a tab there and preventing this from
happening again.

# AdLists

1. https://raw.githubusercontent.com/TheShawnMiranda/LG-TV-Ad-Block/master/list
1. https://big.oisd.nl

# PiHole settings

## DNS

1. AuthN to [nextdns.io](https://nextdns.io)
1. copy IPv4 & IPv6 of NextDNS
1. come back to PiHole
1. Settings → DNS → Custom DNS servers
1. *paste them in this box*
1. *Save&Apply*

## DHCP

1. Settings → DHCP → DHCP Settings
1. toggle **DHCP server enabled**
    - input a range, eg. `x.x.x.100 - 200`
1. toggle **Enable additional IPv6 support (SLAAC + RA)**
1. Advanced DHCP Settings
1. DHCP lease time: `1d`
1. check **Advertise DNS server multiple times**

{{< alert "lightbulb" >}}
**Advertise DNS server multiple times** is needed here otherwise during
DSL outages you'll suffer DNS issues, meaning if you try pinging the PiHole
station you'll get

```sh
Request timeout for icmp_seq 0
ping: sendto: No route to host
```

There's no point to discuss what'll `nc -zv x.x.x.x 22` return.
{{< /alert >}}


# Modem (router) settings

## DNSv4/v6 setup

1. Internet → Zugangsdaten → DNS Server
1. DNSv4-Server
    - Andere DNSv4-Server verwenden
    - *add IPv4 of RaspberryPi*
1. DNSv6-Server
    - Andere DNSv6-Server verwenden
    - *add IPv6 of RaspberryPi*

## DHCPv4 setup

1. Heimnetz → Netzwerkeinstellungen
1. IP-Adressen → IPv4-Einstellungen
1. uncheck **DHCP-Server aktivieren**


{{< alert "triangle-exclamation" >}}
**DHCP-Server aktivieren** needs to be disabled because PiHole acts as DHCPv4
server. This is needed in order to have **client hostnames** shown in PiHole
dashboard.
{{< /alert >}}
