# Static Routing Network Documentation

## Network Overview
---
![alt text](<src/assets/Screenshot 2025-12-16 043612.png>)
---
This documentation covers a multi-router static routing topology with 4 networks connected via serial WAN links.

| Network | Network ID | CIDR | Gateway | Hosts | Router |
|---------|------------|------|---------|-------|--------|
| Network 1 | 192.168.1.0 | /24 | 192.168.1.1 | 1 PC | Router0 |
| Network 2 | 192.168.2.0 | /24 | 192.168.2.1 | 3 PCs | Switch + Router1 |
| Network 3 | 192.168.3.0 | /24 | 192.168.3.1 | 1 PC | Router1 |
| Network 4 | 192.168.4.0 | /24 | 192.168.4.0 | 1 PC | Router5 |
| Network 5 | 192.168.5.0 | /24 | 192.168.5.0 | 1 PC | Router5(1) |
| WAN 1 | 192.169.1.0 | /30 | - | Serial Link | Router0-Router1 |
| WAN 2 | 192.169.2.0 | /30 | - | Serial Link | Router1-Router5 |
| WAN 3 | 192.169.3.0 | /30 | - | Serial Link | Router5-Router5(1) |

---

## Network Topology Structure

```
[Network 1]         [WAN 1]        [Network 2]        [WAN 2]        [Network 3]        [WAN 3]        [Network 4/5]
192.168.1.0/24   192.169.1.0/30   192.168.2.0/24   192.169.2.0/30   192.168.3.0/24   192.169.3.0/30   192.168.4-5.0/24
      |               |                  |               |                  |               |                  |
   [PC-PT]      [ROUTER0]==========[ROUTER1]===========[ROUTER5]==========[ROUTER5(1)]   [PC-PT] [PC-PT]
  192.168.1.2    Se2/0: .1.2      Fa2/1: Switch     Se2/0: .2.2      Fa0/0: .3.1       192.168.4.2  192.168.5.2
                 Fa0/0: .1.1      Fa0/0: .2.1       Fa0/0: Rtr1      Fa0/0: .4.0/.5.0
                                  |
                             [SWITCH-PT]
                            /      |      \
                       [PC-PT] [PC-PT] [PC-PT]
                       .2.2    .2.3    .2.4
```

---

## Complete Device Inventory

### Routers

| Router | Hostname | Interfaces | Networks Connected | Role |
|--------|----------|------------|-------------------|------|
| Router0 | ROUTER0 | Fa0/0, Se2/0 | 192.168.1.0/24, 192.169.1.0/30 | Edge Router |
| Router1 | ROUTER1 | Fa0/0, Fa2/1, Se3/0 | 192.168.2.0/24, 192.169.1.0/30, 192.169.2.0/30 | Core Router |
| Router5 | ROUTER5 | Fa0/0, Se2/0, Se3/0 | 192.168.3.0/24, 192.169.2.0/30, 192.169.3.0/30 | Core Router |
| Router5(1) | ROUTER5 | Fa0/0 (×2), Se2/0 | 192.168.4.0/24, 192.168.5.0/24, 192.169.3.0/30 | Edge Router |

### End Devices

| Network | Device | IP Address | Subnet Mask | Default Gateway | Connected To |
|---------|--------|------------|-------------|-----------------|--------------|
| 192.168.1.0/24 | PC-PT | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 | Router0 Fa0/0 |
| 192.168.2.0/24 | PC-PT | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 | Switch Fa0 |
| 192.168.2.0/24 | PC-PT | 192.168.2.3 | 255.255.255.0 | 192.168.2.1 | Switch Fa0 |
| 192.168.2.0/24 | PC-PT | 192.168.2.4 | 255.255.255.0 | 192.168.2.1 | Switch Fa0 |
| 192.168.3.0/24 | PC-PT | 192.168.3.2 | 255.255.255.0 | 192.168.3.1 | Router1 Fa0/0 |
| 192.168.4.0/24 | PC-PT | 192.168.4.2 | 255.255.255.0 | 192.168.4.0 | Router5(1) Fa0/0 |
| 192.168.5.0/24 | PC-PT | 192.168.5.2 | 255.255.255.0 | 192.168.5.0 | Router5(1) Fa0/0 |

---

## Router Interface Configuration

### Router0 Configuration

**Interface Details:**
| Interface | IP Address | Subnet Mask | Network | Type |
|-----------|------------|-------------|---------|------|
| Fa0/0 | 192.168.1.1 | 255.255.255.0 | 192.168.1.0/24 | LAN |
| Se2/0 | 192.169.1.2 | 255.255.255.252 | 192.169.1.0/30 | WAN (DCE) |

**Configuration Commands:**
```cisco
enable
configure terminal
hostname ROUTER0

! LAN Interface
interface FastEthernet0/0
 description LAN Network 192.168.1.0/24
 ip address 192.168.1.1 255.255.255.0
 no shutdown

! WAN Interface to Router1
interface Serial2/0
 description WAN to Router1
 ip address 192.169.1.2 255.255.255.252
 clock rate 64000
 no shutdown

! Static Routes
! Route to Network 2 (192.168.2.0/24)
ip route 192.168.2.0 255.255.255.0 192.169.1.1

! Route to Network 3 (192.168.3.0/24) via Router1
ip route 192.168.3.0 255.255.255.0 192.169.1.1

! Route to Network 4 (192.168.4.0/24) via Router1
ip route 192.168.4.0 255.255.255.0 192.169.1.1

! Route to Network 5 (192.168.5.0/24) via Router1
ip route 192.168.5.0 255.255.255.0 192.169.1.1

end
write memory
```

---

### Router1 Configuration

**Interface Details:**
| Interface | IP Address | Subnet Mask | Network | Type |
|-----------|------------|-------------|---------|------|
| Fa0/0 | 192.168.3.1 | 255.255.255.0 | 192.168.3.0/24 | LAN |
| Fa2/1 | 192.168.2.1 | 255.255.255.0 | 192.168.2.0/24 | LAN (via Switch) |
| Se3/0 | 192.169.1.1 | 255.255.255.252 | 192.169.1.0/30 | WAN (DTE) |
| Se2/0 | 192.169.2.1 | 255.255.255.252 | 192.169.2.0/30 | WAN (DCE) |

**Configuration Commands:**
```cisco
enable
configure terminal
hostname ROUTER1

! LAN Interface to PC
interface FastEthernet0/0
 description LAN Network 192.168.3.0/24
 ip address 192.168.3.1 255.255.255.0
 no shutdown

! LAN Interface to Switch
interface FastEthernet2/1
 description LAN to Switch Network 192.168.2.0/24
 ip address 192.168.2.1 255.255.255.0
 no shutdown

! WAN Interface to Router0
interface Serial3/0
 description WAN to Router0
 ip address 192.169.1.1 255.255.255.252
 no shutdown

! WAN Interface to Router5
interface Serial2/0
 description WAN to Router5
 ip address 192.169.2.1 255.255.255.252
 clock rate 64000
 no shutdown

! Static Routes
! Route to Network 1 (192.168.1.0/24) via Router0
ip route 192.168.1.0 255.255.255.0 192.169.1.2

! Route to Network 4 (192.168.4.0/24) via Router5
ip route 192.168.4.0 255.255.255.0 192.169.2.2

! Route to Network 5 (192.168.5.0/24) via Router5
ip route 192.168.5.0 255.255.255.0 192.169.2.2

end
write memory
```

---

### Router5 Configuration

**Interface Details:**
| Interface | IP Address | Subnet Mask | Network | Type |
|-----------|------------|-------------|---------|------|
| Se2/0 | 192.169.2.2 | 255.255.255.252 | 192.169.2.0/30 | WAN (DTE) |
| Se3/0 | 192.169.3.1 | 255.255.255.252 | 192.169.3.0/30 | WAN (DCE) |
| Fa0/0 | 192.168.3.1 | 255.255.255.0 | 192.168.3.0/24 | LAN |

**Configuration Commands:**
```cisco
enable
configure terminal
hostname ROUTER5

! LAN Interface (if connected)
interface FastEthernet0/0
 description LAN Network 192.168.3.0/24
 ip address 192.168.3.1 255.255.255.0
 no shutdown

! WAN Interface to Router1
interface Serial2/0
 description WAN to Router1
 ip address 192.169.2.2 255.255.255.252
 no shutdown

! WAN Interface to Router5(1)
interface Serial3/0
 description WAN to Router5(1)
 ip address 192.169.3.1 255.255.255.252
 clock rate 64000
 no shutdown

! Static Routes
! Route to Network 1 (192.168.1.0/24) via Router1
ip route 192.168.1.0 255.255.255.0 192.169.2.1

! Route to Network 2 (192.168.2.0/24) via Router1
ip route 192.168.2.0 255.255.255.0 192.169.2.1

! Route to Network 4 (192.168.4.0/24) via Router5(1)
ip route 192.168.4.0 255.255.255.0 192.169.3.2

! Route to Network 5 (192.168.5.0/24) via Router5(1)
ip route 192.168.5.0 255.255.255.0 192.169.3.2

end
write memory
```

---

### Router5(1) Configuration

**Interface Details:**
| Interface | IP Address | Subnet Mask | Network | Type |
|-----------|------------|-------------|---------|------|
| Se2/0 | 192.169.3.2 | 255.255.255.252 | 192.169.3.0/30 | WAN (DTE) |
| Fa0/0 | 192.168.4.0 | 255.255.255.0 | 192.168.4.0/24 | LAN |
| Fa0/0 | 192.168.5.0 | 255.255.255.0 | 192.168.5.0/24 | LAN |

**Configuration Commands:**
```cisco
enable
configure terminal
hostname ROUTER5-1

! WAN Interface to Router5
interface Serial2/0
 description WAN to Router5
 ip address 192.169.3.2 255.255.255.252
 no shutdown

! LAN Interface for Network 4
interface FastEthernet0/0
 description LAN Network 192.168.4.0/24
 ip address 192.168.4.0 255.255.255.0
 no shutdown

! LAN Interface for Network 5
interface FastEthernet0/1
 description LAN Network 192.168.5.0/24
 ip address 192.168.5.0 255.255.255.0
 no shutdown

! Static Routes
! Route to Network 1 (192.168.1.0/24) via Router5
ip route 192.168.1.0 255.255.255.0 192.169.3.1

! Route to Network 2 (192.168.2.0/24) via Router5
ip route 192.168.2.0 255.255.255.0 192.169.3.1

! Route to Network 3 (192.168.3.0/24) via Router5
ip route 192.168.3.0 255.255.255.0 192.169.3.1

end
write memory
```

---

## Static Routing Table

### Router0 Static Routes

| Destination Network | Subnet Mask | Next Hop | Interface | Administrative Distance |
|-------------------|-------------|----------|-----------|------------------------|
| 192.168.2.0 | 255.255.255.0 | 192.169.1.1 | Se2/0 | 1 |
| 192.168.3.0 | 255.255.255.0 | 192.169.1.1 | Se2/0 | 1 |
| 192.168.4.0 | 255.255.255.0 | 192.169.1.1 | Se2/0 | 1 |
| 192.168.5.0 | 255.255.255.0 | 192.169.1.1 | Se2/0 | 1 |

### Router1 Static Routes

| Destination Network | Subnet Mask | Next Hop | Interface | Administrative Distance |
|-------------------|-------------|----------|-----------|------------------------|
| 192.168.1.0 | 255.255.255.0 | 192.169.1.2 | Se3/0 | 1 |
| 192.168.4.0 | 255.255.255.0 | 192.169.2.2 | Se2/0 | 1 |
| 192.168.5.0 | 255.255.255.0 | 192.169.2.2 | Se2/0 | 1 |

### Router5 Static Routes

| Destination Network | Subnet Mask | Next Hop | Interface | Administrative Distance |
|-------------------|-------------|----------|-----------|------------------------|
| 192.168.1.0 | 255.255.255.0 | 192.169.2.1 | Se2/0 | 1 |
| 192.168.2.0 | 255.255.255.0 | 192.169.2.1 | Se2/0 | 1 |
| 192.168.4.0 | 255.255.255.0 | 192.169.3.2 | Se3/0 | 1 |
| 192.168.5.0 | 255.255.255.0 | 192.169.3.2 | Se3/0 | 1 |

### Router5(1) Static Routes

| Destination Network | Subnet Mask | Next Hop | Interface | Administrative Distance |
|-------------------|-------------|----------|-----------|------------------------|
| 192.168.1.0 | 255.255.255.0 | 192.169.3.1 | Se2/0 | 1 |
| 192.168.2.0 | 255.255.255.0 | 192.169.3.1 | Se2/0 | 1 |
| 192.168.3.0 | 255.255.255.0 | 192.169.3.1 | Se2/0 | 1 |

---

## Complete IP Addressing Table

### LAN Networks

#### Network 1: 192.168.1.0/24

| Device | IP Address | Subnet Mask | Default Gateway | Interface |
|--------|------------|-------------|-----------------|-----------|
| Router0 | 192.168.1.1 | 255.255.255.0 | - | Fa0/0 |
| PC-PT | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 | NIC |

#### Network 2: 192.168.2.0/24

| Device | IP Address | Subnet Mask | Default Gateway | Interface |
|--------|------------|-------------|-----------------|-----------|
| Router1 | 192.168.2.1 | 255.255.255.0 | - | Fa2/1 |
| PC-PT 1 | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 | NIC |
| PC-PT 2 | 192.168.2.3 | 255.255.255.0 | 192.168.2.1 | NIC |
| PC-PT 3 | 192.168.2.4 | 255.255.255.0 | 192.168.2.1 | NIC |

#### Network 3: 192.168.3.0/24

| Device | IP Address | Subnet Mask | Default Gateway | Interface |
|--------|------------|-------------|-----------------|-----------|
| Router1 | 192.168.3.1 | 255.255.255.0 | - | Fa0/0 |
| PC-PT | 192.168.3.2 | 255.255.255.0 | 192.168.3.1 | NIC |

#### Network 4: 192.168.4.0/24

| Device | IP Address | Subnet Mask | Default Gateway | Interface |
|--------|------------|-------------|-----------------|-----------|
| Router5(1) | 192.168.4.0 | 255.255.255.0 | - | Fa0/0 |
| PC-PT | 192.168.4.2 | 255.255.255.0 | 192.168.4.0 | NIC |

#### Network 5: 192.168.5.0/24

| Device | IP Address | Subnet Mask | Default Gateway | Interface |
|--------|------------|-------------|-----------------|-----------|
| Router5(1) | 192.168.5.0 | 255.255.255.0 | - | Fa0/1 |
| PC-PT | 192.168.5.2 | 255.255.255.0 | 192.168.5.0 | NIC |

### WAN Links

#### WAN 1: 192.169.1.0/30

| Device | IP Address | Subnet Mask | Interface | Type |
|--------|------------|-------------|-----------|------|
| Router1 | 192.169.1.1 | 255.255.255.252 | Se3/0 | DTE |
| Router0 | 192.169.1.2 | 255.255.255.252 | Se2/0 | DCE |

**Network Info:**
- Network: 192.169.1.0
- Usable IPs: 192.169.1.1 - 192.169.1.2
- Broadcast: 192.169.1.3

#### WAN 2: 192.169.2.0/30

| Device | IP Address | Subnet Mask | Interface | Type |
|--------|------------|-------------|-----------|------|
| Router1 | 192.169.2.1 | 255.255.255.252 | Se2/0 | DCE |
| Router5 | 192.169.2.2 | 255.255.255.252 | Se2/0 | DTE |

**Network Info:**
- Network: 192.169.2.0
- Usable IPs: 192.169.2.1 - 192.169.2.2
- Broadcast: 192.169.2.3

#### WAN 3: 192.169.3.0/30

| Device | IP Address | Subnet Mask | Interface | Type |
|--------|------------|-------------|-----------|------|
| Router5 | 192.169.3.1 | 255.255.255.252 | Se3/0 | DCE |
| Router5(1) | 192.169.3.2 | 255.255.255.252 | Se2/0 | DTE |

**Network Info:**
- Network: 192.169.3.0
- Usable IPs: 192.169.3.1 - 192.169.3.2
- Broadcast: 192.169.3.3

---

## Testing and Verification

### Verification Commands

**On all routers:**
```cisco
show ip interface brief
show ip route
show ip route static
show interfaces
show running-config
```

### Connectivity Test Matrix

#### From Network 1 (192.168.1.2)

```
ping 192.168.1.1     ✅ Router0 (Local Gateway)
ping 192.169.1.1     ✅ Router1 Se3/0 (1 hop)
ping 192.168.2.1     ✅ Router1 Fa2/1 (1 hop)
ping 192.168.2.2     ✅ PC in Network 2 (2 hops)
ping 192.168.3.2     ✅ PC in Network 3 (2 hops)
ping 192.168.4.2     ✅ PC in Network 4 (3 hops)
ping 192.168.5.2     ✅ PC in Network 5 (3 hops)
tracert 192.168.5.2  (Path: .1.1 → .1.1 → .2.2 → .3.1 → .5.2)
```

#### From Network 2 (192.168.2.2)

```
ping 192.168.2.1     ✅ Router1 (Local Gateway)
ping 192.168.1.2     ✅ PC in Network 1 (2 hops)
ping 192.168.3.2     ✅ PC in Network 3 (1 hop - same router)
ping 192.168.4.2     ✅ PC in Network 4 (3 hops)
ping 192.168.5.2     ✅ PC in Network 5 (3 hops)
```

#### From Network 4 (192.168.4.2)

```
ping 192.168.4.0     ✅ Router5(1) (Local Gateway)
ping 192.168.5.2     ✅ PC in Network 5 (1 hop - same router)
ping 192.169.3.1     ✅ Router5 Se3/0 (1 hop)
ping 192.168.1.2     ✅ PC in Network 1 (4 hops)
ping 192.168.2.2     ✅ PC in Network 2 (3 hops)
tracert 192.168.1.2  (Path: .4.0 → .3.1 → .2.1 → .1.1 → .1.2)
```

---

## Routing Path Analysis

### Path from Network 1 to Network 5

**Route:** 192.168.1.2 → 192.168.5.2

| Hop | Device | Interface Out | Next Hop | Network |
|-----|--------|--------------|----------|---------|
| 0 | PC (192.168.1.2) | - | 192.168.1.1 | Source |
| 1 | Router0 | Se2/0 (192.169.1.2) | 192.169.1.1 | WAN 1 |
| 2 | Router1 | Se2/0 (192.169.2.1) | 192.169.2.2 | WAN 2 |
| 3 | Router5 | Se3/0 (192.169.3.1) | 192.169.3.2 | WAN 3 |
| 4 | Router5(1) | Fa0/1 | 192.168.5.2 | Destination |

**Total Hops:** 4  
**Path Description:** Network1 → Router0 → Router1 → Router5 → Router5(1) → Network5

---

## Troubleshooting Guide

### Common Issues

| Issue | Possible Cause | Solution | Command to Check |
|-------|---------------|----------|------------------|
| No connectivity between networks | Missing static route | Add static route | `show ip route` |
| Serial link down | Clock rate not set on DCE | Configure clock rate | `show controllers serial` |
| Can ping next hop but not beyond | Missing route on intermediate router | Configure route on all routers in path | `show ip route` |
| Interface shows down/down | No shutdown command missing | `no shutdown` on interface | `show ip interface brief` |
| Wrong subnet mask | Configuration error | Reconfigure with correct mask | `show running-config` |

### Diagnostic Commands

```cisco
! Check routing table
show ip route
show ip route static

! Check interface status
show ip interface brief
show interfaces serial 2/0
show controllers serial 2/0

! Verify connectivity
ping [destination]
traceroute [destination]

! Check configuration
show running-config
show startup-config

! Debug (use carefully)
debug ip routing
debug ip packet
```

### Static Route Verification

**Verify static routes are in routing table:**
```cisco
Router# show ip route static
S    192.168.2.0/24 [1/0] via 192.169.1.1
S    192.168.3.0/24 [1/0] via 192.169.1.1
S    192.168.4.0/24 [1/0] via 192.169.1.1
S    192.168.5.0/24 [1/0] via 192.169.1.1
```

---

## Network Summary

### Statistics

| Metric | Count |
|--------|-------|
| Total Routers | 4 |
| Total PCs | 7 |
| LAN Networks | 5 |
| WAN Links | 3 |
| Static Routes | 10 |
| Total Subnets | 8 |

### Router Roles

| Router | Role | Static Routes | Connected Networks |
|--------|------|---------------|-------------------|
| Router0 | Edge | 4 routes | 2 (1 LAN + 1 WAN) |
| Router1 | Core | 3 routes | 4 (2 LAN + 2 WAN) |
| Router5 | Core | 4 routes | 3 (1 LAN + 2 WAN) |
| Router5(1) | Edge | 3 routes | 3 (2 LAN + 1 WAN) |

---

## Best Practices Applied

✅ **Efficient IP Addressing** - /30 subnets for WAN links (only 2 usable IPs)  
✅ **Proper Static Routing** - All networks reachable from all locations  
✅ **Clear Documentation** - All routes documented  
✅ **Consistent Configuration** - Standardized commands across routers  
✅ **Scalability** - Room for additional networks

---

## Configuration Backup

### Router0 Routes Summary
```
RN: 192.168.1.0
STATIC: 192.168.2.0 255.255.255.0 via 192.169.1.1
STATIC: 192.168.3.0 255.255.255.0 via 192.169.1.1
STATIC: 192.168.4.0 255.255.255.0 via 192.169.1.1
STATIC: 192.168.5.0 255.255.255.0 via 192.169.1.1
```

### Router1 Routes Summary
```
RN: 192.168.2.0, 192.168.3.0
STATIC: 192.168.1.0 255.255.255.0 via 192.169.1.2
STATIC: 192.168.4.0 255.255.255.0 via 192.169.2.2
STATIC: 192.168.5.0 255.255.255.0 via 192.169.2.2
```

### Router5 Routes Summary
```
RN: 192.168.3.0 (shared)
STATIC: 192.168.1.0 255.255.255.0 via 192.169.2.1
STATIC: 192.168.2.0 255.255.255.0 via 192.169.2.1
STATIC: 192.168.4.0 255.255.255.0 via 192.169.3.2
STATIC: 192.168.5.0 255.255.255.0 via 192.169.3.2
```

### Router5(1) Routes Summary
```
RN: 192.168.4.0, 192.168.5.0
STATIC: 192.168.1.0 255.255.255.0 via 192.169.3.1
STATIC: 192.168.2.0 255.255.255.0 via 192.169.3.1
STATIC: 192.168.3.0 255.255.255.0 via 192.169.3.1
```

---

## Network Checklist

- [x] All router interfaces configured
- [x] All static routes configured
- [x] Clock rates set on DCE interfaces
- [x] All PCs have correct IP and gateway
- [x] End-to-end connectivity verified
- [x] Routing tables verified
- [ ] Configuration saved on all routers
- [ ] Documentation updated
- [ ] Network tested under load

---

**Document Version:** 1.0  
**Network Type:** Static Routing  
**Total Networks:** 5 LAN + 3 WAN  
**Routing Protocol:** Static Routes  
**Status:** ✅ Operational  
**Last Updated:** December 2024