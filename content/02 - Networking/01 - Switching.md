---
title: "01 - Switching"
description: "Network switches — how they work, MAC address tables, and the difference between Layer 2 and Layer 3 switches."
tags:
  - linux
  - networking
  - switching
---

A **network switch** establishes communication between multiple devices on the same network. Devices connect via **network interfaces**, which may be:

- **Physical** — Ethernet interfaces (e.g. `eth0`, `enp3s0`)
- **Virtual** — software-defined interfaces (e.g. `veth`, bridge interfaces)

---

## How a Switch Works

Unlike a hub (which broadcasts every frame to all connected ports), a switch forwards traffic **only to the destination device**.

A switch builds and maintains a **MAC address table** (also called a CAM table), mapping each MAC address to the port it was learned from:

| MAC Address | Port |
|---|---|
| `aa:bb:cc:dd:ee:01` | Port 1 |
| `aa:bb:cc:dd:ee:02` | Port 3 |

When a frame arrives:
1. The switch records the **source MAC address** and the port it arrived on.
2. It looks up the **destination MAC address** in the table.
3. If found, it forwards the frame to the correct port only.
4. If not found, it **floods** the frame to all ports except the source (until it learns the destination).

Benefits over hubs:
- Improved network performance
- Better bandwidth utilisation
- Reduced unnecessary traffic

---

## Layer 2 vs Layer 3 Switches

| | Layer 2 Switch | Layer 3 Switch |
|---|---|---|
| Operates on | MAC addresses (Data Link layer) | IP addresses (Network layer) |
| Routing | No — same network only | Yes — can route between subnets |
| Use case | Single VLAN / subnet | Inter-VLAN routing, campus networks |

---

## Network Interfaces in Linux

A **network interface** is the component that allows a machine to communicate with other devices on a network. Interfaces can be physical (Ethernet, Wi-Fi adapter) or virtual (VPN tunnel, bridge, container veth pair).

```bash
ip link                        # list all interfaces — name, state, MAC, MTU
ip link show eth0              # details for a specific interface
ip addr                        # list interfaces with assigned IP addresses
ip addr show eth0              # IP details for a specific interface
```

`ip link` output shows:
- Interface name (e.g. `eth0`, `enp3s0`, `lo`)
- State (`UP` / `DOWN`)
- MAC address
- MTU (Maximum Transmission Unit)
- Link type

### Enabling and disabling an interface

```bash
sudo ip link set eth0 up       # bring interface up
sudo ip link set eth0 down     # bring interface down
```

Once IP addresses are assigned and interfaces are up, machines on the same network can communicate through a switch.
