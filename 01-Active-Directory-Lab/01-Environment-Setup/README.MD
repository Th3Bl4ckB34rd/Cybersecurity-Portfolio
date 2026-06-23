# 🔧 Phase 1 — Environment Setup

## 🎯 Objective

Prepare the base infrastructure for the Active Directory lab by deploying a
Windows Server 2022 virtual machine, configuring static networking, and validating
connectivity — prerequisites before installing AD DS.

---

## 🧱 Environment

| Component | Configuration |
|-----------|---------------|
| Hypervisor | VMware Workstation Pro 17 |
| OS | Windows Server 2022 (Desktop Experience) |
| Hostname | DC01 |
| Network Adapter | VMware Host-Only |
| IP Address | 192.168.10.10 / 24 |
| Default Gateway | 192.168.10.1 |
| DNS Server | 192.168.10.10 (self) |

---

## ✅ Tasks Completed

- [x] Create Windows Server 2022 VM in VMware
- [x] Configure Host-Only network adapter
- [x] Rename server to `DC01`
- [x] Assign static IPv4 address (`192.168.10.10/24`)
- [x] Set DNS to point to itself (`192.168.10.10`) in preparation for AD DS
- [x] Validate network configuration

---

## 🔧 Configuration Notes

**Why static IP?**
Domain Controllers must have a fixed IP address. Clients and member servers use
this address for DNS resolution and Kerberos authentication — a changing IP breaks
both.

**Why self-referencing DNS?**
Setting DNS to `192.168.10.10` (itself) prepares the server for AD-integrated DNS.
After AD DS promotion, DC01 will host the `corp.local` DNS zone and resolve all
domain queries locally.

**Why Host-Only networking?**
Isolates the lab from the host's physical network, preventing unintended traffic
exposure while still allowing VM-to-VM communication.

---

## 📸 Evidence

### 1. VM Base Setup
![DC01 Base Setup](./Screenshots/01-DC01-Base-Setup.png)

### 2. Static IP & DNS Configuration
![Network Configuration](./Screenshots/02-DC01-Network-Configuration.png)

### 3. Network Validation
![Network Validation](./Screenshots/03-DC01-Network-Validation.png)

---

## 🧠 Key Learnings

- Static IPs on Domain Controllers are non-negotiable — DNS and Kerberos depend on
  address stability
- Pointing DNS to itself before AD promotion avoids post-install DNS reconfiguration
- Host-Only networking is the safest isolation mode for attack simulation labs —
  offensive tools stay contained
- Renaming the server before domain promotion avoids having to rejoin the domain
  after a rename

---

## ✅ Outcome

DC01 is deployed with a stable static IP, correct DNS configuration, and validated
network connectivity — ready for AD DS installation.

👉 **Next:** [Phase 2 — Active Directory Deployment](../02-Active-Directory-Deployment/)