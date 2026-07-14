# Beowulf LAN Setup

## Scope

This document describes the internal local area network used by the Beowulf cluster. It is intended for shared operational documentation, so host-specific SSH aliases or workstation-specific configuration are avoided where possible.

## Network summary

- LAN subnet: `192.168.88.0/24`
- Router/switch/DNS appliance: MikroTik at `192.168.88.1`
- Head node: primary entry point into the cluster LAN
- Compute node address range currently documented: `192.168.88.11` through `192.168.88.25`
- Cluster hostnames are intended to resolve through MikroTik static DNS records or explicitly managed host files.

## Topology

```text
Cal Poly's eduroam via wall ethernet
        |
        | 
        v
Cluster head node
        |
        | Internal LAN: 192.168.88.0/24
        v
MikroTik router/switch/DNS: 192.168.88.1
        |
        +-- node00: 192.168.88.11
        +-- node01: 192.168.88.12
        +-- node02: 192.168.88.13
        +-- node03: 192.168.88.14
        +-- node04: 192.168.88.15
        +-- node05: 192.168.88.16
        +-- node06: 192.168.88.17
        +-- node07: 192.168.88.18
        +-- node08: 192.168.88.19
        +-- node09: 192.168.88.20
        +-- node10: 192.168.88.21
        +-- node11: 192.168.88.22
        +-- node12: 192.168.88.23
        +-- node13: 192.168.88.24
        +-- node14: 192.168.88.25
```

## Addressing

IP-to-name mappings are configured as static DNS entries in MikroTik RouterOS under `/ip dns static`. 

IMPORTANT: The head node is the entry point of an incoming connection, not MikroTik (and as such it only acts as a switch, and not a router.) This means that the head node is currently responsible for the firewall and DNS configuration for the whole LAN.
In the head node's network settings, the DNS address must be set to the address of MikroTik. There is also a DNS served by CalPoly, which may interfere with ours if the settings are misconfigured.


*NOTE: In the future these aliases will be renamed to a proper naming convention, such as nodeXX.cluster.local. We were unaware that this naming convention is actually enforced by OS. In order to temporarily bypass it, we set "ResolveUnicastSingleLabel=yes" in /etc/systemd/resolved.conf*

| Host | IP address | Notes |
| --- | --- | --- |
| MikroTik | `192.168.88.1` | Router/switch/DNS appliance |
| `node00` | `192.168.88.11` | Compute node |
| `node01` | `192.168.88.12` | Compute node |
| `node02` | `192.168.88.13` | Compute node |
| `node03` | `192.168.88.14` | Compute node |
| `node04` | `192.168.88.15` | Compute node |
| `node05` | `192.168.88.16` | Compute node |
| `node06` | `192.168.88.17` | Compute node |
| `node07` | `192.168.88.18` | Compute node |
| `node08` | `192.168.88.19` | Compute node |
| `node09` | `192.168.88.20` | Compute node |
| `node10` | `192.168.88.21` | Compute node |
| `node11` | `192.168.88.22` | Compute node |
| `node12` | `192.168.88.23` | Compute node |
| `node13` | `192.168.88.24` | Compute node |
| `node14` | `192.168.88.25` | Compute node |



## Access pattern

Users normally access the cluster by SSHing to the head node first. The exact hostname or SSH alias for the head node may differ by workstation.

Example:

```sh
ssh <head-node-hostname-or-alias>
```

From the head node, compute nodes can be reached by hostname or IP:

```sh
ssh node00
ssh 192.168.88.11
```

When ssh'd inside the head node, mikrotik's terminal can be accessed also

```sh
ssh admin@192.168.88.1
```
or 
```sh
ssh mikrotik
```



## MikroTik inspection commands

Run these commands from the RouterOS shell:

```routeros
/ip arp print
/ip dns static print
/interface bridge host print
/interface ethernet print detail
```

## Adding a new node

- When a new ethernet cable is successfully connected, MikroTik should assign it a local IP address, probably incremented from a previous node. Check it by running `/ip arp print` in the switch.
- In the new PC's network settings, IPv4 must be configured to be manual. Use the following settings:
 ```text
   IPv4 method: Manual
   Address:     192.168.88.X    # assigned by MikroTik
   Netmask:     255.255.255.0   # or /24
   Gateway:     192.168.88.10   # head node
   DNS:         192.168.88.1    # MikroTik
 ```

- Keep the IP/name table updated when nodes are added, removed, or renumbered. DNS configuration should be in a single authoritative place. Currently that is the MikroTik DNS table. 


