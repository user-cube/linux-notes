---
title: "02 - Routing"
description: "IP routing in Linux — routing tables, default gateway, multiple gateways, and ip route commands."
tags:
  - linux
  - networking
  - routing
---

**Routing** allows communication between different networks. When two hosts are on different IP networks, packets must be forwarded by a **router** (or gateway).

A router examines the destination IP address of each packet and forwards it according to its **routing table**.

---

## Viewing the Routing Table

```bash
ip route          # modern — preferred
ip route show     # equivalent, explicit form
route             # legacy (net-tools package, deprecated on many distros)
```

Output includes:
- Connected networks (directly attached subnets)
- Default gateway
- Static routes

---

## Default Gateway

Instead of configuring a specific route for every possible destination, a **default route** is used for all traffic with no more specific match:

```
"If no specific route matches the destination, send the packet to the default gateway."
```

The default route is represented as:

```
0.0.0.0/0
```

or simply:

```
default
```

Both mean the same thing — match any destination.

---

## Multiple Gateways

Some environments have more than one gateway:

- One gateway for an internal private network
- A different gateway for reaching the internet

In this case, multiple routes must be configured so traffic is directed to the correct gateway depending on its destination.

---

## Managing IP Addresses and Routes

> [!warning] Changes made with `ip` are not persistent
> All `ip addr` and `ip route` changes are lost on reboot. To persist configuration, use the appropriate tool for your distribution.

| Distribution | Persistent network config |
|---|---|
| Debian / older Ubuntu | `/etc/network/interfaces` |
| Ubuntu 18.04+ | Netplan (`/etc/netplan/*.yaml`) |
| RHEL / Fedora / Rocky | NetworkManager (`nmcli`, `nmtui`) |

### Assign an IP address

```bash
sudo ip addr add 192.168.1.100/24 dev eth0
```

### Remove an IP address

```bash
sudo ip addr del 192.168.1.100/24 dev eth0
```

### Add a route

```bash
sudo ip route add 10.0.0.0/24 via 192.168.1.1          # route to a specific network
sudo ip route add default via 192.168.1.1               # set default gateway
```

### Remove a route

```bash
sudo ip route del 10.0.0.0/24 via 192.168.1.1
sudo ip route del default
```
