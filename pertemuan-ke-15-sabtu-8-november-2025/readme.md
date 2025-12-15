# VLAN Network Documentation - Multi-Switch Topology

## Network Overview

This documentation covers a complex VLAN network topology with 4 VLANs distributed across multiple switches using trunk links.
---
![alt text](<src/assets/Screenshot 2025-12-16 033553.png>)
---
| VLAN ID | VLAN Name | Network | Subnet Mask | Total Devices | Color Code |
|---------|-----------|---------|-------------|---------------|------------|
| 100 | VLAN 100 | 192.168.1.0/24 | 255.255.255.0 | 8 PCs | Yellow |
| 200 | VLAN 200 | 192.168.1.0/24 | 255.255.255.0 | 7 PCs | Pink |
| 300 | VLAN 300 | 192.168.1.0/24 | 255.255.255.0 | 2 PCs | Cyan |
| - | Native/Trunk | - | - | 4 Switches | Gray |

---

## Network Topology Structure

```
         [VLAN 300]              [VLAN 100 - Top]               [VLAN 300]
           PC(.5)        PC(.1) PC(.2) PC(.6) PC(.7) PC(.10) PC(.11)    PC(.14)
              |            |      |      |      |       |       |           |
         ----SW4-----------SW1-----------SW2-----------SW3----------------SW5----
              |  Trunk     |  Trunk     |   Trunk     |   Trunk           |
              |  (Red)     |  (Black)   |   (Red)     |   (Black)         |
         [VLAN 200]    [VLAN 200]   [VLAN 100]    [VLAN 200]         [VLAN 200]
        PC(.3) PC(.4)  PC(.8)      PC(.15) PC(.16) PC(.9) PC(.12)    PC(.17) PC(.18)
                       PC(.1.8)                                       
```

---

## Complete Device Inventory

### VLAN 100 (Yellow) - 8 Devices

| Device Name | IP Address | Interface | Connected To | Switch | Port |
|-------------|------------|-----------|--------------|--------|------|
| PC-PT | 192.168.1.1 | Fa0 | Switch1 | SW1 | Fa0/1 |
| PC-PT | 192.168.1.2 | Fa0 | Switch1 | SW1 | Fa1/1 |
| PC-PT | 192.168.1.6 | Fa0 | Switch2 | SW2 | Fa0/1 |
| PC-PT | 192.168.1.7 | Fa0 | Switch2 | SW2 | Fa5/1 |
| PC-PT | 192.168.1.10 | Fa0 | Switch3 | SW3 | Fa0/1 |
| PC-PT | 192.168.1.11 | Fa0 | Switch3 | SW3 | Fa1/1 |
| PC-PT | 192.168.1.15 | Fa0 | Switch-Center | SW-C | Fa0 |
| PC-PT | 192.168.1.16 | Fa0 | Switch-Center | SW-C | Fa0 |

### VLAN 200 (Pink) - 7 Devices

| Device Name | IP Address | Interface | Connected To | Switch | Port |
|-------------|------------|-----------|--------------|--------|------|
| PC-PT | 192.168.1.3 | Fa0 | Switch4 | SW4 | Fa0 |
| PC-PT | 192.168.1.4 | Fa0 | Switch4 | SW4 | Fa0 |
| PC-PT | 192.168.1.8 | Fa0 | Switch1 | SW1 | Fa0 |
| PC-PT | 192.168.1.9 | Fa0 | Switch2 | SW2 | Fa0 |
| PC-PT | 192.168.1.12 | Fa0 | Switch3 | SW3 | Fa0 |
| PC-PT | 192.168.1.13 | Fa0 | Switch5 | SW5 | Fa0 |
| PC-PT | 192.168.1.17 | Fa0 | Switch-Center | SW-C | Fa0 |
| PC-PT | 192.168.1.18 | Fa0 | Switch-Center | SW-C | Fa0 |

### VLAN 300 (Cyan) - 2 Devices

| Device Name | IP Address | Interface | Connected To | Switch | Port |
|-------------|------------|-----------|--------------|--------|------|
| PC-PT | 192.168.1.5 | Fa0 | Switch4 | SW4 | Fa0 |
| PC-PT | 192.168.1.14 | Fa0 | Switch5 | SW5 | Fa0 |

---

## Switch Infrastructure

### Switch Connectivity Matrix

| Switch | Model | Trunk Ports | Connected Switches | VLANs Present |
|--------|-------|-------------|-------------------|---------------|
| Switch1 (SW1) | 2960-24 | Fa6/1, Fa2/1 | SW4, SW-Center | 100, 200 |
| Switch2 (SW2) | 2960-24 | Fa4/1, Fa2/1 | SW1, SW3 | 100, 200 |
| Switch3 (SW3) | 2960-24 | Fa2/1, Fa5/1 | SW2, SW5 | 100, 200 |
| Switch4 (SW4) | 2960-24 | Fa2/1 | SW1 | 200, 300 |
| Switch5 (SW5) | 2960-24 | Fa2/1 | SW3 | 200, 300 |
| Switch-Center (SW-C) | 2960-24 | Fa1/1, Fa2/1, Fa3/1 | SW1, Multiple | 100, 200 |

### Trunk Link Details

| Link | Port A | Port B | VLANs Allowed | Type | Status |
|------|--------|--------|---------------|------|--------|
| SW1 ↔ SW4 | Fa6/1 | Fa2/1 | 100, 200, 300 | Trunk | ✅ Active |
| SW1 ↔ SW2 | Fa4/1 | Fa4/1 | 100, 200 | Trunk (Red) | ✅ Active |
| SW2 ↔ SW3 | Fa2/1 | Fa2/1 | 100, 200 | Trunk (Red) | ✅ Active |
| SW3 ↔ SW5 | Fa5/1 | Fa2/1 | 100, 200, 300 | Trunk | ✅ Active |
| SW1 ↔ SW-C | Fa2/1 | Fa1/1 | 100, 200 | Trunk | ✅ Active |
| SW2 ↔ SW-C | Fa5/1 | Fa0/1 | 100, 200 | Trunk | ✅ Active |

---

## VLAN Configuration

### VLAN 100 Configuration

**Network Details:**
- **VLAN ID:** 100
- **Name:** VLAN 100
- **Network:** 192.168.1.0/24
- **Gateway:** 192.168.1.1 (recommended)
- **Devices:** 8 PCs

**IP Allocation:**
| IP Address | Device Location | Switch |
|------------|-----------------|--------|
| 192.168.1.1 | Top Row | SW1 |
| 192.168.1.2 | Top Row | SW1 |
| 192.168.1.6 | Top Row | SW2 |
| 192.168.1.7 | Top Row | SW2 |
| 192.168.1.10 | Top Row | SW3 |
| 192.168.1.11 | Top Row | SW3 |
| 192.168.1.15 | Center Bottom | SW-C |
| 192.168.1.16 | Center Bottom | SW-C |

### VLAN 200 Configuration

**Network Details:**
- **VLAN ID:** 200
- **Name:** VLAN 200
- **Network:** 192.168.1.0/24
- **Gateway:** 192.168.1.1 (recommended)
- **Devices:** 7 PCs

**IP Allocation:**
| IP Address | Device Location | Switch |
|------------|-----------------|--------|
| 192.168.1.3 | Left Bottom | SW4 |
| 192.168.1.4 | Left Bottom | SW4 |
| 192.168.1.8 | Center Left | SW1 |
| 192.168.1.9 | Center | SW2 |
| 192.168.1.12 | Center Right | SW3 |
| 192.168.1.13 | Right Bottom | SW5 |
| 192.168.1.17 | Center Bottom | SW-C |
| 192.168.1.18 | Center Bottom | SW-C |

### VLAN 300 Configuration

**Network Details:**
- **VLAN ID:** 300
- **Name:** VLAN 300
- **Network:** 192.168.1.0/24
- **Gateway:** 192.168.1.1 (recommended)
- **Devices:** 2 PCs

**IP Allocation:**
| IP Address | Device Location | Switch |
|------------|-----------------|--------|
| 192.168.1.5 | Left Edge | SW4 |
| 192.168.1.14 | Right Edge | SW5 |

---

## Switch Configuration Commands

### Switch1 (SW1) Configuration

```cisco
enable
configure terminal
hostname Switch1

! Create VLANs
vlan 100
 name VLAN_100
vlan 200
 name VLAN_200

! Access Ports - VLAN 100
interface FastEthernet0/1
 description PC 192.168.1.1
 switchport mode access
 switchport access vlan 100
 spanning-tree portfast
 no shutdown

interface FastEthernet1/1
 description PC 192.168.1.2
 switchport mode access
 switchport access vlan 100
 spanning-tree portfast
 no shutdown

! Access Ports - VLAN 200
interface FastEthernet0/0
 description PC 192.168.1.8
 switchport mode access
 switchport access vlan 200
 spanning-tree portfast
 no shutdown

! Trunk Ports
interface FastEthernet6/1
 description Trunk to Switch4
 switchport mode trunk
 switchport trunk allowed vlan 100,200,300
 no shutdown

interface FastEthernet4/1
 description Trunk to Switch2
 switchport mode trunk
 switchport trunk allowed vlan 100,200
 no shutdown

interface FastEthernet2/1
 description Trunk to Switch-Center
 switchport mode trunk
 switchport trunk allowed vlan 100,200
 no shutdown

end
write memory
```

### Switch2 (SW2) Configuration

```cisco
enable
configure terminal
hostname Switch2

! Create VLANs
vlan 100
 name VLAN_100
vlan 200
 name VLAN_200

! Access Ports - VLAN 100
interface FastEthernet0/1
 description PC 192.168.1.6
 switchport mode access
 switchport access vlan 100
 spanning-tree portfast
 no shutdown

interface FastEthernet5/1
 description PC 192.168.1.7
 switchport mode access
 switchport access vlan 100
 spanning-tree portfast
 no shutdown

! Access Ports - VLAN 200
interface FastEthernet0/0
 description PC 192.168.1.9
 switchport mode access
 switchport access vlan 200
 spanning-tree portfast
 no shutdown

! Trunk Ports
interface FastEthernet4/1
 description Trunk to Switch1
 switchport mode trunk
 switchport trunk allowed vlan 100,200
 no shutdown

interface FastEthernet2/1
 description Trunk to Switch3
 switchport mode trunk
 switchport trunk allowed vlan 100,200
 no shutdown

interface FastEthernet5/1
 description Trunk to Switch-Center
 switchport mode trunk
 switchport trunk allowed vlan 100,200
 no shutdown

end
write memory
```

### Switch3 (SW3) Configuration

```cisco
enable
configure terminal
hostname Switch3

! Create VLANs
vlan 100
 name VLAN_100
vlan 200
 name VLAN_200

! Access Ports - VLAN 100
interface FastEthernet0/1
 description PC 192.168.1.10
 switchport mode access
 switchport access vlan 100
 spanning-tree portfast
 no shutdown

interface FastEthernet1/1
 description PC 192.168.1.11
 switchport mode access
 switchport access vlan 100
 spanning-tree portfast
 no shutdown

! Access Ports - VLAN 200
interface FastEthernet0/0
 description PC 192.168.1.12
 switchport mode access
 switchport access vlan 200
 spanning-tree portfast
 no shutdown

! Trunk Ports
interface FastEthernet2/1
 description Trunk to Switch2
 switchport mode trunk
 switchport trunk allowed vlan 100,200
 no shutdown

interface FastEthernet5/1
 description Trunk to Switch5
 switchport mode trunk
 switchport trunk allowed vlan 100,200,300
 no shutdown

end
write memory
```

### Switch4 (SW4) Configuration

```cisco
enable
configure terminal
hostname Switch4

! Create VLANs
vlan 200
 name VLAN_200
vlan 300
 name VLAN_300

! Access Ports - VLAN 200
interface FastEthernet0/0
 description PC 192.168.1.3
 switchport mode access
 switchport access vlan 200
 spanning-tree portfast
 no shutdown

interface FastEthernet0/1
 description PC 192.168.1.4
 switchport mode access
 switchport access vlan 200
 spanning-tree portfast
 no shutdown

! Access Ports - VLAN 300
interface FastEthernet0/0
 description PC 192.168.1.5
 switchport mode access
 switchport access vlan 300
 spanning-tree portfast
 no shutdown

! Trunk Ports
interface FastEthernet2/1
 description Trunk to Switch1
 switchport mode trunk
 switchport trunk allowed vlan 100,200,300
 no shutdown

end
write memory
```

### Switch5 (SW5) Configuration

```cisco
enable
configure terminal
hostname Switch5

! Create VLANs
vlan 200
 name VLAN_200
vlan 300
 name VLAN_300

! Access Ports - VLAN 200
interface FastEthernet0/0
 description PC 192.168.1.13
 switchport mode access
 switchport access vlan 200
 spanning-tree portfast
 no shutdown

! Access Ports - VLAN 300
interface FastEthernet0/0
 description PC 192.168.1.14
 switchport mode access
 switchport access vlan 300
 spanning-tree portfast
 no shutdown

! Trunk Ports
interface FastEthernet2/1
 description Trunk to Switch3
 switchport mode trunk
 switchport trunk allowed vlan 100,200,300
 no shutdown

end
write memory
```

### Switch-Center (SW-C) Configuration

```cisco
enable
configure terminal
hostname Switch-Center

! Create VLANs
vlan 100
 name VLAN_100
vlan 200
 name VLAN_200

! Access Ports - VLAN 100
interface range FastEthernet0/1-2
 description PCs 192.168.1.15-16
 switchport mode access
 switchport access vlan 100
 spanning-tree portfast
 no shutdown

! Access Ports - VLAN 200
interface range FastEthernet0/3-4
 description PCs 192.168.1.17-18
 switchport mode access
 switchport access vlan 200
 spanning-tree portfast
 no shutdown

! Trunk Ports
interface FastEthernet1/1
 description Trunk to Switch1
 switchport mode trunk
 switchport trunk allowed vlan 100,200
 no shutdown

interface FastEthernet2/1
 description Trunk to Switch2
 switchport mode trunk
 switchport trunk allowed vlan 100,200
 no shutdown

interface FastEthernet3/1
 description Trunk to other switches
 switchport mode trunk
 switchport trunk allowed vlan 100,200
 no shutdown

end
write memory
```

---

## PC Configuration

### VLAN 100 PCs Configuration

**All VLAN 100 PCs:**
```
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1 (if router configured)
```

| PC IP | Configuration |
|-------|---------------|
| 192.168.1.1 | IP: 192.168.1.1, Gateway: 192.168.1.1 |
| 192.168.1.2 | IP: 192.168.1.2, Gateway: 192.168.1.1 |
| 192.168.1.6 | IP: 192.168.1.6, Gateway: 192.168.1.1 |
| 192.168.1.7 | IP: 192.168.1.7, Gateway: 192.168.1.1 |
| 192.168.1.10 | IP: 192.168.1.10, Gateway: 192.168.1.1 |
| 192.168.1.11 | IP: 192.168.1.11, Gateway: 192.168.1.1 |
| 192.168.1.15 | IP: 192.168.1.15, Gateway: 192.168.1.1 |
| 192.168.1.16 | IP: 192.168.1.16, Gateway: 192.168.1.1 |

### VLAN 200 PCs Configuration

**All VLAN 200 PCs:**
```
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1 (if router configured)
```

| PC IP | Configuration |
|-------|---------------|
| 192.168.1.3 | IP: 192.168.1.3, Gateway: 192.168.1.1 |
| 192.168.1.4 | IP: 192.168.1.4, Gateway: 192.168.1.1 |
| 192.168.1.8 | IP: 192.168.1.8, Gateway: 192.168.1.1 |
| 192.168.1.9 | IP: 192.168.1.9, Gateway: 192.168.1.1 |
| 192.168.1.12 | IP: 192.168.1.12, Gateway: 192.168.1.1 |
| 192.168.1.13 | IP: 192.168.1.13, Gateway: 192.168.1.1 |
| 192.168.1.17 | IP: 192.168.1.17, Gateway: 192.168.1.1 |
| 192.168.1.18 | IP: 192.168.1.18, Gateway: 192.168.1.1 |

### VLAN 300 PCs Configuration

**All VLAN 300 PCs:**
```
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1 (if router configured)
```

| PC IP | Configuration |
|-------|---------------|
| 192.168.1.5 | IP: 192.168.1.5, Gateway: 192.168.1.1 |
| 192.168.1.14 | IP: 192.168.1.14, Gateway: 192.168.1.1 |

---

## Testing and Verification

### VLAN Verification Commands

**On any switch:**
```cisco
show vlan brief
show interfaces trunk
show spanning-tree
show mac address-table
show interfaces status
```

### Connectivity Tests

#### VLAN 100 Tests (Same VLAN - Should Work)
```
From PC 192.168.1.1:
ping 192.168.1.2    ✅ Success
ping 192.168.1.6    ✅ Success
ping 192.168.1.7    ✅ Success
ping 192.168.1.10   ✅ Success
ping 192.168.1.11   ✅ Success
ping 192.168.1.15   ✅ Success
ping 192.168.1.16   ✅ Success
```

#### VLAN 200 Tests (Same VLAN - Should Work)
```
From PC 192.168.1.3:
ping 192.168.1.4    ✅ Success
ping 192.168.1.8    ✅ Success
ping 192.168.1.9    ✅ Success
ping 192.168.1.12   ✅ Success
ping 192.168.1.13   ✅ Success
ping 192.168.1.17   ✅ Success
ping 192.168.1.18   ✅ Success
```

#### VLAN 300 Tests (Same VLAN - Should Work)
```
From PC 192.168.1.5:
ping 192.168.1.14   ✅ Success
```

#### Cross-VLAN Tests (Should FAIL without Router)
```
From VLAN 100 PC:
ping 192.168.1.3    ❌ Failed (Different VLAN)
ping 192.168.1.5    ❌ Failed (Different VLAN)

From VLAN 200 PC:
ping 192.168.1.1    ❌ Failed (Different VLAN)
ping 192.168.1.14   ❌ Failed (Different VLAN)
```

---

## Inter-VLAN Routing (Optional)

To enable communication between VLANs, configure a router or Layer 3 switch:

### Router-on-a-Stick Configuration

```cisco
! On Router
enable
configure terminal
hostname Router-VLAN

interface GigabitEthernet0/0
 no shutdown

interface GigabitEthernet0/0.100
 description VLAN 100
 encapsulation dot1Q 100
 ip address 192.168.1.1 255.255.255.0

interface GigabitEthernet0/0.200
 description VLAN 200
 encapsulation dot1Q 200
 ip address 192.168.1.1 255.255.255.0

interface GigabitEthernet0/0.300
 description VLAN 300
 encapsulation dot1Q 300
 ip address 192.168.1.1 255.255.255.0

end
write memory
```

### Switch Connection to Router

```cisco
! On switch connected to router
interface FastEthernet0/24
 description Trunk to Router
 switchport mode trunk
 switchport trunk allowed vlan 100,200,300
 no shutdown
```

---

## Network Statistics

### Device Distribution

| VLAN | Devices | Percentage | Switches |
|------|---------|------------|----------|
| VLAN 100 | 8 | 47% | SW1, SW2, SW3, SW-C |
| VLAN 200 | 7 | 41% | SW1, SW2, SW3, SW4, SW5, SW-C |
| VLAN 300 | 2 | 12% | SW4, SW5 |
| **Total** | **17** | **100%** | **6 switches** |

### Trunk Link Summary

| Metric | Count |
|--------|-------|
| Total Switches | 6 |
| Trunk Links | 6 |
| Access Ports | 17 |
| VLANs Configured | 3 |
| Total Devices | 17 PCs |

---

## Troubleshooting Guide

### Common VLAN Issues

| Issue | Possible Cause | Solution |
|-------|----------------|----------|
| Can't ping within VLAN | Wrong VLAN assignment | Verify: `show vlan brief` |
| Trunk not working | Native VLAN mismatch | Check: `show interfaces trunk` |
| PC can't reach other VLAN | No inter-VLAN routing | Configure router or L3 switch |
| Spanning tree blocking | Redundant path | Check: `show spanning-tree` |
| Port not in correct VLAN | Wrong access VLAN | Check: `show interfaces switchport` |

### Diagnostic Commands

```cisco
! VLAN Status
show vlan brief
show vlan id 100

! Trunk Status
show interfaces trunk
show interfaces FastEthernet0/1 switchport

! Port Status
show interfaces status
show mac address-table

! Spanning Tree
show spanning-tree
show spanning-tree vlan 100

! General Troubleshooting
show running-config
show ip interface brief
```

---

## Best Practices Implemented

✅ **Proper VLAN Segmentation** - Three separate VLANs for different departments  
✅ **Trunk Configuration** - Efficient inter-switch communication  
✅ **Port Security Ready** - Can implement portfast on access ports  
✅ **Spanning Tree** - Prevents loops in redundant topology  
✅ **Consistent Naming** - Clear VLAN names and descriptions  
✅ **Documentation** - Complete configuration guide

---

## Security Recommendations

### Implement These Security Features

1. **Port Security**
```cisco
interface FastEthernet0/1
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address sticky
```

2. **Disable Unused Ports**
```cisco
interface range FastEthernet0/5-24
 shutdown
 switchport mode access
 switchport access vlan 999
```

3. **Secure Trunk Ports**
```cisco
interface FastEthernet0/24
 switchport trunk native vlan 999
 switchport nonegotiate
```

4. **Enable BPDU Guard**
```cisco
spanning-tree portfast bpduguard default
```

---

## Network Health Checklist

- [x] All VLANs created on all switches
- [x] Trunk ports configured correctly
- [x] Access ports assigned to correct VLANs
- [x] All devices have correct IP addresses
- [x] Intra-VLAN connectivity working
- [ ] Inter-VLAN routing configured (if needed)
- [ ] Port security enabled
- [ ] Unused ports disabled
- [ ] Passwords changed from defaults
- [ ] Configuration backed up

---

**Document Version:** 1.0  
**Total VLANs:** 3  
**Total Switches:** 6  
**Total Devices:** 17 PCs  
**Topology Type:** Multi-Switch with Trunking  
**Status:** ✅ Operational  
**Last Updated:** December 2024