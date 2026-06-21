# 🏢 Active Directory Security Lab

## Overview

This lab simulates a small enterprise Windows environment to practice identity management,
Windows administration, attack simulation, detection engineering, and defensive hardening.
The environment is built from scratch using VMware and documents both offensive techniques
and the defensive controls used to detect them.

---

## 🎯 Objective

The objective of this lab is to simulate a real enterprise Active Directory environment in order to understand how identity systems are attacked and how they can be secured, monitored, and hardened in practice.

---

## Lab Architecture

> 📌 Network diagram:

```text
                         🌐 Host-Only Network
                           192.168.10.0/24
                        Domain: corp.local

         ┌──────────────────┬─────────────────┬──────────────────┐
         │                  │                 │                  │
  ┌──────┴──────┐   ┌───────┴─────┐  ┌───────┴──────┐  ┌───────┴──────┐
  │    DC01     │   │    WS01     │  │     FS01     │  │     KALI     │
  │─────────────│   │─────────────│  │──────────────│  │──────────────│
  │  AD DS      │   │  Windows 11 │  │ File Server  │  │  Kali Linux  │
  │  DNS        │   │  Client     │  │  SMB Shares  │  │  (Attacker)  │
  │─────────────│   │─────────────│  │──────────────│  │──────────────│
  │.10.10       │   │  .10.20     │  │   .10.15     │  │   .10.30     │
  └─────────────┘   └─────────────┘  └──────────────┘  └──────────────┘
```
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

## ⚔️ Attack Coverage

| Attack | Technique | MITRE ATT&CK |
|--------|-----------|---------------|
| BloodHound Enumeration | AD graph-based attack path discovery | T1069, T1087 |
| Kerberoasting | Service ticket extraction & offline cracking | T1558.003 (Kerberos Service Ticket Request) |
| AS-REP Roasting | Exploit accounts without pre-auth | T1558.004 |
| Pass-the-Hash | Reuse NTLM hashes for lateral movement | T1550.002 |

---

## 🔎 Detection Reference

| Event ID | Description | Security Relevance | Data Source |
|----------|-------------|-------------------|-------------|
| 4768 | Kerberos Authentication (TGT Request) | Baseline authentication activity / potential AS-REP Roasting indicators | Security Logs |
| 4769 | Kerberos Service Ticket (TGS Request) | Kerberoasting detection (service ticket requests for SPNs) | Security Logs |
| 4771 | Kerberos Pre-Authentication Failure | Brute force attacks / password spraying attempts | Security Logs |
| 4625 | Failed Logon Attempt | Credential attacks, brute force, and invalid authentication attempts | Security Logs |
| 4672 | Special Privileges Assigned to New Logon | Privilege escalation / administrative logons | Security Logs |
| 4728 | Member Added to Security-Enabled Global Group | Persistence / privilege escalation via group membership changes | Security Logs |

---

## 📸 Evidence & Screenshots

Each phase includes evidence of configuration, attack execution, and detection results.

---

## 🧠 Lessons Learned

This section will document:

- Key misconfigurations discovered
- Attack techniques understood
- Detection gaps identified
- Hardening improvements applied
- Real-world enterprise implications

---

## References

- [Microsoft AD DS Documentation](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
- [MITRE ATT&CK — Enterprise](https://attack.mitre.org/matrices/enterprise/)
- [BloodHound Documentation](https://bloodhound.specterops.io/get-started/introduction)
- [SpecterOps — Kerberoasting](https://specterops.io/blog/2019/02/20/kerberoasting-revisited/)