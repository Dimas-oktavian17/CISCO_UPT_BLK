# EIGRP AS 10 Network with Telnet - Quick Guide
---
![alt text](<src/assets/Screenshot 2025-12-17 141022.png>)
---
## Network Overview

| Item | Value |
|------|-------|
| Routing Protocol | EIGRP |
| Autonomous System | 10 |
| Total Routers | 5 |
| Total PCs | 6 |
| Telnet Password | cisco123 |
| Enable Password | class |

---

## EIGRP Overview

| Feature | Value |
|---------|-------|
| AS Number | 10 |
| Protocol | Enhanced Interior Gateway Routing Protocol |
| Metric | Bandwidth, Delay, Load, Reliability |
| Admin Distance | 90 (internal), 170 (external) |
| Algorithm | DUAL (Diffusing Update Algorithm) |
| Updates | Partial, triggered |
| Multicast | 224.0.0.10 |

---

## Network Topology

```
    [PC1.2]  [PC2.2]
       |        |
    [Router4]--[Router3]--[Router2]
       |          |          |
    [Router1]--[Router5]  [PC3.2]
       |          |        [PC4.2]
    [PC5.2]   [PC6.2]
```

---

## IP Addressing Summary

### WAN Links (Serial /30)

| Link | Network | Router A | IP A | Router B | IP B |
|------|---------|----------|------|----------|------|
| 1 | 10.0.0.0/30 | Router3 | .1 | Router4 | .2 |
| 2 | 10.0.0.4/30 | Router3 | .5 | Router2 | .6 |
| 3 | 10.0.0.8/30 | Router4 | .9 | Router1 | .10 |
| 4 | 10.0.0.12/30 | Router1 | .13 | Router5 | .14 |

### LAN Networks

| Network | Gateway | PC IP | Subnet Mask | Router |
|---------|---------|-------|-------------|--------|
| 192.168.1.0/24 | 192.168.1.1 | 192.168.1.2 | 255.255.255.0 | Router4 |
| 192.168.2.0/24 | 192.168.2.1 | 192.168.2.2 | 255.255.255.0 | Router4 |
| 192.168.3.0/24 | 192.168.3.1 | 192.168.3.2 | 255.255.255.0 | Router2 |
| 192.168.4.0/24 | 192.168.4.1 | 192.168.4.2 | 255.255.255.0 | Router2 |
| 192.168.5.0/24 | 192.168.5.1 | 192.168.5.2 | 255.255.255.0 | Router1 |
| 192.168.6.0/24 | 192.168.6.1 | 192.168.6.2 | 255.255.255.0 | Router5 |

---

## Router Configuration Template

### Standard Router Config

```cisco
enable
configure terminal
hostname Router1

! Enable password
enable secret class

! Console access
line console 0
 password cisco123
 login
 logging synchronous

! Telnet access
line vty 0 4
 password cisco123
 login
 transport input telnet

! Example LAN Interface
interface FastEthernet0/0
 ip address 192.168.5.1 255.255.255.0
 no shutdown

! Example WAN Interface
interface Serial2/0
 ip address 10.0.0.10 255.255.255.252
 no shutdown

! EIGRP Configuration
router eigrp 10
 network 192.168.5.0 0.0.0.255
 network 10.0.0.8 0.0.0.3
 no auto-summary
 eigrp router-id 1.1.1.1

end
write memory
```

---

## EIGRP Configuration by Router

### Router1

```cisco
hostname Router1

interface FastEthernet0/0
 ip address 192.168.5.1 255.255.255.0
 no shutdown

interface Serial2/0
 ip address 10.0.0.10 255.255.255.252
 no shutdown

interface Serial3/0
 ip address 10.0.0.13 255.255.255.252
 clock rate 64000
 no shutdown

router eigrp 10
 network 192.168.5.0 0.0.0.255
 network 10.0.0.8 0.0.0.3
 network 10.0.0.12 0.0.0.3
 no auto-summary
 eigrp router-id 1.1.1.1
 passive-interface FastEthernet0/0

line vty 0 4
 password cisco123
 login
```

### Router2

```cisco
hostname Router2

interface FastEthernet1/0
 ip address 192.168.3.1 255.255.255.0
 no shutdown

interface FastEthernet0/0
 ip address 192.168.4.1 255.255.255.0
 no shutdown

interface Serial2/0
 ip address 10.0.0.6 255.255.255.252
 no shutdown

router eigrp 10
 network 192.168.3.0 0.0.0.255
 network 192.168.4.0 0.0.0.255
 network 10.0.0.4 0.0.0.3
 no auto-summary
 eigrp router-id 2.2.2.2
 passive-interface FastEthernet0/0
 passive-interface FastEthernet1/0

line vty 0 4
 password cisco123
 login
```

### Router3

```cisco
hostname Router3

interface Serial2/0
 ip address 10.0.0.1 255.255.255.252
 clock rate 64000
 no shutdown

interface Serial3/0
 ip address 10.0.0.5 255.255.255.252
 clock rate 64000
 no shutdown

router eigrp 10
 network 10.0.0.0 0.0.0.3
 network 10.0.0.4 0.0.0.3
 no auto-summary
 eigrp router-id 3.3.3.3

line vty 0 4
 password cisco123
 login
```

### Router4

```cisco
hostname Router4

interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface FastEthernet1/0
 ip address 192.168.2.1 255.255.255.0
 no shutdown

interface Serial2/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown

interface Serial3/0
 ip address 10.0.0.9 255.255.255.252
 clock rate 64000
 no shutdown

router eigrp 10
 network 192.168.1.0 0.0.0.255
 network 192.168.2.0 0.0.0.255
 network 10.0.0.0 0.0.0.3
 network 10.0.0.8 0.0.0.3
 no auto-summary
 eigrp router-id 4.4.4.4
 passive-interface FastEthernet0/0
 passive-interface FastEthernet1/0

line vty 0 4
 password cisco123
 login
```

### Router5

```cisco
hostname Router5

interface FastEthernet0/0
 ip address 192.168.6.1 255.255.255.0
 no shutdown

interface Serial2/0
 ip address 10.0.0.14 255.255.255.252
 no shutdown

router eigrp 10
 network 192.168.6.0 0.0.0.255
 network 10.0.0.12 0.0.0.3
 no auto-summary
 eigrp router-id 5.5.5.5
 passive-interface FastEthernet0/0

line vty 0 4
 password cisco123
 login
```

---

## EIGRP Key Commands

### Basic EIGRP Setup

```cisco
router eigrp 10
 network 192.168.1.0 0.0.0.255
 network 10.0.0.0 0.0.0.3
 no auto-summary
 eigrp router-id 1.1.1.1
 passive-interface FastEthernet0/0
```

**Wildcard Mask Examples:**
- `/24` = 0.0.0.255
- `/30` = 0.0.0.3
- `/16` = 0.0.255.255

---

## Telnet Access

### Connect to Router

```
PC> telnet 192.168.1.1
Password: cisco123

Router> enable
Password: class

Router#
```

### Telnet Credentials

| Access | Password |
|--------|----------|
| VTY (Telnet) | cisco123 |
| Enable | class |
| Console | cisco123 |

---

## Verification Commands

### EIGRP Verification

```cisco
show ip eigrp neighbors          # View EIGRP neighbors
show ip eigrp topology           # View topology table
show ip route eigrp              # Show EIGRP routes
show ip protocols                # Check EIGRP config
show ip eigrp interfaces         # EIGRP enabled interfaces
```

### Sample Output

```cisco
Router# show ip eigrp neighbors

IP-EIGRP neighbors for process 10
H   Address      Interface    Hold  Uptime   SRTT   RTO  Q  Seq
                               (sec)          (ms)       Cnt Num
0   10.0.0.13    Se3/0        14    00:15:23  40   240  0  5
1   10.0.0.2     Se2/0        12    00:15:20  28   200  0  8
```

---

## Testing Connectivity

### From PC

```bash
ping 192.168.1.1        # Local gateway
ping 192.168.6.2        # Remote PC
tracert 192.168.3.2     # Trace path
```

### From Router

```cisco
telnet 192.168.2.1      # Connect to another router
ping 10.0.0.5           # Test WAN link
show ip route           # Check routing table
```

---

## Troubleshooting

### Common Issues

| Issue | Check | Command |
|-------|-------|---------|
| No EIGRP neighbors | AS number match | `show ip eigrp neighbors` |
| Routes missing | Network statement | `show ip protocols` |
| Can't telnet | VTY password | `show running-config` |
| Wrong AS | Verify AS 10 | `show ip protocols` |

### Debug Commands

```cisco
debug eigrp packets              # Monitor EIGRP packets
debug eigrp neighbors            # Track neighbor formation
no debug all                     # Turn off debugging
```

---

## EIGRP vs RIP Comparison

| Feature | EIGRP | RIP |
|---------|-------|-----|
| Convergence | Fast (seconds) | Slow (minutes) |
| Metric | Composite | Hop count |
| Updates | Partial, triggered | Full, periodic (30s) |
| Scalability | Large networks | Small networks |
| Bandwidth | Efficient | Higher usage |
| Load Balancing | Unequal-cost | Equal-cost only |

---

## Network Summary

| Item | Count/Details |
|------|---------------|
| Routers | 5 |
| PCs | 6 |
| LAN Networks | 6 (192.168.1-6.0/24) |
| WAN Links | 4 (10.0.0.x/30) |
| AS Number | 10 |
| Routing Protocol | EIGRP |
| Telnet Password | cisco123 |
| Enable Password | class |

---

## Quick Checklist

- [ ] Configure all interfaces with IP addresses
- [ ] Set clock rates on DCE serial interfaces
- [ ] Enable EIGRP AS 10 on all routers
- [ ] Add network statements with wildcard masks
- [ ] Set router-id for each router
- [ ] Configure passive interfaces on LAN ports
- [ ] Set telnet password (cisco123)
- [ ] Set enable password (class)
- [ ] Configure all PC IPs and gateways
- [ ] Verify EIGRP neighbors formed
- [ ] Test telnet access between routers
- [ ] Check routing tables (show ip route)
- [ ] Test end-to-end connectivity
- [ ] Save configurations (write memory)

---

**Network Type:** EIGRP AS 10 with Telnet  
**Routing:** Enhanced Interior Gateway Routing Protocol  
**AS Number:** 10  
**Telnet Password:** cisco123  
**Enable Password:** class  
**Status:** ✅ Ready for Deployment  
**Last Updated:** December 2024