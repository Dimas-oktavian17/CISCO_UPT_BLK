# RIP v1 Network with Telnet - Quick Guide

## Network Overview
---
![alt text](<src/assets/Screenshot 2025-12-17 135429.png>)
---
**Note:** All IPs use 192.1.1.x subnet

| Item | Value |
|------|-------|
| Routing Protocol | RIP Version 1 |
| Total Routers | 7 |
| Total PCs | 9 |
| WAN Links | /30 subnets |
| Telnet Password | 123456 |
| Enable Password | cisco |

---

## Quick Network Map

```
     [Router-Top]
    /      |      \
[Router1] [Router2] [Router3]
   |         |         |
  PCs       PCs       PCs
```

---

## Router IP Addressing

### WAN Links (Serial Interfaces)

| Link | Network | Router A | IP A | Router B | IP B |
|------|---------|----------|------|----------|------|
| Link 1 | 192.1.1.0/30 | Top | .1 | Router1 | .2 |
| Link 2 | 192.1.1.4/30 | Top | .5 | Router2 | .6 |
| Link 3 | 192.1.1.8/30 | Router2 | .9 | Router3 | .10 |
| Link 4 | 192.1.1.12/30 | Top | .13 | Router4 | .14 |
| Link 5 | 192.1.1.16/30 | Router4 | .17 | Router5 | .18 |
| Link 6 | 192.1.1.20/30 | Router4 | .21 | Router6 | .22 |

---

## PC IP Addresses

| PC | IP Address | Gateway | Network |
|----|------------|---------|---------|
| PC-PT | 192.168.1.2-rip | 192.168.1.1 | LAN 1 |
| PC-PT | 192.168.2.2-rip | 192.168.2.1 | LAN 2 |
| PC-PT | 192.168.3.2-rip | 192.168.3.1 | LAN 3 |
| PC-PT | 192.168.4.2-rip | 192.168.4.1 | LAN 4 |
| PC-PT | 192.168.5.2-rip | 192.168.5.1 | LAN 5 |
| PC-PT | 192.168.6.2-rip | 192.168.6.1 | LAN 6 |
| PC-PT | 192.168.7.2-rip | 192.168.7.1 | LAN 7 |
| PC-PT | 192.168.8.2-rip | 192.168.8.1 | LAN 8 |
| PC-PT | 192.168.9.2-rip | 192.168.9.1 | LAN 9 |

---

## Router Configuration Template

### Standard Router Config (All Routers)

```cisco
enable
configure terminal
hostname Router1

! Enable password
enable secret cisco

! Console access
line console 0
 password cisco
 login
 logging synchronous

! Telnet access
line vty 0 4
 password 123456
 login
 transport input telnet

! Example LAN Interface
interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown

! Example WAN Interface
interface Serial2/0
 ip address 192.1.1.2 255.255.255.252
 clock rate 64000
 no shutdown

! RIP Configuration
router rip
 version 1
 network 192.168.1.0
 network 192.1.1.0
 passive-interface FastEthernet0/0

end
write memory
```

---

## Telnet Access Guide

### Connect to Router

**From PC:**
```
telnet 192.168.1.1
Password: 123456
Router> enable
Password: cisco
Router#
```

### Telnet Passwords

| Access Type | Password |
|-------------|----------|
| VTY (Telnet) | 123456 |
| Enable | cisco |
| Console | cisco |

---

## Quick RIP Configuration

### Basic RIP Setup

```cisco
router rip
 version 1
 network 192.168.1.0
 network 192.168.2.0
 network 192.1.1.0
 passive-interface FastEthernet0/0
 passive-interface FastEthernet1/0
```

**Key Points:**
- Use classful networks only
- Add all directly connected networks
- Set LAN interfaces as passive

---

## Verification Commands

### Quick Check

```cisco
show ip route              # View routing table
show ip protocols          # Check RIP config
show ip interface brief    # Interface status
ping [destination]         # Test connectivity
```

### Telnet Test

```cisco
telnet 192.168.2.1        # Connect to another router
show users                # See who's logged in
```

---

## Router-Specific Configs

### Top Router (Center)

```cisco
interface Serial2/0
 ip address 192.1.1.1 255.255.255.252
interface Serial3/0
 ip address 192.1.1.5 255.255.255.252
interface Serial6/0
 ip address 192.1.1.13 255.255.255.252
interface FastEthernet0/0
 ip address 192.168.4.1 255.255.255.0

router rip
 version 1
 network 192.1.1.0
 network 192.168.4.0
```

### Edge Routers (With PCs)

```cisco
interface FastEthernet0/0
 ip address 192.168.X.1 255.255.255.0
interface Serial2/0
 ip address 192.1.1.X 255.255.255.252

router rip
 version 1
 network 192.168.X.0
 network 192.1.1.0
 passive-interface FastEthernet0/0
```

---

## Network Testing

### Connectivity Test

```bash
# From any PC
ping 192.168.1.1          # Local gateway
ping 192.168.5.2          # Remote PC
tracert 192.168.9.2       # Trace route

# From router
telnet 192.168.2.1        # Access another router
show ip route rip         # Check learned routes
```

---

## Troubleshooting

| Issue | Check | Command |
|-------|-------|---------|
| Can't telnet | VTY password | `show running-config | include line vty` |
| No RIP routes | Network statement | `show ip protocols` |
| Interface down | Physical connection | `show ip interface brief` |
| Wrong password | Verify config | `show running-config` |

---

## Security Settings Summary

```cisco
! Enable password
enable secret cisco

! Telnet access
line vty 0 4
 password 123456
 login

! Console access
line console 0
 password cisco
 login
```

**Important:** Change default passwords in production!

---

## Complete Network Summary

| Item | Count/Value |
|------|-------------|
| Routers | 7 |
| PCs | 9 |
| LAN Networks | 9 (192.168.1-9.0/24) |
| WAN Links | 6 (192.1.1.x/30) |
| Routing | RIP v1 |
| Telnet Password | 123456 |
| Enable Password | cisco |
| Update Timer | 30 seconds |

---

## Quick Start Checklist

- [ ] Configure all router interfaces
- [ ] Set clock rates on DCE interfaces
- [ ] Enable RIP on all routers
- [ ] Add network statements
- [ ] Configure telnet access
- [ ] Set passwords (VTY: 123456, Enable: cisco)
- [ ] Configure passive interfaces
- [ ] Set PC IPs and gateways
- [ ] Test telnet connectivity
- [ ] Verify RIP routing tables
- [ ] Test end-to-end connectivity
- [ ] Save all configurations

---

**Network Type:** RIP Version 1 with Telnet  
**Status:** ✅ Ready for Deployment  
**Telnet Password:** 123456  
**Enable Password:** cisco  
**Last Updated:** December 2024