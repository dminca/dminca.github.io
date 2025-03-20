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

# F.A.Q.

## PiHole lacks an IPv4 address

Even though you've pinned it on the modem somehow PiHole flushed it. This is
how you can pin it.

Since you're using Raspberry Pi OS Lite and don't have the dhcpcd service, you can set a static IP address using the Network Manager tool. Here's how to do it:

### Using nmtui

1. Open the Network Manager text user interface by running:

```bash
sudo nmtui
```

2. Select "Edit a connection" and choose your network interface (either eth0 for Ethernet or wlan0 for Wi-Fi).

3. Navigate to "IPv4 CONFIGURATION" and change it from "Automatic" to "Manual".

4. Add your desired static IP address, along with the subnet mask (usually /24), gateway, and DNS servers.

5. Save the configuration and exit nmtui.

6. Reboot your Raspberry Pi to apply the changes:

```bash
sudo reboot
```

### Alternative Method: Editing Configuration Files

If you prefer editing configuration files directly, you can modify the NetworkManager connection file:

1. Find your connection file in `/etc/NetworkManager/system-connections/`.

2. Edit the file using sudo:

```bash
sudo nano /etc/NetworkManager/system-connections/your_connection_file
```

3. In the [ipv4] section, add or modify these lines:

```
[ipv4]
method=manual
address1=192.168.1.X/24,192.168.1.1
dns=192.168.1.1;8.8.8.8;
```

Replace "192.168.1.X" with your desired static IP, and adjust the gateway and DNS servers as needed.

4. Save the file and restart the NetworkManager service:

```bash
sudo systemctl restart NetworkManager
```

These methods should allow you to set a static IP address on your Raspberry Pi OS Lite system without relying on the dhcpcd service[^1][^3][^5].

Sources
[^1]: How to Set Up a Raspberry Pi Static IP Address - Pi My Life Up https://pimylifeup.com/raspberry-pi-static-ip-address/
[^2]: How to give your Raspberry Pi a Static IP Address - UPDATE https://thepihut.com/blogs/raspberry-pi-tutorials/how-to-give-your-raspberry-pi-a-static-ip-address-update
[^3]: Set a static IP address with nmtui on Raspberry Pi OS 12 'Bookworm' https://www.jeffgeerling.com/blog/2024/set-static-ip-address-nmtui-on-raspberry-pi-os-12-bookworm
[^4]: How to set static IP on Raspberry Pi 4b (64bit - lite) : r/raspberry_pi https://www.reddit.com/r/raspberry_pi/comments/17l10wr/how_to_set_static_ip_on_raspberry_pi_4b_64bit_lite/
[^5]: How to set a static IP address in PiOS? - Raspberry Pi Forums https://forums.raspberrypi.com/viewtopic.php?t=362637
[^6]: How to Set a Static IP Address on Raspberry Pi | Tom's Hardware https://www.tomshardware.com/how-to/static-ip-raspberry-pi
[^7]: RIGHT and WRONG ways to give a STATIC IP to your Raspberry PI https://www.youtube.com/watch?v=VJtIedYfvSk
