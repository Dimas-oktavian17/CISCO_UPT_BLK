# Multi-Network Wireless Router Documentation

## Network Overview

This documentation covers a complex multi-network topology with 5 separate wireless networks connected through a central WAN router.
---
![alt text](<src/assets/Screenshot 2025-12-16 031656.png>)
---
| Network | Network ID | Router IP | Security | SSID/Name | Color Code |
|---------|------------|-----------|----------|-----------|------------|
| NET-SLX | 192.168.100.0/24 | 192.168.100.1 | WPA | NET-SLX | Yellow |
| NET-TK | 192.168.101.0/24 | 192.168.101.0 | WPA | NET-TK-2025 | Cyan |
| NET-SHIMAN | 192.168.102.0/24 | 192.168.102.0 | WEP | NET-SHIMAN-2025 | Blue |
| NET-TKJ | 192.168.120.0/24 | 192.168.120.0 | WPA | NET-TKJ-2025 | Red/Pink |
| NET-OA | 192.168.121.0/24 | 192.168.121.0 | WEP | NET-OA-2025 | Purple |

---

## Complete Network Device Inventory

| Network | Device Name | Device Type | IP Address | Interface | Security | Status |
|---------|-------------|-------------|------------|-----------|----------|--------|
| NET-SLX | WRT300N | Wireless Router | 192.168.100.1 | 0/0 | WPA | Active |
| NET-SLX | PC-PT | Workstation | 192.168.100.3 | Wireless | WPA | Active |
| NET-SLX | PC-PT | Workstation | 192.168.100.4 | Wireless | WPA | Active |
| NET-TK | NET-TK | Wireless Router | 192.168.101.0 | 0/0, 0/1 | WPA | Active |
| NET-TK | PC-PT | Workstation | 192.168.101.101 | Wireless | WPA | Active |
| NET-TK | PC-PT | Workstation | 192.168.101.100 | Wireless | WPA | Active |
| NET-SHIMAN | NET-SHIMAN | Wireless Router | 192.168.102.0 | 0/0 | WEP | Active |
| NET-SHIMAN | PC-PT | Workstation | 192.168.102.2 | Wireless | WEP | Active |
| NET-SHIMAN | PC-PT | Workstation | 192.168.102.3 | Wireless | WEP | Active |
| NET-TKJ | NET-TKJ | Wireless Router | 192.168.120.0 | 0/0 | WPA | Active |
| NET-TKJ | PC-PT | Workstation | 192.168.120.2 | Wireless | WPA | Active |
| NET-TKJ | PC-PT | Workstation | 192.168.120.3 | Wireless | WPA | Active |
| NET-OA | NET-OA | Wireless Router | 192.168.121.0 | 0/0 | WEP | Active |
| NET-OA | PC-PT | Workstation | 192.168.121.2 | Wireless | WEP | Active |
| NET-OA | PC-PT | Workstation | 192.168.121.3 | Wireless | WEP | Active |
| WAN | WRT300N | Central Router | - | Multiple | - | Active |

---

## Network Topology Structure

```
                        [WAN ROUTER]
                        WRT300N (Central)
                              |
        +-----------+---------+---------+-----------+
        |           |         |         |           |
    [NET-SLX]   [NET-TK]  [NET-SHIMAN] [NET-TKJ]  [NET-OA]
   192.168.100  192.168.101 192.168.102 192.168.120 192.168.121
      WPA         WPA         WEP         WPA         WEP
       |           |           |           |           |
    [2 PCs]     [2 PCs]     [2 PCs]     [2 PCs]     [2 PCs]
```

---

## NET-SLX Details (192.168.100.0/24)

### Network Configuration
- **Network ID:** 192.168.100.0/24
- **Router Model:** WRT300N (Linksys)
- **Router IP:** 192.168.100.1
- **Security:** WPA
- **SSID:** NET-SLX
- **Password:** NET-SLX-2025

### Device Table

| Device | IP Address | MAC Filter | Connection Type | Status |
|--------|------------|------------|-----------------|--------|
| WRT300N Router | 192.168.100.1 | - | Wired to WAN | ✅ Active |
| PC-PT 1 | 192.168.100.3 | No | Wireless | ✅ Active |
| PC-PT 2 | 192.168.100.4 | No | Wireless | ✅ Active |

### Router Configuration

```
Hostname: WRT300N
Management IP: 192.168.100.1
Subnet Mask: 255.255.255.0
SSID: NET-SLX
Security: WPA-Personal
Password: NET-SLX-2025
DHCP: Enabled
DHCP Range: 192.168.100.2 - 192.168.100.254
```

---

## NET-TK Details (192.168.101.0/24)

### Network Configuration
- **Network ID:** 192.168.101.0/24
- **Router Model:** Wireless Router
- **Router IP:** 192.168.101.0
- **Security:** WPA
- **SSID:** NET-TK
- **Password:** NET-TK-2025
- **MAC Filter:** Enabled

### Device Table

| Device | IP Address | MAC Filter | Connection Type | Status |
|--------|------------|------------|-----------------|--------|
| NET-TK Router | 192.168.101.0 | - | Wired to WAN | ✅ Active |
| PC-PT 1 | 192.168.101.101 | Yes | Wireless | ✅ Active |
| PC-PT 2 | 192.168.101.100 | Yes | Wireless | ✅ Active |

### Router Configuration

```
Hostname: NET-TK
Management IP: 192.168.101.0
Subnet Mask: 255.255.255.0
SSID: NET-TK
Security: WPA-Personal
Password: NET-TK-2025
MAC Filter: Enabled
DHCP: Enabled
Interfaces: 0/0 (WAN), 0/1 (LAN)
```

---

## NET-SHIMAN Details (192.168.102.0/24)

### Network Configuration
- **Network ID:** 192.168.102.0/24
- **Router Model:** Wireless Router
- **Router IP:** 192.168.102.0
- **Security:** WEP (⚠️ Insecure)
- **SSID:** NET-SHIMAN
- **Password:** NET-SHIMAN-2025
- **SSID Hidden:** Yes

### Device Table

| Device | IP Address | MAC Filter | Connection Type | Status |
|--------|------------|------------|-----------------|--------|
| NET-SHIMAN Router | 192.168.102.0 | - | Wired to WAN | ✅ Active |
| PC-PT 1 | 192.168.102.2 | No | Wireless | ✅ Active |
| PC-PT 2 | 192.168.102.3 | No | Wireless | ✅ Active |

### Router Configuration

```
Hostname: NET-SHIMAN
Management IP: 192.168.102.0
Subnet Mask: 255.255.255.0
SSID: NET-SHIMAN (Hidden)
Security: WEP (⚠️ Needs Upgrade)
Password: NET-SHIMAN-2025
SSID Broadcast: Disabled
DHCP: Enabled
Interface: 0/0
```

---

## NET-TKJ Details (192.168.120.0/24)

### Network Configuration
- **Network ID:** 192.168.120.0/24
- **Router Model:** Wireless Router
- **Router IP:** 192.168.120.0
- **Security:** WPA
- **SSID:** NET-TKJ
- **Password:** NET-TKJ-2025

### Device Table

| Device | IP Address | MAC Filter | Connection Type | Status |
|--------|------------|------------|-----------------|--------|
| NET-TKJ Router | 192.168.120.0 | - | Wired to WAN | ✅ Active |
| PC-PT 1 | 192.168.120.2 | No | Wireless | ✅ Active |
| PC-PT 2 | 192.168.120.3 | No | Wireless | ✅ Active |

### Router Configuration

```
Hostname: NET-TKJ
Management IP: 192.168.120.0
Subnet Mask: 255.255.255.0
SSID: NET-TKJ
Security: WPA-Personal
Password: NET-TKJ-2025
DHCP: Enabled
Interface: 0/0
```

---

## NET-OA Details (192.168.121.0/24)

### Network Configuration
- **Network ID:** 192.168.121.0/24
- **Router Model:** Wireless Router
- **Router IP:** 192.168.121.0
- **Security:** WEP (⚠️ Insecure)
- **SSID:** NET-OA
- **Password:** NET-OA-2025
- **SSID Hidden:** Yes

### Device Table

| Device | IP Address | MAC Filter | Connection Type | Status |
|--------|------------|------------|-----------------|--------|
| NET-OA Router | 192.168.121.0 | - | Wired to WAN | ✅ Active |
| PC-PT 1 | 192.168.121.2 | No | Wireless | ✅ Active |
| PC-PT 2 | 192.168.121.3 | No | Wireless | ✅ Active |

### Router Configuration

```
Hostname: NET-OA
Management IP: 192.168.121.0
Subnet Mask: 255.255.255.0
SSID: NET-OA (Hidden)
Security: WEP (⚠️ Needs Upgrade)
Password: NET-OA-2025
SSID Broadcast: Disabled
DHCP: Enabled
Interface: 0/0
```

---

## Complete IP Address Allocation

### NET-SLX (192.168.100.0/24)

| IP Address | Device | Status |
|------------|--------|--------|
| 192.168.100.1 | Router (Gateway) | Active |
| 192.168.100.2 | Available | - |
| 192.168.100.3 | PC-PT 1 | Active |
| 192.168.100.4 | PC-PT 2 | Active |
| 192.168.100.5-254 | DHCP Pool | Available |

### NET-TK (192.168.101.0/24)

| IP Address | Device | Status |
|------------|--------|--------|
| 192.168.101.0 | Router (Gateway) | Active |
| 192.168.101.1-99 | Reserved | - |
| 192.168.101.100 | PC-PT 2 | Active |
| 192.168.101.101 | PC-PT 1 | Active |
| 192.168.101.102-254 | DHCP Pool | Available |

### NET-SHIMAN (192.168.102.0/24)

| IP Address | Device | Status |
|------------|--------|--------|
| 192.168.102.0 | Router (Gateway) | Active |
| 192.168.102.1 | Available | - |
| 192.168.102.2 | PC-PT 1 | Active |
| 192.168.102.3 | PC-PT 2 | Active |
| 192.168.102.4-254 | DHCP Pool | Available |

### NET-TKJ (192.168.120.0/24)

| IP Address | Device | Status |
|------------|--------|--------|
| 192.168.120.0 | Router (Gateway) | Active |
| 192.168.120.1 | Available | - |
| 192.168.120.2 | PC-PT 1 | Active |
| 192.168.120.3 | PC-PT 2 | Active |
| 192.168.120.4-254 | DHCP Pool | Available |

### NET-OA (192.168.121.0/24)

| IP Address | Device | Status |
|------------|--------|--------|
| 192.168.121.0 | Router (Gateway) | Active |
| 192.168.121.1 | Available | - |
| 192.168.121.2 | PC-PT 1 | Active |
| 192.168.121.3 | PC-PT 2 | Active |
| 192.168.121.4-254 | DHCP Pool | Available |

---

## Security Assessment

### Security Summary Table

| Network | Security Type | Strength | Hidden SSID | MAC Filter | Recommendation |
|---------|---------------|----------|-------------|------------|----------------|
| NET-SLX | WPA | ✅ Good | No | No | Maintain |
| NET-TK | WPA | ✅ Good | No | ✅ Yes | Excellent |
| NET-SHIMAN | WEP | ⚠️ Weak | ✅ Yes | No | **Upgrade to WPA2** |
| NET-TKJ | WPA | ✅ Good | No | No | Maintain |
| NET-OA | WEP | ⚠️ Weak | ✅ Yes | No | **Upgrade to WPA2** |

### Security Recommendations

#### Critical Issues (NET-SHIMAN & NET-OA)
🔴 **WEP Security is deprecated and easily crackable**

**Immediate Actions Required:**
1. Upgrade from WEP to WPA2-PSK or WPA3
2. Change passwords to 12+ character passphrases
3. Enable MAC address filtering
4. Implement network monitoring

#### Best Practices for All Networks
✅ **Enable MAC Filtering** (like NET-TK)  
✅ **Use WPA2 or WPA3** encryption  
✅ **Strong passwords** (minimum 12 characters)  
✅ **Regular password rotation** (every 90 days)  
✅ **Monitor connected devices** regularly  
✅ **Disable WPS** if not needed  
✅ **Update router firmware** regularly

---

## Router Access Credentials

| Network | Router IP | Default Username | Default Password | Management |
|---------|-----------|------------------|------------------|------------|
| NET-SLX | 192.168.100.1 | admin | admin | HTTP/HTTPS |
| NET-TK | 192.168.101.0 | admin | admin | HTTP/HTTPS |
| NET-SHIMAN | 192.168.102.0 | admin | admin | HTTP/HTTPS |
| NET-TKJ | 192.168.120.0 | admin | admin | HTTP/HTTPS |
| NET-OA | 192.168.121.0 | admin | admin | HTTP/HTTPS |

⚠️ **Change default credentials immediately in production environment**

---

## Network Testing Procedures

### Connectivity Test Matrix

#### NET-SLX Tests
```
From PC 192.168.100.3:
ping 192.168.100.1     (Gateway)
ping 192.168.100.4     (Other PC)
ping 8.8.8.8          (Internet)
```

#### NET-TK Tests
```
From PC 192.168.101.101:
ping 192.168.101.0     (Gateway)
ping 192.168.101.100   (Other PC)
ping 8.8.8.8          (Internet)
```

#### NET-SHIMAN Tests
```
From PC 192.168.102.2:
ping 192.168.102.0     (Gateway)
ping 192.168.102.3     (Other PC)
ping 8.8.8.8          (Internet)
```

#### NET-TKJ Tests
```
From PC 192.168.120.2:
ping 192.168.120.0     (Gateway)
ping 192.168.120.3     (Other PC)
ping 8.8.8.8          (Internet)
```

#### NET-OA Tests
```
From PC 192.168.121.2:
ping 192.168.121.0     (Gateway)
ping 192.168.121.3     (Other PC)
ping 8.8.8.8          (Internet)
```

---

## Troubleshooting Guide

### Common Issues and Solutions

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| Can't connect to WiFi | Wrong password | Verify network password |
| No internet access | Router not connected to WAN | Check WAN connection |
| Slow connection | Too many devices | Check connected devices |
| Can't see hidden SSID | SSID broadcast disabled | Manually enter SSID name |
| MAC filter blocking | MAC not in whitelist | Add MAC to filter list (NET-TK) |
| WEP connection fails | Outdated security | Upgrade to WPA2 |

### Diagnostic Commands

**Windows:**
```
ipconfig /all          (Check IP configuration)
ping [gateway]         (Test gateway connectivity)
tracert 8.8.8.8       (Trace route to internet)
netsh wlan show networks (Show available networks)
```

**Router Access:**
```
http://192.168.100.1   (NET-SLX)
http://192.168.101.0   (NET-TK)
http://192.168.102.0   (NET-SHIMAN)
http://192.168.120.0   (NET-TKJ)
http://192.168.121.0   (NET-OA)
```

---

## Network Statistics

### Overall Summary

| Metric | Count |
|--------|-------|
| Total Networks | 5 |
| Total Routers | 6 (5 edge + 1 central) |
| Total PCs | 10 |
| WPA Networks | 3 |
| WEP Networks | 2 (⚠️ Need upgrade) |
| Hidden SSIDs | 2 |
| MAC Filtering Enabled | 1 |
| Total IP Addresses Available | 1,270 |
| Total IP Addresses Used | 15 |

---

## Best Practices Implementation

### Currently Implemented ✅
- Multiple isolated networks
- DHCP enabled on all networks
- Hidden SSID on sensitive networks (SHIMAN, OA)
- MAC filtering on NET-TK
- Consistent password naming scheme

### Needs Improvement ⚠️
- Upgrade WEP to WPA2/WPA3 (NET-SHIMAN, NET-OA)
- Enable MAC filtering on all networks
- Change default router credentials
- Implement regular password rotation
- Add network monitoring
- Enable logging on all routers

---

## Quick Reference Card

### Network Quick Access

| Network | SSID | Password | Router IP | Security |
|---------|------|----------|-----------|----------|
| NET-SLX | NET-SLX | NET-SLX-2025 | 192.168.100.1 | WPA ✅ |
| NET-TK | NET-TK | NET-TK-2025 | 192.168.101.0 | WPA ✅ |
| NET-SHIMAN | NET-SHIMAN | NET-SHIMAN-2025 | 192.168.102.0 | WEP ⚠️ |
| NET-TKJ | NET-TKJ | NET-TKJ-2025 | 192.168.120.0 | WPA ✅ |
| NET-OA | NET-OA | NET-OA-2025 | 192.168.121.0 | WEP ⚠️ |

---

## Compliance Checklist

- [ ] All routers configured with unique passwords
- [ ] Default admin credentials changed
- [x] DHCP enabled on all networks
- [ ] WPA2 or better on all networks (Currently 3/5)
- [x] Network documentation completed
- [ ] MAC filtering enabled (Currently 1/5)
- [ ] Regular security audits scheduled
- [ ] Firmware updates scheduled
- [x] IP addressing documented
- [ ] Disaster recovery plan created

---

**Document Version:** 1.0  
**Total Networks:** 5  
**Security Status:** 3 Secure, 2 Need Upgrade  
**Last Updated:** December 2024  
**Next Review:** Quarterly