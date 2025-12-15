# Network Documentation - Ruang A & B

## Network Overview

| Ruang | Network | CIDR | Subnet Mask | Gateway | Usable IPs |
|-------|---------|------|-------------|---------|------------|
| Ruang B | 192.168.10.0 | /28 | 255.255.255.240 | 192.168.10.1 | 14 |
| Ruang A | 192.168.10.16 | /29 | 255.255.255.248 | 192.168.10.17 | 6 |

---

## Complete IP Address Table

| Ruang | Unit | IP Address | Subnet Mask | Default Gateway | Device Type |
|-------|------|------------|-------------|-----------------|-------------|
| Ruang B | PRINTER | 192.168.10.2 | 255.255.255.240 | 192.168.10.1 | Printer |
| Ruang B | PC-0 | 192.168.10.3 | 255.255.255.240 | 192.168.10.1 | Workstation |
| Ruang B | PC-1 | 192.168.10.4 | 255.255.255.240 | 192.168.10.1 | Workstation |
| Ruang B | PC-2 | 192.168.10.5 | 255.255.255.240 | 192.168.10.1 | Workstation |
| Ruang B | PC-3 | 192.168.10.6 | 255.255.255.240 | 192.168.10.1 | Workstation |
| Ruang B | PC-4 | 192.168.10.7 | 255.255.255.240 | 192.168.10.1 | Workstation |
| Ruang A | PC-5 | 192.168.10.18 | 255.255.255.248 | 192.168.10.17 | Workstation |
| Ruang A | PC-6 | 192.168.10.19 | 255.255.255.248 | 192.168.10.17 | Workstation |

---

## Ruang B Details (192.168.10.0/28)

### Network Information
- **Network Address:** 192.168.10.0
- **CIDR:** /28
- **Subnet Mask:** 255.255.255.240
- **Default Gateway:** 192.168.10.1
- **Broadcast Address:** 192.168.10.15
- **First Usable IP:** 192.168.10.1
- **Last Usable IP:** 192.168.10.14
- **Total Hosts:** 14

### Device List

| Device | IP Address |
|--------|------------|
| Gateway/Router | 192.168.10.1 |
| PRINTER | 192.168.10.2 |
| PC-0 | 192.168.10.3 |
| PC-1 | 192.168.10.4 |
| PC-2 | 192.168.10.5 |
| PC-3 | 192.168.10.6 |
| PC-4 | 192.168.10.7 |
| **Available** | 192.168.10.8 - 192.168.10.14 |

---

## Ruang A Details (192.168.10.16/29)

### Network Information
- **Network Address:** 192.168.10.16
- **CIDR:** /29
- **Subnet Mask:** 255.255.255.248
- **Default Gateway:** 192.168.10.17
- **Broadcast Address:** 192.168.10.23
- **First Usable IP:** 192.168.10.17
- **Last Usable IP:** 192.168.10.22
- **Total Hosts:** 6

### Device List

| Device | IP Address |
|--------|------------|
| Gateway/Router | 192.168.10.17 |
| PC-5 | 192.168.10.18 |
| PC-6 | 192.168.10.19 |
| **Available** | 192.168.10.20 - 192.168.10.22 |

---

## Quick Configuration Guide

### Ruang B Devices
```
IP: 192.168.10.x
Subnet: 255.255.255.240
Gateway: 192.168.10.1
```

### Ruang A Devices
```
IP: 192.168.10.x
Subnet: 255.255.255.248
Gateway: 192.168.10.17
```

---

## Network Summary

| Location | Total Devices | Assigned | Available |
|----------|---------------|----------|-----------|
| Ruang B | 14 hosts | 6 devices | 7 IPs free |
| Ruang A | 6 hosts | 2 devices | 3 IPs free |

---

## Testing Connectivity

### From Ruang B:
```
ping 192.168.10.1     (Gateway)
ping 192.168.10.2     (Printer)
ping 192.168.10.18    (PC-5 in Ruang A)
```

### From Ruang A:
```
ping 192.168.10.17    (Gateway)
ping 192.168.10.19    (PC-6)
ping 192.168.10.3     (PC-0 in Ruang B)
```