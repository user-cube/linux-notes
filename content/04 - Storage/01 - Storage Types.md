---
title: "01 - Storage Types"
description: "Linux storage architectures — DAS, NAS, and SAN — characteristics, protocols, and use cases."
tags:
  - linux
  - storage
  - das
  - nas
  - san
---

Linux systems can use different storage architectures depending on performance, scalability, and availability requirements. The three most common are:

| Type | Full Name | Access model |
|---|---|---|
| **DAS** | Direct Attached Storage | Local block device |
| **NAS** | Network Attached Storage | File share over the network |
| **SAN** | Storage Area Network | Remote block device |

---

## DAS — Direct Attached Storage

Storage physically connected to the host.

Examples: internal hard drives, SSDs, NVMe drives, USB storage.

**Characteristics:**
- High performance and low latency (no network overhead)
- Simple to configure
- Accessible only by the directly connected host — not shareable

Common in desktop computers, laptops, and standalone servers.

```bash
lsblk          # list locally attached block devices
```

---

## NAS — Network Attached Storage

Shared file storage accessed over a standard network.

**Characteristics:**
- Multiple systems can access the same storage simultaneously
- Centralised management — easier to expand
- Higher latency than DAS due to network overhead
- Presented as a **filesystem** (file-level access)

Common NAS protocols:

| Protocol | Use case |
|---|---|
| **NFS** | Linux/Unix file sharing |
| **SMB/CIFS** | Windows and cross-platform sharing |

```bash
# Mount an NFS share
sudo mount -t nfs 192.168.1.50:/shared /mnt/nas

# Mount an SMB share
sudo mount -t cifs //192.168.1.50/share /mnt/smb -o username=john
```

Common uses: user home directories, shared documents, backups, application data.

> [!warning] NAS and databases
> Hosting production databases on NAS is generally not recommended. The file-level access model and network latency can cause performance and reliability issues. Use DAS or SAN for database workloads.

---

## SAN — Storage Area Network

High-performance storage network that presents storage to the OS as **block devices** — indistinguishable from locally attached disks.

**Characteristics:**
- Very high performance and low latency
- Storage is partitioned and allocated per host (not shared at the file level)
- More complex and expensive to deploy than NAS
- Designed for mission-critical workloads

Common SAN protocols:

| Protocol | Transport | Notes |
|---|---|---|
| **Fibre Channel (FCP)** | Dedicated FC fabric | Traditional enterprise SAN |
| **iSCSI** | Ethernet (TCP/IP) | Lower cost, easier to deploy |
| **NVMe-oF** | Ethernet / FC / InfiniBand | Modern, ultra-low latency |

Fibre Channel uses dedicated switches and HBAs (Host Bus Adapters) separate from the standard network infrastructure.

Typical SAN use cases: enterprise databases, virtual machine storage (VMware, Hyper-V), high-performance computing.

---

## Comparison

| | DAS | NAS | SAN |
|---|---|---|---|
| Access model | Block (local) | File (network) | Block (network) |
| Shared access | No | Yes | Yes (per LUN) |
| Latency | Lowest | Medium | Low |
| Cost | Low | Low–Medium | High |
| Complexity | Simple | Moderate | Complex |
| Best for | Single-host performance | File sharing, backups | Mission-critical, databases, VMs |
