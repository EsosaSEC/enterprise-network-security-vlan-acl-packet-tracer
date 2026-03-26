# Enterprise Network Security Design with VLAN Segmentation, ACL Enforcement & Attacker Isolation

**Cisco Packet Tracer Simulation**  
**Author:** ESOSA OKONEDO 
**Date:** February 2026

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-2026-1E90FF?style=for-the-badge&logo=cisco)  
![Network Security](https://img.shields.io/badge/Network%20Security-VLAN%20%7C%20ACL%20%7C%20SSH-FF4500?style=for-the-badge)

## 📄 Full Project Documentation

**[📋 Download Complete Report (PDF)](https://drive.google.com/file/d/1Kn5fttQSbP123g-Bvzl2qFeQlLzSnlEo/view?usp=drive_link)**

Includes:
- Executive Summary
- Detailed Objectives
- Full CLI Configurations
- Security Validation & Testing Results
- Network Topology & Screenshots
---

## 🚀 Project Overview

This project demonstrates a complete **secure enterprise network design** for a small-to-medium enterprise (SME) using **VLAN segmentation**, **extended ACLs**, and **attacker isolation** in Cisco Packet Tracer.

The design prevents lateral movement between departments (HR ↔ Finance), restricts server access based on roles, fully isolates an Attacker VLAN, and enforces secure SSH management only from authorized subnets.

**Key Security Principles Demonstrated:**
- Network segmentation (VLANs)
- Least-privilege access control (Extended ACLs on SVIs)
- Internal threat containment (Attacker VLAN 99)
- Management plane hardening (SSHv2 + ACL)
- Proper ACL placement and verification

---

## 🏗️ Network Architecture

### VLAN Segmentation

| VLAN | Department          | Subnet             | Gateway       | Devices                          |
|------|---------------------|--------------------|---------------|----------------------------------|
| 10   | HR                  | 192.168.10.0/24    | 192.168.10.1  | 4 PCs/Laptops                    |
| 20   | Finance             | 192.168.20.0/24    | 192.168.20.1  | 4 PCs/Laptops                    |
| 30   | IT                  | 192.168.30.0/24    | 192.168.30.1  | 4 PCs/Laptops                    |
| 40   | Management          | 192.168.40.0/24    | 192.168.40.1  | 2 PCs/Laptops                    |
| 50   | Critical Servers    | 192.168.50.0/24    | 192.168.50.1  | Web, DNS, FTP Servers            |
| 99   | Attacker/Test       | 192.168.99.0/24    | 192.168.99.1  | Attacker-PC                      |

**Total devices:** 18 (14 endpoints + 3 servers + 1 attacker)

### Topology
- **Core Layer**: Cisco 3560 Multilayer Switch (`CORE-SW`) – handles inter-VLAN routing via SVIs
- **Access Layer**: Two Cisco 2960 switches (`Access-SW1` & `Access-SW2`)
- All uplinks are 802.1Q trunks

<img width="1879" height="660" alt="Screenshot 2026-02-04 114304" src="https://github.com/user-attachments/assets/811bd96a-572e-46d1-a15c-67a2f85f46e6" />

---

## 🔐 Key Security Features

### 1. Departmental ACL Enforcement (Prevent Lateral Movement)
- **HR-ACL** (VLAN 10) → Blocks all traffic to Finance VLAN 20
- **FIN-ACL** (VLAN 20) → Blocks all traffic to HR VLAN 10
- Applied **inbound** on respective SVIs (best practice)

### 2. Attacker VLAN 99 – Complete Isolation
```cisco
ip access-list extended BLOCK-ATTACKER
 deny ip 192.168.99.0 0.0.0.255 any
 deny ip any 192.168.99.0 0.0.0.255
 permit ip any any
```
Applied on VLAN 99 SVI → Attacker cannot reach any internal resource.

### 3. Secure Remote Management (SSH Only)
- SSHv2 + RSA 1024 keys
- Local authentication (ITADMIN user)
- VTY restricted with standard ACL SSH-MGMT (only IT + Management subnets allowed)
- Telnet completely disabled

### 📋Full Configuration Highlights
- Access-SW1 (HR & Finance)
- Access-SW2 (IT, Management, Servers, Attacker)
- CORE-SW (VLANs, SVIs, Routing, ACLs, SSH)

### All CLI commands are fully documented in the project files:
- configs/Access-SW1.txt
- configs/Access-SW2.txt
- configs/CORE-SW.txt

### ✅ Validation & Testing
Tested scenarios:
- HR cannot reach Finance (100% packet loss)
- Finance cannot reach HR (100% packet loss)
- Attacker VLAN 99 is completely isolated (all pings fail)
- IT & Management can SSH to CORE-SW
- All other departments blocked from SSH
- Full access to Web, DNS, and FTP servers for authorized VLANs

### 🛠️ Technologies & Tools
- Cisco Packet Tracer 2026
- Cisco Catalyst 3560 (Layer 3)
- Cisco Catalyst 2960 (Layer 2)
- VLANs, Trunking (802.1Q), SVIs
- Extended & Standard ACLs
- SSHv2 with RSA keys
- Static IP addressing

### 🎯 Skills Demonstrated
- Enterprise network segmentation
- Zero-trust style access control using ACLs
- Secure device hardening (SSHv2, ACL on VTY)
- Threat containment & isolation
- Real-world hierarchical network design (Core + Access)
