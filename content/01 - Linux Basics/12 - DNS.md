---
title: "12 - DNS"
description: "DNS in Linux — /etc/hosts, /etc/resolv.conf, name resolution order, DNS record types, nslookup, and dig."
tags:
  - linux
  - networking
  - dns
  - basics
---

Linux resolves hostnames using either local mappings or DNS servers. The resolution order is configurable.

---

## `/etc/hosts`

Local static hostname mappings — resolved before DNS is queried.

```
192.168.1.10   web-server
192.168.1.20   database
```

No validation is performed on entries. Once added, the OS resolves that hostname to the specified IP immediately.

Common uses: testing, development environments, small networks, temporary overrides.

---

## DNS Servers — `/etc/resolv.conf`

Specifies the DNS servers the system queries when a hostname is not found locally.

```
nameserver 8.8.8.8
nameserver 1.1.1.1
```

> [!note] systemd-resolved
> On modern systemd-based distributions (Ubuntu 18.04+, Fedora, RHEL 8+), `/etc/resolv.conf` is often a symlink managed by `systemd-resolved`:
> ```bash
> ls -l /etc/resolv.conf   # may point to /run/systemd/resolve/stub-resolv.conf
> resolvectl status        # view current DNS configuration and status
> ```

### Search Domains

```
nameserver 8.8.8.8
search mycompany.com
```

With `search mycompany.com` configured, typing `web` resolves as `web.mycompany.com` automatically. Multiple search domains can be listed.

---

## Name Resolution Order — `/etc/nsswitch.conf`

Controls which sources are checked and in what order:

```
hosts: files dns
```

| Source | Meaning |
|---|---|
| `files` | Check `/etc/hosts` |
| `dns` | Query configured DNS servers |

Default order: local hosts file first, then DNS. If neither resolves the hostname, the lookup fails.

---

## DNS Forwarding

A private DNS server can forward unresolved queries to public resolvers, allowing an organisation to handle both internal and internet hostnames from a single DNS infrastructure.

Common forwarders:
- Google Public DNS: `8.8.8.8`
- Cloudflare DNS: `1.1.1.1`

---

## Domain Name Structure

```
www.example.com
│   │       └── Top-Level Domain (TLD)
│   └────────── Domain
└────────────── Hostname
```

---

## DNS Record Types

| Record | Purpose |
|---|---|
| **A** | Maps a hostname to an IPv4 address |
| **AAAA** | Maps a hostname to an IPv6 address |
| **CNAME** | Alias from one hostname to another |
| **MX** | Mail server responsible for a domain |
| **NS** | Authoritative name server for a domain |
| **PTR** | Reverse DNS — maps an IP address back to a hostname |

---

## `nslookup`

Queries DNS servers directly — not affected by `/etc/hosts`.

```bash
nslookup example.com              # basic query (A record)
nslookup -type=MX example.com     # query a specific record type
nslookup example.com 8.8.8.8      # query a specific DNS server
```

---

## `dig`

More advanced DNS query tool. Provides full DNS response details including TTL, flags, and authority information.

```bash
dig example.com                   # basic query (A record)
dig example.com MX                # query MX records
dig example.com NS                # query name servers
dig @8.8.8.8 example.com          # query a specific DNS server
dig -x 1.1.1.1                    # reverse DNS lookup (PTR record)
```

`dig` is the preferred tool for DNS troubleshooting on Linux — its output is more complete than `nslookup`.

---

## `host`

A simpler alternative for quick lookups:

```bash
host example.com                  # A record
host -t MX example.com            # MX record
host 8.8.8.8                      # reverse DNS lookup
```
