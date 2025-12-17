# OSPF Multi-Area Quick Guide
![alt text](<src/assets/Screenshot 2025-12-17 142839.png>)
---
## Network Overview

| Item | Value |
|------|-------|
| Routing Protocol | OSPF (OSPFv2) |
| Process ID | 1 |
| Total Routers | 6 |
| Total Areas | 4 (Area 0, 1, 2, 3) |
| Total PCs | 3 |
| Telnet Password | ospf123 |
| Enable Password | cisco |

---

## OSPF Areas

| Area | Type | Color | Routers | Networks |
|------|------|-------|---------|----------|
| Area 0 | Backbone | Yellow | Router-PT (top 3) | 10.0.0.0/30, 10.0.0.4/30 |
| Area 1 | Standard | Pink | Router3 | 192.168.1.0/24, 10.0.0.8/30 |
| Area 2 | Standard | Green | Router4 | 192.168.2.0/24, 10.0.0.12/30 |
| Area 3 | Standard | Orange | Router5 | 192.168.3.0/24, 10.0.0.16/30 |

---

## OSPF Topology

```
        AREA 0 (Backbone - Yellow)
    [RouterL]---[RouterC]---[RouterR]
        |           |           |
     Area 1      Area 2      Area 3
    (Pink)      (Green)     (Orange)
        |           |           |
   [Router3]   [Router4]   [Router5]
     Fa0/0       Fa0/0       Fa0/0
        |           |           |
     [PC1.2]     [PC2.2]     [PC3.2]
```

---

## IP Addressing

### Area 0 (Backbone) - WAN Links

| Link | Network | Router A | IP A | Router B | IP B |
|------|---------|----------|------|----------|------|
| 1 | 10.0.0.0/30 | RouterL | Fa4/0 | RouterC | Fa4/0 |
| 2 | 10.0.0.4/30 | RouterC | Fa5/0 | RouterR | Fa4/0 |

### Area Connections - WAN Links

| Area | Network | ABR | IP | Internal Router | IP |
|------|---------|-----|----|-----------------|----|
| Area 1 | 10.0.0.8/30 | RouterL Se2/0 | .9 | Router3 Se2/0 | .10 |
| Area 2 | 10.0.0.12/30 | RouterC Se2/0 | .13 | Router4 Se2/0 | .14 |
| Area 3 | 10.0.0.16/30 | RouterR Se2/0 | .17 | Router5 Se2/0 | .18 |

### LAN Networks

| Area | Network | Gateway | PC IP | Router |
|------|---------|---------|-------|--------|
| Area 1 | 192.168.1.0/24 | 192.168.1.1 | 192.168.1.2 | Router3 Fa0/0 |
| Area 2 | 192.168.2.0/24 | 192.168.2.1 | 192.168.2.2 | Router4 Fa0/0 |
| Area 3 | 192.168.3.0/24 | 192.168.3.1 | 192.168.3.2 | Router5 Fa0/0 |

---

## OSPFv2 Configuration

### RouterL (Left ABR - Area 0 & Area 1)

```cisco
hostname RouterL

interface FastEthernet4/0
 ip address 10.0.0.1 255.255.255.252
 no shutdown

interface Serial2/0
 ip address 10.0.0.9 255.255.255.252
 clock rate 64000
 no shutdown

router ospf 1
 router-id 1.1.1.1
 network 10.0.0.0 0.0.0.3 area 0
 network 10.0.0.8 0.0.0.3 area 1

line vty 0 4
 password ospf123
 login
enable secret cisco
```

### RouterC (Center ABR - Area 0 & Area 2)

```cisco
hostname RouterC

interface FastEthernet4/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown

interface FastEthernet5/0
 ip address 10.0.0.5 255.255.255.252
 no shutdown

interface Serial2/0
 ip address 10.0.0.13 255.255.255.252
 clock rate 64000
 no shutdown

router ospf 1
 router-id 2.2.2.2
 network 10.0.0.0 0.0.0.3 area 0
 network 10.0.0.4 0.0.0.3 area 0
 network 10.0.0.12 0.0.0.3 area 2

line vty 0 4
 password ospf123
 login
enable secret cisco
```

### RouterR (Right ABR - Area 0 & Area 3)

```cisco
hostname RouterR

interface FastEthernet4/0
 ip address 10.0.0.6 255.255.255.252
 no shutdown

interface Serial2/0
 ip address 10.0.0.17 255.255.255.252
 clock rate 64000
 no shutdown

router ospf 1
 router-id 3.3.3.3
 network 10.0.0.4 0.0.0.3 area 0
 network 10.0.0.16 0.0.0.3 area 3

line vty 0 4
 password ospf123
 login
enable secret cisco
```

### Router3 (Area 1 Internal)

```cisco
hostname Router3

interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface Serial2/0
 ip address 10.0.0.10 255.255.255.252
 no shutdown

router ospf 1
 router-id 4.4.4.4
 network 192.168.1.0 0.0.0.255 area 1
 network 10.0.0.8 0.0.0.3 area 1
 passive-interface FastEthernet0/0

line vty 0 4
 password ospf123
 login
enable secret cisco
```

### Router4 (Area 2 Internal)

```cisco
hostname Router4

interface FastEthernet0/0
 ip address 192.168.2.1 255.255.255.0
 no shutdown

interface Serial2/0
 ip address 10.0.0.14 255.255.255.252
 no shutdown

router ospf 1
 router-id 5.5.5.5
 network 192.168.2.0 0.0.0.255 area 2
 network 10.0.0.12 0.0.0.3 area 2
 passive-interface FastEthernet0/0

line vty 0 4
 password ospf123
 login
enable secret cisco
```

### Router5 (Area 3 Internal)

```cisco
hostname Router5

interface FastEthernet0/0
 ip address 192.168.3.1 255.255.255.0
 no shutdown

interface Serial2/0
 ip address 10.0.0.18 255.255.255.252
 no shutdown

router ospf 1
 router-id 6.6.6.6
 network 192.168.3.0 0.0.0.255 area 3
 network 10.0.0.16 0.0.0.3 area 3
 passive-interface FastEthernet0/0

line vty 0 4
 password ospf123
 login
enable secret cisco
```

---

## OSPF Key Concepts

### Router Types

| Type | Description | Routers in This Network |
|------|-------------|-------------------------|
| **ABR** | Area Border Router (connects multiple areas) | RouterL, RouterC, RouterR |
| **Internal** | Router entirely within one area | Router3, Router4, Router5 |
| **Backbone** | Router in Area 0 | RouterL, RouterC, RouterR |

### OSPF Characteristics

| Feature | Value |
|---------|-------|
| Protocol Type | Link-State |
| Algorithm | Dijkstra SPF |
| Metric | Cost (based on bandwidth) |
| Admin Distance | 110 |
| Multicast | 224.0.0.5 (All OSPF routers), 224.0.0.6 (DR/BDR) |
| Hello Timer | 10 seconds (broadcast), 30 seconds (non-broadcast) |
| Dead Timer | 40 seconds (broadcast), 120 seconds (non-broadcast) |

---

## Verification Commands

### OSPF Status

```cisco
show ip ospf                      # OSPF process info
show ip ospf neighbor             # View neighbors
show ip ospf interface            # OSPF interface details
show ip ospf database             # Link-state database
show ip route ospf                # OSPF routes only
show ip protocols                 # Routing protocols
```

### Sample Output - show ip ospf neighbor

```
Router# show ip ospf neighbor

Neighbor ID     Pri   State      Dead Time   Address        Interface
2.2.2.2          1    FULL/DR    00:00:35    10.0.0.2       Fa4/0
4.4.4.4          0    FULL/-     00:00:32    10.0.0.10      Se2/0
```

### Sample Output - show ip ospf database

```
Router# show ip ospf database

            OSPF Router with ID (1.1.1.1) (Process ID 1)

                Router Link States (Area 0)
Link ID         ADV Router      Age  Seq#       Checksum Link count
1.1.1.1         1.1.1.1         250  0x80000003 0x00B5C8 2
2.2.2.2         2.2.2.2         225  0x80000004 0x00C3D9 3

                Summary Net Link States (Area 0)
Link ID         ADV Router      Age  Seq#       Checksum
192.168.1.0     1.1.1.1         125  0x80000001 0x00AB12
```

---

## OSPFv1 vs OSPFv2 Quick Comparison

| Feature | OSPFv1 (Not Standard) | OSPFv2 (Standard) |
|---------|----------------------|-------------------|
| **IP Version** | IPv4 | IPv4 |
| **Configuration** | `router ospf 1` | `router ospf 1` |
| **Network Command** | Uses wildcard mask | Uses wildcard mask |
| **Authentication** | Limited | MD5, Clear text |
| **Multicast** | 224.0.0.5, 224.0.0.6 | 224.0.0.5, 224.0.0.6 |
| **Standard** | Not official | RFC 2328 |

**Note:** In Cisco, "OSPF version 2" refers to OSPFv2 which is the standard OSPF for IPv4. OSPFv3 is for IPv6.

---

## Alternative: OSPFv2 Enhanced Configuration

### With Authentication (MD5)

```cisco
! On interface
interface Serial2/0
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 MyKey123

! Or area-wide
router ospf 1
 area 1 authentication message-digest
```

### With Manual Cost

```cisco
interface Serial2/0
 ip ospf cost 100
```

### With Priority (DR/BDR Election)

```cisco
interface FastEthernet4/0
 ip ospf priority 100
```

### Advanced OSPF Features

```cisco
router ospf 1
 router-id 1.1.1.1
 
 ! Area configuration
 network 10.0.0.0 0.0.0.3 area 0
 network 10.0.0.8 0.0.0.3 area 1
 
 ! Passive interface
 passive-interface default
 no passive-interface Serial2/0
 
 ! Default route advertisement
 default-information originate
 
 ! Route summarization
 area 1 range 192.168.0.0 255.255.0.0
 
 ! Stub area configuration
 area 1 stub
 
 ! Adjust timers
 timers throttle spf 5000 10000 20000
```

---

## OSPFv2 Features Not in Basic Config

### 1. Stub Areas

```cisco
! On all routers in Area 1
router ospf 1
 area 1 stub

! On ABR only (Totally Stubby)
router ospf 1
 area 1 stub no-summary
```

### 2. NSSA (Not-So-Stubby Area)

```cisco
router ospf 1
 area 1 nssa
```

### 3. Virtual Links (if non-contiguous areas)

```cisco
router ospf 1
 area 2 virtual-link 3.3.3.3
```

### 4. Route Filtering

```cisco
router ospf 1
 distribute-list 10 in
 
access-list 10 deny 192.168.99.0 0.0.0.255
access-list 10 permit any
```

---

## Testing & Verification

### Connectivity Test

```bash
# From PC in Area 1
ping 192.168.1.1          # Local gateway
ping 192.168.2.2          # PC in Area 2
ping 192.168.3.2          # PC in Area 3
tracert 192.168.3.2       # View path
```

### OSPF Neighbor Check

```cisco
! Should see neighbors in FULL state
show ip ospf neighbor

! Check interface OSPF status
show ip ospf interface brief
```

### Route Verification

```cisco
show ip route ospf
show ip ospf database
```

---

## Troubleshooting

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| No neighbors | Area mismatch | Verify area numbers |
| Neighbor stuck in INIT | ACL blocking | Check access-lists |
| Wrong routes | Network statement | Verify network commands |
| High CPU | Too many routes | Implement summarization |
| Authentication fail | Key mismatch | Verify MD5 keys |

### Debug Commands (Use Carefully)

```cisco
debug ip ospf hello
debug ip ospf adj
debug ip ospf events
no debug all
```

---

## Quick Configuration Steps

1. **Configure interfaces** with IP addresses
2. **Enable OSPF** process: `router ospf 1`
3. **Set router-id**: `router-id x.x.x.x`
4. **Add networks** with correct area: `network x.x.x.x 0.0.0.x area X`
5. **Configure passive** interfaces on LAN ports
6. **Set telnet** password on VTY lines
7. **Verify neighbors**: `show ip ospf neighbor`
8. **Check routes**: `show ip route ospf`

---

## Network Summary

| Item | Details |
|------|---------|
| Routers | 6 (3 ABRs, 3 Internal) |
| PCs | 3 |
| OSPF Process ID | 1 |
| Areas | 4 (Area 0, 1, 2, 3) |
| Area 0 Routers | RouterL, RouterC, RouterR |
| ABRs | RouterL, RouterC, RouterR |
| Telnet Password | ospf123 |
| Enable Password | cisco |
| Protocol | OSPFv2 |
| Admin Distance | 110 |

---

## Checklist

- [ ] Configure all router interfaces
- [ ] Set clock rates on DCE serial interfaces
- [ ] Enable OSPF process 1 on all routers
- [ ] Set unique router-id for each router
- [ ] Add network statements with correct areas
- [ ] Configure passive interfaces on LAN ports
- [ ] Set telnet password (ospf123)
- [ ] Set enable password (cisco)
- [ ] Verify OSPF neighbors in FULL state
- [ ] Check Area 0 has all ABR connections
- [ ] Test inter-area routing
- [ ] Verify all PCs can reach each other
- [ ] Save configurations

---

**Routing Protocol:** OSPF Multi-Area (OSPFv2)  
**Process ID:** 1  
**Total Areas:** 4 (Backbone + 3 Standard)  
**Telnet:** ospf123  
**Enable:** cisco  
**Status:** ✅ Ready for Deployment