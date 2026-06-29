---
title: "03 - Network Troubleshooting"
description: "A systematic approach to diagnosing Linux network connectivity issues using ip, ping, traceroute, nslookup, and dig."
tags:
  - linux
  - networking
  - troubleshooting
---

When troubleshooting network connectivity, follow a logical sequence — starting at the interface level and working up to DNS.

---

## Step 1 — Verify the Interface is Up

```bash
ip link
```

Check that the interface state is `UP`. If it shows `DOWN`:

```bash
sudo ip link set eth0 up
```

No traffic can flow through a down interface.

---

## Step 2 — Verify the IP Address

```bash
ip addr
```

Confirm that the interface has the expected IP address and subnet mask. A missing or incorrect address means the machine cannot participate in the network.

---

## Step 3 — Verify the Routing Table

```bash
ip route
```

Check whether a suitable route exists for the destination network, and that the default gateway is set correctly. Without a matching route, packets cannot leave the host.

---

## Step 4 — Test Connectivity

```bash
ping -c 4 192.168.1.1          # test reachability to gateway (4 packets)
ping -c 4 8.8.8.8              # test internet connectivity by IP
```

> [!warning] ICMP may be filtered
> A remote host may block ICMP Echo Requests, causing `ping` to fail even though connectivity exists. Always use IP-based `ping` before hostname-based `ping` to separate routing issues from DNS issues.

---

## Step 5 — Test DNS Resolution

If connectivity by IP works but hostnames fail, the problem is DNS:

```bash
nslookup example.com           # query configured DNS server
dig example.com                # detailed DNS query with TTL and flags
```

These tools query DNS directly and are not affected by ICMP filtering.

---

## Step 6 — Trace the Network Path

If ping reaches the gateway but a remote host is unreachable, trace the route to identify where packets stop:

```bash
traceroute example.com         # trace path hop by hop (uses UDP by default)
tracepath example.com          # similar, no root required, auto-detects MTU
```

Each line shows a hop (router) along the path. A `* * *` response indicates a hop that drops ICMP or UDP probes — not necessarily a failure point.

---

## Quick Reference

| Check | Command |
|---|---|
| Interface state | `ip link` |
| IP address | `ip addr` |
| Routing table | `ip route` |
| Gateway reachability | `ping -c 4 <gateway-ip>` |
| Internet by IP | `ping -c 4 8.8.8.8` |
| DNS resolution | `nslookup example.com` / `dig example.com` |
| Path trace | `traceroute example.com` |
