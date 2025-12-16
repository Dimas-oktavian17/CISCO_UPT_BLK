# RIP Version 1 Network Documentation

## Network Overview

This documentation covers a RIP (Routing Information Protocol) version 1 dynamic routing topology with 3 routers and multiple networks.

| Network | Network ID | CIDR | Gateway | Hosts | Routing |
|---------|------------|------|---------|-------|---------|
| NetA | 192.168.1.0 | /24 | 192.168.1.1 | 1 PC | RIP v1 |
| NetB | 192.168.2.0 | /24 | 192.168.2.1 | 1 PC | RIP v1 |
| NetC | 192.168.3.0 | /24 | 192.168.3.1 | 1 PC | RIP v1 |
| NetD | 192.168.4.0 | /24 | 192.168.4.1 | 1 PC | RIP v1 |
| NetE | 192.168.5.0 | /24 | 192.168.5.1 | 1 PC | RIP v1 |
| NetD (Right) | 192.168.6.0 | /24 | 192.168.6.1 | 1 PC | RIP v1 |
| WAN 1 | 192.100.1.0 | /30 | - | Serial Link | RIP v1 |
| WAN 2 | 192.100.1.4 | /30 | - | Serial Link | RIP v1 |
| WAN 3 | 192.100.1.8 | /30 | - | Serial Link | RIP v1 |

---

## Network Topology Structure

```
[NetA]     [NetB]              [WAN 1]              [WAN 2]           [NetE]    [NetD]
.1.0/24    .2.0/24         192.100.1.0/30       192.100.1.4/30      .5.0/24   .6.0/24
   |          |                  |                    |                  |         |
[PC1.2]  [PC2.2]           [RouterA]===========[RouterB]============[RouterC]-[PC6.2]
          Fa0   Fa1/0      Se2/0   Se3/0       Se2/0  Se3/0  Fa1/0   Se3/0  Fa0/0
                  |           |                    |      |     |        |
              [NetC]      [WAN 1]              [WAN 2][WAN3][NetE]   [NetD]
              .3.0/24    192.100.1.0/30        .4/30  .8/30 .5.0/24  .6.0/24
                  |          |                    |      |
              [PC3.2]    .1/.2              .5/.6  .9/.10
                                                |      |
                                            [NetC]  [NetD]
                                            .3.0/24  .4.0/24
                                               |        |
                                           [PC3.2]  [PC4.2]
```

---

## RIP Version 1 Overview

### RIP v1 Characteristics

| Feature | Value/Description |
|---------|------------------|
| Protocol Type | Distance Vector |
| Algorithm | Bellman-Ford |
| Metric | Hop Count (max 15) |
| Administrative Distance | 120 |
| Update Timer | 30 seconds |
| Invalid Timer | 180 seconds |
| Holddown Timer | 180 seconds |
| Flush Timer | 240 seconds |
| Classful Routing | Yes (No VLSM support) |
| Subnet Mask | Not advertised |
| Authentication | Not supported |
| Multicast Address | Broadcast (255.255.255.255) |

### RIP v1 Limitations

⚠️ **Classful Routing** - Does not send subnet mask information  
⚠️ **No VLSM Support** - All subnets must use same mask  
⚠️ **No Authentication** - Less secure than RIP v2  
⚠️ **Broadcast Updates** - Uses more bandwidth  
⚠️ **15 Hop Limit** - Limited network size  
⚠️ **Slow Convergence** - Takes time to adapt to changes  

---

## Complete Device Inventory

### Routers

| Router | Hostname | Role | Interfaces | Networks |
|--------|----------|------|------------|----------|
| RouterA | RouterA | Edge Router | Fa0, Fa1/0, Se2/0, Se3/0 | NetA, NetB, NetC, WAN1 |
| RouterB | RouterB | Core Router | Fa0/0, Fa1/0, Se2/0, Se3/0 | NetC, NetD, WAN1, WAN2, WAN3 |
| RouterC | RouterC | Edge Router | Fa0/0, Fa1/0, Se3/0 | NetE, NetD, WAN2 |

### End Devices

| Device | Hostname | IP Address | Subnet Mask | Default Gateway | Network | Connected To |
|--------|----------|------------|-------------|-----------------|---------|--------------|
| PC-PT | NetA | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 | NetA | RouterA Fa0 |
| PC-PT | NetB | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 | NetB | RouterA Fa0 |
| PC-PT | NetC | 192.168.3.2 | 255.255.255.0 | 192.168.3.1 | NetC | RouterA Fa1/0 |
| PC-PT | NetD | 192.168.4.2 | 255.255.255.0 | 192.168.4.1 | NetD | RouterB Fa1/0 |
| PC-PT | NetE | 192.168.5.2 | 255.255.255.0 | 192.168.5.1 | NetE | RouterC Fa0/0 |
| PC-PT | NetD | 192.168.6.2 | 255.255.255.0 | 192.168.6.1 | NetD | RouterC Fa1/0 |

---

## WAN Link Details

### WAN 1: 192.100.1.0/30

| Router | Interface | IP Address | Subnet Mask | Type |
|--------|-----------|------------|-------------|------|
| RouterA | Se2/0 | 192.100.1.1 | 255.255.255.252 | DCE |
| RouterA | Se3/0 | 192.100.1.5 | 255.255.255.252 | DCE |

**Network Info:**
- Network: 192.100.1.0
- Usable: 192.100.1.1 - 192.100.1.2
- Broadcast: 192.100.1.3

### WAN 2: 192.100.1.4/30

| Router | Interface | IP Address | Subnet Mask | Type |
|--------|-----------|------------|-------------|------|
| RouterB | Se2/0 | 192.100.1.6 | 255.255.255.252 | DTE |
| RouterC | Se3/0 | 192.100.1.10 | 255.255.255.252 | DTE |

**Network Info:**
- Network: 192.100.1.4
- Usable: 192.100.1.5 - 192.100.1.6
- Broadcast: 192.100.1.7

### WAN 3: 192.100.1.8/30

| Router | Interface | IP Address | Subnet Mask | Type |
|--------|-----------|------------|-------------|------|
| RouterB | Se3/0 | 192.100.1.9 | 255.255.255.252 | DCE |
| RouterB | Se2/0 | 192.100.1.2 | 255.255.255.252 | DTE |

**Network Info:**
- Network: 192.100.1.8
- Usable: 192.100.1.9 - 192.100.1.10
- Broadcast: 192.100.1.11

---

## Router Configurations with RIP v1

### RouterA Configuration

**Interface Summary:**
| Interface | IP Address | Subnet Mask | Network | Description |
|-----------|------------|-------------|---------|-------------|
| Fa0 | 192.168.1.1 | 255.255.255.0 | NetA | LAN |
| Fa0 | 192.168.2.1 | 255.255.255.0 | NetB | LAN |
| Fa1/0 | 192.168.3.1 | 255.255.255.0 | NetC | LAN |
| Se2/0 | 192.100.1.1 | 255.255.255.252 | WAN 1 | To RouterB |
| Se3/0 | 192.100.1.5 | 255.255.255.252 | WAN 2 | To RouterB |

**Complete Configuration:**
```cisco
enable
configure terminal
hostname RouterA

! LAN Interface - NetA
interface FastEthernet0/0
 description LAN NetA 192.168.1.0/24
 ip address 192.168.1.1 255.255.255.0
 no shutdown

! LAN Interface - NetB (sub-interface or second port)
interface FastEthernet0/1
 description LAN NetB 192.168.2.0/24
 ip address 192.168.2.1 255.255.255.0
 no shutdown

! LAN Interface - NetC
interface FastEthernet1/0
 description LAN NetC 192.168.3.0/24
 ip address 192.168.3.1 255.255.255.0
 no shutdown

! WAN Interface to RouterB - Link 1
interface Serial2/0
 description WAN to RouterB
 ip address 192.100.1.1 255.255.255.252
 clock rate 64000
 no shutdown

! WAN Interface to RouterB - Link 2
interface Serial3/0
 description WAN to RouterB
 ip address 192.100.1.5 255.255.255.252
 clock rate 64000
 no shutdown

! RIP Version 1 Configuration
router rip
 version 1
 network 192.168.1.0
 network 192.168.2.0
 network 192.168.3.0
 network 192.100.1.0
 passive-interface FastEthernet0/0
 passive-interface FastEthernet0/1
 passive-interface FastEthernet1/0

end
write memory
```

---

### RouterB Configuration

**Interface Summary:**
| Interface | IP Address | Subnet Mask | Network | Description |
|-----------|------------|-------------|---------|-------------|
| Fa0/0 | 192.168.3.1 | 255.255.255.0 | NetC | LAN |
| Fa1/0 | 192.168.4.1 | 255.255.255.0 | NetD | LAN |
| Se2/0 | 192.100.1.2 or .6 | 255.255.255.252 | WAN | To RouterA/C |
| Se3/0 | 192.100.1.9 | 255.255.255.252 | WAN 3 | To RouterB |

**Complete Configuration:**
```cisco
enable
configure terminal
hostname RouterB

! LAN Interface - NetC
interface FastEthernet0/0
 description LAN NetC 192.168.3.0/24
 ip address 192.168.3.1 255.255.255.0
 no shutdown

! LAN Interface - NetD
interface FastEthernet1/0
 description LAN NetD 192.168.4.0/24
 ip address 192.168.4.1 255.255.255.0
 no shutdown

! WAN Interface to RouterA
interface Serial2/0
 description WAN to RouterA
 ip address 192.100.1.2 255.255.255.252
 no shutdown

! WAN Interface to RouterC
interface Serial3/0
 description WAN to RouterC
 ip address 192.100.1.9 255.255.255.252
 clock rate 64000
 no shutdown

! Additional WAN Interface
interface Serial6/0
 description WAN Link
 ip address 192.100.1.6 255.255.255.252
 no shutdown

! RIP Version 1 Configuration
router rip
 version 1
 network 192.168.3.0
 network 192.168.4.0
 network 192.100.1.0
 passive-interface FastEthernet0/0
 passive-interface FastEthernet1/0

end
write memory
```

---

### RouterC Configuration

**Interface Summary:**
| Interface | IP Address | Subnet Mask | Network | Description |
|-----------|------------|-------------|---------|-------------|
| Fa0/0 | 192.168.5.1 | 255.255.255.0 | NetE | LAN |
| Fa1/0 | 192.168.6.1 | 255.255.255.0 | NetD | LAN |
| Se3/0 | 192.100.1.10 | 255.255.255.252 | WAN 2 | To RouterB |

**Complete Configuration:**
```cisco
enable
configure terminal
hostname RouterC

! LAN Interface - NetE
interface FastEthernet0/0
 description LAN NetE 192.168.5.0/24
 ip address 192.168.5.1 255.255.255.0
 no shutdown

! LAN Interface - NetD
interface FastEthernet1/0
 description LAN NetD 192.168.6.0/24
 ip address 192.168.6.1 255.255.255.0
 no shutdown

! WAN Interface to RouterB
interface Serial3/0
 description WAN to RouterB
 ip address 192.100.1.10 255.255.255.252
 no shutdown

! RIP Version 1 Configuration
router rip
 version 1
 network 192.168.5.0
 network 192.168.6.0
 network 192.100.1.0
 passive-interface FastEthernet0/0
 passive-interface FastEthernet1/0

end
write memory
```

---

## RIP Configuration Explained

### RIP Network Command

```cisco
router rip
 version 1
 network 192.168.1.0
 network 192.100.1.0
```

**Explanation:**
- `router rip` - Enables RIP routing process
- `version 1` - Specifies RIP version 1
- `network [classful-network]` - Advertises networks in classful manner
- RIP v1 automatically summarizes to classful boundaries

### Passive Interface

```cisco
passive-interface FastEthernet0/0
```

**Purpose:**
- Prevents RIP updates from being sent on this interface
- Still advertises the network
- Saves bandwidth on LAN interfaces
- Improves security (no routing updates to end users)

---

## Complete IP Addressing Table

### LAN Networks

| Network | Network ID | Gateway | PC IP | Subnet Mask | Router Interface |
|---------|------------|---------|-------|-------------|-----------------|
| NetA | 192.168.1.0/24 | 192.168.1.1 | 192.168.1.2 | 255.255.255.0 | RouterA Fa0/0 |
| NetB | 192.168.2.0/24 | 192.168.2.1 | 192.168.2.2 | 255.255.255.0 | RouterA Fa0/1 |
| NetC | 192.168.3.0/24 | 192.168.3.1 | 192.168.3.2 | 255.255.255.0 | RouterA Fa1/0 |
| NetD | 192.168.4.0/24 | 192.168.4.1 | 192.168.4.2 | 255.255.255.0 | RouterB Fa1/0 |
| NetE | 192.168.5.0/24 | 192.168.5.1 | 192.168.5.2 | 255.255.255.0 | RouterC Fa0/0 |
| NetD | 192.168.6.0/24 | 192.168.6.1 | 192.168.6.2 | 255.255.255.0 | RouterC Fa1/0 |

### WAN Networks

| WAN | Network | Router A | IP A | Router B | IP B | Type |
|-----|---------|----------|------|----------|------|------|
| WAN 1 | 192.100.1.0/30 | RouterA | 192.100.1.1 | RouterB | 192.100.1.2 | Serial |
| WAN 1 | 192.100.1.0/30 | RouterA | 192.100.1.5 | RouterB | 192.100.1.6 | Serial |
| WAN 2 | 192.100.1.4/30 | RouterB | 192.100.1.6 | RouterC | 192.100.1.10 | Serial |
| WAN 3 | 192.100.1.8/30 | RouterB | 192.100.1.9 | RouterB | 192.100.1.10 | Serial |

---

## RIP Routing Tables

### Expected Routing Table - RouterA

```cisco
RouterA# show ip route

Gateway of last resort is not set

     192.168.1.0/24 is directly connected, FastEthernet0/0
     192.168.2.0/24 is directly connected, FastEthernet0/1
     192.168.3.0/24 is directly connected, FastEthernet1/0
R    192.168.4.0/24 [120/1] via 192.100.1.2, 00:00:15, Serial2/0
R    192.168.5.0/24 [120/2] via 192.100.1.2, 00:00:15, Serial2/0
R    192.168.6.0/24 [120/2] via 192.100.1.2, 00:00:15, Serial2/0
     192.100.1.0/30 is directly connected, Serial2/0
     192.100.1.4/30 is directly connected, Serial3/0
R    192.100.1.8/30 [120/1] via 192.100.1.2, 00:00:15, Serial2/0
```

### Expected Routing Table - RouterB

```cisco
RouterB# show ip route

Gateway of last resort is not set

R    192.168.1.0/24 [120/1] via 192.100.1.1, 00:00:23, Serial2/0
R    192.168.2.0/24 [120/1] via 192.100.1.1, 00:00:23, Serial2/0
     192.168.3.0/24 is directly connected, FastEthernet0/0
     192.168.4.0/24 is directly connected, FastEthernet1/0
R    192.168.5.0/24 [120/1] via 192.100.1.10, 00:00:18, Serial3/0
R    192.168.6.0/24 [120/1] via 192.100.1.10, 00:00:18, Serial3/0
     192.100.1.0/30 is directly connected, Serial2/0
     192.100.1.4/30 is directly connected, Serial6/0
     192.100.1.8/30 is directly connected, Serial3/0
```

### Expected Routing Table - RouterC

```cisco
RouterC# show ip route

Gateway of last resort is not set

R    192.168.1.0/24 [120/2] via 192.100.1.9, 00:00:11, Serial3/0
R    192.168.2.0/24 [120/2] via 192.100.1.9, 00:00:11, Serial3/0
R    192.168.3.0/24 [120/1] via 192.100.1.9, 00:00:11, Serial3/0
R    192.168.4.0/24 [120/1] via 192.100.1.9, 00:00:11, Serial3/0
     192.168.5.0/24 is directly connected, FastEthernet0/0
     192.168.6.0/24 is directly connected, FastEthernet1/0
R    192.100.1.0/30 [120/1] via 192.100.1.9, 00:00:11, Serial3/0
R    192.100.1.4/30 [120/1] via 192.100.1.9, 00:00:11, Serial3/0
     192.100.1.8/30 is directly connected, Serial3/0
```

**Legend:**
- `R` - RIP learned route
- `[120/1]` - [Administrative Distance/Metric (Hop Count)]
- Directly connected - Interface on local router

---

## Testing and Verification

### RIP Verification Commands

```cisco
! View RIP routing protocol status
show ip protocols

! View routing table
show ip route
show ip route rip

! View RIP database
show ip rip database

! Debug RIP (use carefully)
debug ip rip
debug ip rip events

! View interface status
show ip interface brief

! Check RIP timers
show ip protocols | include Timer
```

### Sample: Show IP Protocols Output

```cisco
RouterA# show ip protocols

Routing Protocol is "rip"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Sending updates every 30 seconds, next due in 22 seconds
  Invalid after 180 seconds, hold down 180, flushed after 240
  Redistributing: rip
  Default version control: send version 1, receive version 1
    Interface             Send  Recv  Triggered RIP  Key-chain
    FastEthernet0/0       1     1                                   
    Serial2/0             1     1                                   
  Automatic network summarization is in effect
  Maximum path: 4
  Routing for Networks:
    192.168.1.0
    192.168.2.0
    192.168.3.0
    192.100.1.0
  Passive Interface(s):
    FastEthernet0/0
    FastEthernet1/0
  Routing Information Sources:
    Gateway         Distance      Last Update
    192.100.1.2     120          00:00:12
  Distance: (default is 120)
```

---

## Connectivity Testing

### From NetA (192.168.1.2)

```bash
ping 192.168.1.1     # ✅ Local Gateway
ping 192.168.2.2     # ✅ NetB (1 hop)
ping 192.168.3.2     # ✅ NetC (1 hop)
ping 192.168.4.2     # ✅ NetD (2 hops via RouterB)
ping 192.168.5.2     # ✅ NetE (3 hops via RouterB, RouterC)
ping 192.168.6.2     # ✅ NetD (3 hops via RouterB, RouterC)

tracert 192.168.6.2  
# Path: .1.1 → .1.2 → .9 → .6.1 → .6.2
```

### From NetE (192.168.5.2)

```bash
ping 192.168.5.1     # ✅ Local Gateway
ping 192.168.6.2     # ✅ NetD (1 hop - same router)
ping 192.168.4.2     # ✅ NetD (2 hops via RouterB)
ping 192.168.3.2     # ✅ NetC (2 hops via RouterB)
ping 192.168.1.2     # ✅ NetA (3 hops via RouterB, RouterA)
ping 192.168.2.2     # ✅ NetB (3 hops via RouterB, RouterA)
```

### Connectivity Matrix

| From → To | NetA | NetB | NetC | NetD | NetE | NetD(R3) | Hops |
|-----------|------|------|------|------|------|----------|------|
| NetA | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 0-3 |
| NetB | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 0-3 |
| NetC | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 0-3 |
| NetD | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 0-3 |
| NetE | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 0-3 |
| NetD(R3) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 0-3 |

---

## RIP Convergence and Timers

### RIP Timers Explained

| Timer | Default | Purpose |
|-------|---------|---------|
| **Update Timer** | 30 seconds | How often RIP sends updates |
| **Invalid Timer** | 180 seconds | Time before route marked invalid |
| **Holddown Timer** | 180 seconds | Time to suppress route updates |
| **Flush Timer** | 240 seconds | Time before route removed from table |

### Convergence Process

1. **Initial State** - Routers exchange routing tables
2. **30-second Updates** - Continuous routing information exchange
3. **Route Calculation** - Best path based on hop count
4. **Convergence** - All routers have consistent routing tables
5. **Periodic Updates** - Maintain routing information

**Typical Convergence Time:** 
- Small network: 30-90 seconds
- This topology: ~60-90 seconds

---

## Troubleshooting Guide

### Common RIP Issues

| Issue | Symptoms | Cause | Solution |
|-------|----------|-------|----------|
| Routes not appearing | Missing networks in table | Wrong network statement | Verify `network` command |
| Routing loops | Packets bouncing | Misconfiguration | Check RIP version consistency |
| Slow convergence | Delays after changes | Normal RIP behavior | Consider upgrading to RIP v2 or OSPF |
| No RIP neighbor | No routes learned | Interface mismatch | Check `show ip protocols` |
| Classful summarization | Wrong routing | RIP v1 limitation | Ensure all subnets use same mask |

### Diagnostic Commands

```cisco
! Check RIP configuration
show running-config | section router rip
show ip protocols

! View routing table
show ip route
show ip route rip

! Check RIP database
show ip rip database

! Monitor RIP updates (CAREFUL - generates lots of output)
debug ip rip
debug ip rip events

! Turn off debugging
undebug all
no debug all

! Check interface status
show ip interface brief
show interfaces

! Verify network statements
show ip protocols | include Routing for Networks
```

### Step-by-Step Troubleshooting

**Step 1: Verify RIP is running**
```cisco
show ip protocols
# Should show "Routing Protocol is rip"
```

**Step 2: Check network statements**
```cisco
show running-config | section router rip
# Verify all networks are listed
```

**Step 3: Verify interfaces are up**
```cisco
show ip interface brief
# All interfaces should show "up/up"
```

**Step 4: Check for RIP routes**
```cisco
show ip route rip
# Should see routes marked with "R"
```

**Step 5: Monitor RIP updates**
```cisco
debug ip rip
# Watch for incoming/outgoing updates
# Use "undebug all" when done!
```

---

## RIP vs Static Routing Comparison

| Feature | RIP v1 | Static Routing |
|---------|--------|----------------|
| Configuration | Automatic route discovery | Manual route entry |
| Scalability | Good for small networks | Poor for large networks |
| Convergence | Automatic (60-90 sec) | Immediate but manual |
| Bandwidth Usage | Periodic updates (30 sec) | No overhead |
| Administrative Distance | 120 | 1 |
| Fault Tolerance | Automatic rerouting | Manual reconfiguration |
| CPU Usage | Moderate | Minimal |
| Complexity | Low to configure | High to maintain |

---

## Network Statistics

| Metric | Count |
|--------|-------|
| Total Routers | 3 |
| Total PCs | 6 |
| LAN Networks | 6 |
| WAN Links | 3 |
| RIP Version | 1 |
| Maximum Hops | 3 |
| Update Interval | 30 seconds |
| Administrative Distance | 120 |

---

## Best Practices for RIP v1

✅ **Use passive-interface** on LAN interfaces  
✅ **Consistent subnet masks** across network  
✅ **Limit network size** to max 15 hops  
✅ **Monitor convergence** time regularly  
✅ **Document all networks** in RIP process  
⚠️ **Consider upgrading** to RIP v2 for better features  
⚠️ **Not suitable** for large enterprise networks  

---

## Upgrade Path: RIP v1 to RIP v2

### Benefits of RIP v2

- ✅ Supports VLSM and CIDR
- ✅ Multicast updates (224.0.0.9) instead of broadcast
- ✅ Authentication support (MD5)
- ✅ Route tags for external routes
- ✅ Next-hop information

### Simple Upgrade Command

```cisco
router rip
 version 2
 no auto-summary
```

**Note:** All routers must be upgraded simultaneously for RIP v2

---

## Configuration Backup

### RIP Configuration Summary

**RouterA:**
```
router rip
 version 1
 network 192.168.1.0
 network 192.168.2.0
 network 192.168.3.0
 network 192.100.1.0
 passive-interface Fa0/0
 passive-interface Fa0/1
 passive-interface Fa1/0
```

**RouterB:**
```
router rip
 version 1
 network 192.168.3.0
 network 192.168.4.0
 network 192.100.1.0
 passive-interface Fa0/0
 passive-interface Fa1/0
```

**RouterC:**
```
router rip
 version 1
 network 192.168.5.0
 network 192.168.6.0
 network 192.100.1.0
 passive-interface Fa0/0
 passive-interface Fa1/0
```

---

## Network Checklist

- [x] All router interfaces configured
- [x] RIP routing protocol enabled on all routers
- [x] Clock rates set on DCE interfaces
- [x] All network statements configured in RIP
- [x] Passive interfaces configured on LAN ports
- [x] All PCs have correct IP and gateway
- [x] End-to-end connectivity verified
- [x] RIP routing tables verified
- [ ] Configuration saved on all routers (copy run start)
- [ ] RIP convergence tested
- [ ] Documentation updated
- [ ] Network tested under load
- [ ] Backup configuration files created

---

**Document Version:** 1.0  
**Network Type:** Dynamic Routing  
**Total Networks:** 6 LAN + 3 WAN  
**Routing Protocol:** RIP Version 1  
**Administrative Distance:** 120  
**Update Timer:** 30 seconds  
**Maximum Hops:** 15  
**Status:** ✅ Operational  
**Last Updated:** December 2024