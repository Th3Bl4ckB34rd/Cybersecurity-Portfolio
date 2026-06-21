# 🏢 Active Directory Security Lab

## Overview

This lab simulates a small enterprise Windows environment to practice identity management,
Windows administration, attack simulation, detection engineering, and defensive hardening.
The environment is built from scratch using VMware and documents both offensive techniques
and the defensive controls used to detect them.

---

## Lab Architecture

> 📌 Network diagram coming soon.

| Role | Hostname | OS | IP Address |
|------|----------|----|------------|
| Domain Controller | DC01 | Windows Server 2022 | 192.168.10.10 |
| Windows Client | WS01 | Windows 11 | 192.168.10.20 |
| Attack Machine | KALI | Kali Linux | 192.168.10.30 |

**Hypervisor:** VMware Workstation 17 Pro — **Network:** NAT (192.168.10.0/24)

---

## Skills Demonstrated

| Category | Skills |
|----------|--------|
| Administration | Active Directory, DNS, DHCP, Group Policy (GPO) |
| Identity & Access | OU design, user/group management, privilege delegation |
| Offensive | Kerberoasting, AS-REP Roasting, BloodHound enumeration |
| Defensive | Security hardening, attack detection, event log analysis |

---

## Project Roadmap

### 🔧 Phase 1 — Infrastructure Setup
- [x] Install Windows Server 2022
- [ ] Promote to Domain Controller (AD DS)
- [ ] Configure DNS and DHCP
- [ ] Join Windows 11 client to domain

### 🏗️ Phase 2 — Active Directory Configuration
- [ ] Design and create Organizational Units (OUs)
- [ ] Create users and security groups
- [ ] Configure Group Policy Objects (GPOs)
- [ ] Configure shared resources and permissions
- [ ] Delegate administrative privileges

### ⚔️ Phase 3 — Attack Simulation
- [ ] BloodHound — AD enumeration and attack path mapping
- [ ] Kerberoasting — SPN-based service account attacks
- [ ] AS-REP Roasting — targeting accounts without pre-authentication
- [ ] Pass-the-Hash / Pass-the-Ticket

### 🛡️ Phase 4 — Detection & Hardening
- [ ] Enable and configure Windows Event Logging
- [ ] Detect attacks via Event IDs (4768, 4769, 4771, 4625, etc.)
- [ ] Apply security hardening (SMB signing, LDAP signing, tiering model)
- [ ] Document findings and recommendations

---

## Attack Coverage

| Attack | Technique | MITRE ATT&CK |
|--------|-----------|---------------|
| BloodHound Enumeration | AD graph-based attack path discovery | T1069, T1087 |
| Kerberoasting | Request TGS for SPNs, crack offline | T1558.003 |
| AS-REP Roasting | Exploit accounts without pre-auth | T1558.004 |
| Pass-the-Hash | Reuse NTLM hashes for lateral movement | T1550.002 |

---

## Detection Reference

| Event ID | Description | Relevance |
|----------|-------------|-----------|
| 4768 | Kerberos TGT request | Baseline / AS-REP Roasting |
| 4769 | Kerberos TGS request | Kerberoasting detection |
| 4771 | Kerberos pre-auth failed | Brute force / spray |
| 4625 | Failed logon | Credential attacks |
| 4672 | Special privileges assigned | Privilege escalation |
| 4728 | Member added to security group | Persistence |

---

## Screenshots

> 📸 Screenshots will be added as each phase is completed.

---

## Lessons Learned

> 📝 To be documented during and after the project.

---

## References

- [Microsoft AD DS Documentation](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
- [MITRE ATT&CK — Enterprise](https://attack.mitre.org/matrices/enterprise/)
- [BloodHound Documentation](https://bloodhound.specterops.io/get-started/introduction)
- [SpecterOps — Kerberoasting](https://specterops.io/blog/2019/02/20/kerberoasting-revisited/)