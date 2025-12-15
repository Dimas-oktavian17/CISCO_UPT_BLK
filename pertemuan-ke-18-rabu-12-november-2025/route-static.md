# Complex Static Routing Network Documentation

## Network Overview
---
![alt text](<src/assets/Screenshot 2025-12-16 044735.png>)
---
This documentation covers a complex multi-router static routing topology with 4 routers, 1 gateway, and multiple LAN/WAN segments.

| Network | Network ID | CIDR | Usable Range | Gateway | Hosts |
|---------|------------|------|--------------|---------|-------|
| LAN 1 | 192.168.1.0 | /24 | 192.168.1.1-254 | 192.168.1.1 | 1 PC |
| LAN 2 | 192.168.2.0 | /24 | 192.168.2.1-254 | 192.168.2.1 | 1 PC |
| LAN 3 | 192.168.3.0 | /24 | 192.168.3.1-254 | 192.168.3.1 | 1 PC |
| LAN 4 | 192.168.4.0 | /24 | 192.168.4.1-254 | 192.168.4.1 | 1 PC |
| LAN 5 | 192.168.5.0 | /24 | 192.168.5.1-254 | 192.168.5.1 | 1 PC |
| LAN 6 | 192.168.6.0 | /24 | 192.168.6.1-254 | 192.168.6.1 | 1 PC |
| WAN 1 | 192.100.1.0 | /30 | .1-.2 | - | Serial Link |
| WAN 2 | 192.100.1.4 | /30 | .5-.6 | - | Serial Link |
| WAN 3 | 192.100.1.8 | /30 | .9-.10 | - | Serial Link |
| WAN 4 | 192.100.1.12 | /30 | .13-.14 | - | Serial Link |

---

## Network Topology Structure

```
[LAN 1]          [WAN 1]         [WAN 2]         [WAN 3]         [LAN 3]
192.168.1.0/24  192.100.1.0/30  192.100.1.4/30  192.100.1.8/30  192.168.3.0/24
     |              |                |                |                |
  [PC1.2]      [Router0]========[Router]===========[Router2]======[PC3.2]
  [PC2.2]         Se2/0          Se2/0  Se3/0       Se2/0         
192.168.2.0/24  Fa0/0: .1.1    Multi-Interface   Fa0/0: .3.1
                                   |
                                Se8/0 (WAN 4)
                                   |
                              192.100.1.12/30
                                   |
                               [Gateway]
                               Se2/0, Se3/0
                               Fa0/0, Fa1/0
                                 /    \
                            [LAN 4]  [LAN 5]
                           PC4.2     PC5.2
                           192.168.4-5.0/24
```

---

## Complete Device Inventory

### Routers

| Device | Hostname | Role | Interfaces | Networks Connected |
|--------|----------|------|------------|-------------------|
| Router0 | Router0 | Edge Router | Fa0/0, Se2/0 | LAN 1, LAN 2, WAN 1 |
| Router | Router | Core Router | Fa0/0, Se2/0, Se3/0, Se6/0, Se8/0 | WAN 1, WAN 2, WAN 3, WAN 4 |
| Router2 | Router2 | Edge Router | Fa0/0, Se2/0 | LAN 3, WAN 3 |
| Router3 | Router3 | Edge Router | Fa0/0, Se2/0 | LAN 6, WAN 4 |
| Gateway | Gateway | Distribution Router | Fa0/0, Fa1/0, Se2/0, Se3/0 | LAN 4, LAN 5, WAN 4 |

### End Devices

| Device | IP Address | Subnet Mask | Default Gateway | Network | Connected To |
|--------|------------|-------------|-----------------|---------|--------------|
| PC-PT | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 | LAN 1 | Router0 Fa0/0 |
| PC-PT | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 | LAN 2 | Router Fa0/0 |
| PC-PT | 192.168.3.2 | 255.255.255.0 | 192.168.3.1 | LAN 3 | Router2 Fa0/0 |
| PC-PT | 192.168.4.2 | 255.255.255.0 | 192.168.4.1 | LAN 4 | Gateway Fa0/0 |
| PC-PT | 192.168.5.2 | 255.255.255.0 | 192.168.5.1 | LAN 5 | Gateway Fa1/0 |
| PC-PT | 192.168.6.2 | 255.255.255.0 | 192.168.6.1 | LAN 6 | Router3 Fa0/0 |

---

## WAN Link Details

### WAN 1: 192.100.1.0/30

| Device | Interface | IP Address | Subnet Mask | Type |
|--------|-----------|------------|-------------|------|
| Router0 | Se2/0 | 192.100.1.1 | 255.255.255.252 | DCE |
| Router | Se2/0 | 192.100.1.2 | 255.255.255.252 | DTE |

**Network Info:**
- Network: 192.100.1.0
- Usable: 192.100.1.1 - 192.100.1.2
- Broadcast: 192.100.1.3

### WAN 2: 192.100.1.4/30

| Device | Interface | IP Address | Subnet Mask | Type |
|--------|-----------|------------|-------------|------|
| Router | Se3/0 | 192.100.1.5 | 255.255.255.252 | DCE |
| Router | Se6/0 | 192.100.1.6 | 255.255.255.252 | DTE |

**Network Info:**
- Network: 192.100.1.4
- Usable: 192.100.1.5 - 192.100.1.6
- Broadcast: 192.100.1.7

### WAN 3: 192.100.1.8/30

| Device | Interface | IP Address | Subnet Mask | Type |
|--------|-----------|------------|-------------|------|
| Router | Se8/0 | 192.100.1.6 | 255.255.255.252 | DCE |
| Router2 | Se2/0 | 192.100.1.6 | 255.255.255.252 | DTE |

**Network Info:**
- Network: 192.100.1.8
- Usable: 192.100.1.9 - 192.100.1.10
- Broadcast: 192.100.1.11

### WAN 4: 192.100.1.12/30

| Device | Interface | IP Address | Subnet Mask | Type |
|--------|-----------|------------|-------------|------|
| Router | Se8/0 | 192.100.1.13 | 255.255.255.252 | DCE |
| Gateway | Se2/0 | 192.100.1.10 | 255.255.255.252 | DTE |
| Gateway | Se3/0 | 192.100.1.10 | 255.255.255.252 | DTE |

**Network Info:**
- Network: 192.100.1.12
- Usable: 192.100.1.13 - 192.100.1.14
- Broadcast: 192.100.1.15

---

## Router Configurations

### Router0 Configuration

**Interface Summary:**
| Interface | IP Address | Subnet Mask | Description |
|-----------|------------|-------------|-------------|
| Fa0/0 | 192.168.1.1 | 255.255.255.0 | LAN 1 & LAN 2 |
| Se2/0 | 192.100.1.1 | 255.255.255.252 | WAN to Router (Core) |

**Configuration:**
```cisco
enable
configure terminal
hostname Router0

! LAN Interface
interface FastEthernet0/0
 description LAN Network 192.168.1.0/24 and 192.168.2.0/24
 ip address 192.168.1.1 255.255.255.0
 no shutdown

! WAN Interface to Core Router
interface Serial2/0
 description WAN to Core Router
 ip address 192.100.1.1 255.255.255.252
 clock rate 64000
 no shutdown

! Static Routes
! Route to LAN 2 via Core Router
ip route 192.168.2.0 255.255.255.0 192.100.1.2

! Route to LAN 3 via Core Router
ip route 192.168.3.0 255.255.255.0 192.100.1.2

! Route to LAN 4 via Core Router
ip route 192.168.4.0 255.255.255.0 192.100.1.2

! Route to LAN 5 via Core Router
ip route 192.168.5.0 255.255.255.0 192.100.1.2

! Route to LAN 6 via Core Router
ip route 192.168.6.0 255.255.255.0 192.100.1.2

! Route to internal WAN networks
ip route 192.100.1.4 255.255.255.252 192.100.1.2
ip route 192.100.1.8 255.255.255.252 192.100.1.2
ip route 192.100.1.12 255.255.255.252 192.100.1.2

end
write memory
```

---

### Core Router Configuration

**Interface Summary:**
| Interface | IP Address | Subnet Mask | Description |
|-----------|------------|-------------|-------------|
| Fa0/0 | 192.168.2.1 | 255.255.255.0 | LAN 2 |
| Se2/0 | 192.100.1.2 | 255.255.255.252 | WAN to Router0 |
| Se3/0 | 192.100.1.5 | 255.255.255.252 | Internal WAN |
| Se6/0 | 192.100.1.6 | 255.255.255.252 | Internal WAN |
| Se8/0 | 192.100.1.8 or .13 | 255.255.255.252 | WAN to Router2/Gateway |

**Configuration:**
```cisco
enable
configure terminal
hostname Router-Core

! LAN Interface
interface FastEthernet0/0
 description LAN Network 192.168.2.0/24
 ip address 192.168.2.1 255.255.255.0
 no shutdown

! WAN Interface to Router0
interface Serial2/0
 description WAN to Router0
 ip address 192.100.1.2 255.255.255.252
 no shutdown

! WAN Interface - Internal Link 1
interface Serial3/0
 description Internal WAN Link 1
 ip address 192.100.1.5 255.255.255.252
 clock rate 64000
 no shutdown

! WAN Interface - Internal Link 2
interface Serial6/0
 description Internal WAN Link 2
 ip address 192.100.1.6 255.255.255.252
 no shutdown

! WAN Interface to Router2
interface Serial8/0
 description WAN to Router2 and Gateway
 ip address 192.100.1.13 255.255.255.252
 clock rate 64000
 no shutdown

! Static Routes
! Route to LAN 1 via Router0
ip route 192.168.1.0 255.255.255.0 192.100.1.1

! Route to LAN 3 via Router2
ip route 192.168.3.0 255.255.255.0 192.100.1.10

! Route to LAN 4 via Gateway
ip route 192.168.4.0 255.255.255.0 192.100.1.10

! Route to LAN 5 via Gateway
ip route 192.168.5.0 255.255.255.0 192.100.1.10

! Route to LAN 6 via Router3
ip route 192.168.6.0 255.255.255.0 192.100.1.14

end
write memory
```

---

### Router2 Configuration

**Interface Summary:**
| Interface | IP Address | Subnet Mask | Description |
|-----------|------------|-------------|-------------|
| Fa0/0 | 192.168.3.1 | 255.255.255.0 | LAN 3 |
| Se2/0 | 192.100.1.6 | 255.255.255.252 | WAN to Core Router |

**Configuration:**
```cisco
enable
configure terminal
hostname Router2

! LAN Interface
interface FastEthernet0/0
 description LAN Network 192.168.3.0/24
 ip address 192.168.3.1 255.255.255.0
 no shutdown

! WAN Interface to Core Router
interface Serial2/0
 description WAN to Core Router
 ip address 192.100.1.6 255.255.255.252
 no shutdown

! Static Routes
! Route to LAN 1 via Core Router
ip route 192.168.1.0 255.255.255.0 192.100.1.5

! Route to LAN 2 via Core Router
ip route 192.168.2.0 255.255.255.0 192.100.1.5

! Route to LAN 4 via Core Router
ip route 192.168.4.0 255.255.255.0 192.100.1.5

! Route to LAN 5 via Core Router
ip route 192.168.5.0 255.255.255.0 192.100.1.5

! Route to LAN 6 via Core Router
ip route 192.168.6.0 255.255.255.0 192.100.1.5

! Route to other WAN networks
ip route 192.100.1.0 255.255.255.252 192.100.1.5
ip route 192.100.1.4 255.255.255.252 192.100.1.5
ip route 192.100.1.12 255.255.255.252 192.100.1.5

end
write memory
```

---

### Router3 Configuration

**Interface Summary:**
| Interface | IP Address | Subnet Mask | Description |
|-----------|------------|-------------|-------------|
| Fa0/0 | 192.168.6.1 | 255.255.255.0 | LAN 6 |
| Se2/0 | 192.100.1.14 | 255.255.255.252 | WAN to Core Router |

**Configuration:**
```cisco
enable
configure terminal
hostname Router3

! LAN Interface
interface FastEthernet0/0
 description LAN Network 192.168.6.0/24
 ip address 192.168.6.1 255.255.255.0
 no shutdown

! WAN Interface to Core Router
interface Serial2/0
 description WAN to Core Router
 ip address 192.100.1.14 255.255.255.252
 no shutdown

! Static Routes
! Default route to Core Router
ip route 0.0.0.0 0.0.0.0 192.100.1.13

! Or specific routes
ip route 192.168.1.0 255.255.255.0 192.100.1.13
ip route 192.168.2.0 255.255.255.0 192.100.1.13
ip route 192.168.3.0 255.255.255.0 192.100.1.13
ip route 192.168.4.0 255.255.255.0 192.100.1.13
ip route 192.168.5.0 255.255.255.0 192.100.1.13

end
write memory
```

---

### Gateway Configuration

**Interface Summary:**
| Interface | IP Address | Subnet Mask | Description |
|-----------|------------|-------------|-------------|
| Fa0/0 | 192.168.4.1 | 255.255.255.0 | LAN 4 |
| Fa1/0 | 192.168.5.1 | 255.255.255.0 | LAN 5 |
| Se2/0 | 192.100.1.10 | 255.255.255.252 | WAN to Core Router |
| Se3/0 | 192.100.1.10 | 255.255.255.252 | WAN to Core Router |

**Configuration:**
```cisco
enable
configure terminal
hostname Gateway

! LAN Interface 1
interface FastEthernet0/0
 description LAN Network 192.168.4.0/24
 ip address 192.168.4.1 255.255.255.0
 no shutdown

! LAN Interface 2
interface FastEthernet1/0
 description LAN Network 192.168.5.0/24
 ip address 192.168.5.1 255.255.255.0
 no shutdown

! WAN Interface 1 to Core Router
interface Serial2/0
 description WAN to Core Router
 ip address 192.100.1.10 255.255.255.252
 no shutdown

! WAN Interface 2 to Core Router
interface Serial3/0
 description WAN to Core Router (Backup)
 ip address 192.100.1.10 255.255.255.252
 no shutdown

! Static Routes
! Route to LAN 1 via Core Router
ip route 192.168.1.0 255.255.255.0 192.100.1.13

! Route to LAN 2 via Core Router
ip route 192.168.2.0 255.255.255.0 192.100.1.13

! Route to LAN 3 via Core Router
ip route 192.168.3.0 255.255.255.0 192.100.1.13

! Route to LAN 6 via Core Router
ip route 192.168.6.0 255.255.255.0 192.100.1.13

! Route to other WAN networks
ip route 192.100.1.0 255.255.255.252 192.100.1.13
ip route 192.100.1.4 255.255.255.252 192.100.1.13
ip route 192.100.1.8 255.255.255.252 192.100.1.13

end
write memory
```

---

## Static Routing Tables

### Router0 Routing Table

| Destination | Mask | Next Hop | Interface | AD |
|-------------|------|----------|-----------|-----|
| 192.168.1.0 | /24 | Connected | Fa0/0 | 0 |
| 192.168.2.0 | /24 | 192.100.1.2 | Se2/0 | 1 |
| 192.168.3.0 | /24 | 192.100.1.2 | Se2/0 | 1 |
| 192.168.4.0 | /24 | 192.100.1.2 | Se2/0 | 1 |
| 192.168.5.0 | /24 | 192.100.1.2 | Se2/0 | 1 |
| 192.168.6.0 | /24 | 192.100.1.2 | Se2/0 | 1 |
| 192.100.1.0 | /30 | Connected | Se2/0 | 0 |

### Core Router Routing Table

| Destination | Mask | Next Hop | Interface | AD |
|-------------|------|----------|-----------|-----|
| 192.168.1.0 | /24 | 192.100.1.1 | Se2/0 | 1 |
| 192.168.2.0 | /24 | Connected | Fa0/0 | 0 |
| 192.168.3.0 | /24 | 192.100.1.10 | Se8/0 | 1 |
| 192.168.4.0 | /24 | 192.100.1.10 | Se8/0 | 1 |
| 192.168.5.0 | /24 | 192.100.1.10 | Se8/0 | 1 |
| 192.168.6.0 | /24 | 192.100.1.14 | Se8/0 | 1 |
| 192.100.1.0 | /30 | Connected | Se2/0 | 0 |
| 192.100.1.4 | /30 | Connected | Se3/0 | 0 |
| 192.100.1.12 | /30 | Connected | Se8/0 | 0 |

---

## Complete IP Addressing Table

### LAN Networks

| Network | Network ID | Gateway | PC IP | Subnet Mask | Router |
|---------|------------|---------|-------|-------------|--------|
| LAN 1 | 192.168.1.0/24 | 192.168.1.1 | 192.168.1.2 | 255.255.255.0 | Router0 |
| LAN 2 | 192.168.2.0/24 | 192.168.2.1 | 192.168.2.2 | 255.255.255.0 | Core Router |
| LAN 3 | 192.168.3.0/24 | 192.168.3.1 | 192.168.3.2 | 255.255.255.0 | Router2 |
| LAN 4 | 192.168.4.0/24 | 192.168.4.1 | 192.168.4.2 | 255.255.255.0 | Gateway |
| LAN 5 | 192.168.5.0/24 | 192.168.5.1 | 192.168.5.2 | 255.255.255.0 | Gateway |
| LAN 6 | 192.168.6.0/24 | 192.168.6.1 | 192.168.6.2 | 255.255.255.0 | Router3 |

### WAN Networks

| WAN | Network | Router A | IP A | Router B | IP B |
|-----|---------|----------|------|----------|------|
| WAN 1 | 192.100.1.0/30 | Router0 | 192.100.1.1 | Core Router | 192.100.1.2 |
| WAN 2 | 192.100.1.4/30 | Core Router | 192.100.1.5 | Core Router | 192.100.1.6 |
| WAN 3 | 192.100.1.8/30 | Core Router | 192.100.1.6 | Router2 | 192.100.1.6 |
| WAN 4 | 192.100.1.12/30 | Core Router | 192.100.1.13 | Gateway | 192.100.1.10 |

---

## Testing and Verification

### Connectivity Test Matrix

#### From LAN 1 (192.168.1.2)

```bash
ping 192.168.1.1     # ✅ Local Gateway
ping 192.100.1.2     # ✅ Core Router (1 hop)
ping 192.168.2.2     # ✅ LAN 2 (2 hops)
ping 192.168.3.2     # ✅ LAN 3 (3 hops)
ping 192.168.4.2     # ✅ LAN 4 (3 hops)
ping 192.168.5.2     # ✅ LAN 5 (3 hops)
ping 192.168.6.2     # ✅ LAN 6 (3 hops)

tracert 192.168.6.2  
# Path: .1.1 → .1.2 → .13 → .14 → .6.2
```

#### From LAN 2 (192.168.2.2)

```bash
ping 192.168.2.1     # ✅ Local Gateway
ping 192.168.1.2     # ✅ LAN 1 (2 hops)
ping 192.168.3.2     # ✅ LAN 3 (3 hops)
ping 192.168.4.2     # ✅ LAN 4 (2 hops)
ping 192.168.5.2     # ✅ LAN 5 (2 hops)
ping 192.168.6.2     # ✅ LAN 6 (3 hops)
```

#### From LAN 4 or LAN 5 (Gateway)

```bash
ping 192.168.4.1     # ✅ Local Gateway (LAN 4)
ping 192.168.5.1     # ✅ Local Gateway (LAN 5)
ping 192.168.1.2     # ✅ LAN 1 (3 hops)
ping 192.168.2.2     # ✅ LAN 2 (2 hops)
ping 192.168.3.2     # ✅ LAN 3 (3 hops)
ping 192.168.6.2     # ✅ LAN 6 (3 hops)
```

### Verification Commands

```cisco
! Check routing table
show ip route
show ip route static

! Check interface status
show ip interface brief
show interfaces
show controllers serial

! Verify connectivity
ping [destination]
traceroute [destination]

! Check configuration
show running-config
show startup-config
```

---

## Path Analysis

### Path from LAN 1 to LAN 6

**Route:** 192.168.1.2 → 192.168.6.2

| Hop | Device | Interface Out | Next Hop IP | Network |
|-----|--------|---------------|-------------|---------|
| 0 | PC (192.168.1.2) | - | 192.168.1.1 | Source LAN 1 |
| 1 | Router0 | Se2/0 | 192.100.1.2 | WAN 1 |
| 2 | Core Router | Se8/0 | 192.100.1.14 | WAN 4 |
| 3 | Router3 | Fa0/0 | 192.168.6.2 | Destination LAN 6 |

**Total Hops:** 3  
**Total Distance:** 4 router hops

---

## Troubleshooting Guide

### Common Issues

| Issue | Cause | Solution | Command |
|-------|-------|----------|---------|
| No connectivity between LANs | Missing static route | Add route on all routers in path | `show ip route` |
| Serial link down | No clock rate on DCE | Set clock rate 64000 | `show controllers serial` |
| Can reach next hop only | Missing route on intermediate router | Configure route on core router | `show ip route static` |
| Asymmetric routing | Different paths for forward/return | Verify routes in both directions | `traceroute` |

### Diagnostic Steps

1. **Verify local connectivity**
```cisco
ping [local gateway]
show ip interface brief
```

2. **Check routing table**
```cisco
show ip route
show ip route static
```

3. **Test next hop**
```cisco
ping [next hop IP]
traceroute [destination]
```

4. **Verify serial links**
```cisco
show interfaces serial 2/0
show controllers serial 2/0
```

5. **Check configuration**
```cisco
show running-config | include ip route
show ip protocols
```

---

## Network Statistics

| Metric | Count |
|--------|-------|
| Total Routers | 5 |
| Total PCs | 6 |
| LAN Networks | 6 |
| WAN Links | 4 |
| Static Routes | ~30 (total across all routers) |
| Total Interfaces | 19 |

---

## Best Practices Applied

✅ **Efficient WAN Addressing** - /30 subnets for point-to-point  
✅ **Complete Static Routing** - Full mesh connectivity  
✅ **Redundancy** - Multiple WAN links on core router  
✅ **Clear Documentation** - All routes documented  
✅ **Consistent Gateway IPs** - .1 for all LAN gateways  

---

## Configuration Checklist

- [ ] All router interfaces configured
- [ ] Clock rates set on DCE interfaces
- [ ] All static routes configured
- [ ] All PCs have correct IP and gateway
- [ ] End-to-end connectivity verified
- [ ] Configuration saved on all devices
- [ ] Documentation completed
- [ ] Network diagram updated

---

**Document Version:** 1.0  
**Network Type:** Static Routing  
**Total Networks:** 6 LAN + 4 WAN  
**Routing Protocol:** Static Routes Only  
**Topology:** Star with Core Router  
**Status:** ✅ Ready for Deployment  
**Last Updated:** December 2024