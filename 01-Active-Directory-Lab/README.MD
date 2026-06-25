# 🏢 Active Directory Security Lab

## 🎯 Objective

This lab simulates a real enterprise Active Directory environment built from scratch
using VMware to understand how identity systems operate, how they are attacked, and
how they can be detected, monitored, and hardened in practice.

The project follows a full attack lifecycle:

- Infrastructure deployment & AD configuration
- Attack simulation (SMB, LDAP, RID brute force, credential access)
- Attack path analysis (BloodHound CE)
- Detection engineering (SOC modeling & MITRE ATT&CK mapping)

---

## 🧱 Lab Architecture

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
  │192.168.10.10│   │192.168.10.20│  │192.168.10.15 │  │192.168.10.30 │
  └─────────────┘   └─────────────┘  └──────────────┘  └──────────────┘
```

---

## 🧠 Skills Demonstrated

| Category | Skills |
|----------|--------|
| Infrastructure | VMware lab design, Windows Server 2022 setup |
| Identity & Access | Active Directory, DNS, GPO, OU design, privilege delegation |
| Offensive Security | SMB/LDAP enumeration, NetExec, BloodHound, credential access |
| Attack Simulation | RID brute force, account enumeration, ACL abuse |
| Blue Team / SOC | Detection engineering, MITRE ATT&CK mapping, event log analysis |
| Threat Detection | Behavioral detection logic, SOC correlation modeling |

---

## 📂 Project Phases

| # | Phase | Status |
|---|-------|:------:|
| 01 | [🔧 Environment Setup](./01-Environment-Setup/) | ✅ Complete |
| 02 | [🏗️ Active Directory Deployment](./02-Active-Directory-Deployment/) | ✅ Complete |
| 03 | [🌐 DNS Configuration & Validation](./03-Domain-Configuration/) | ✅ Complete |
| 04 | [👥 Identity Management](./04-Identity-Management/) | ✅ Complete |
| 05 | [💻 Client Integration](./05-Client-Integration/) | ✅ Complete |
| 06 | [🔐 Security Hardening](./06-Security-Hardening/) | ✅ Complete |
| 07 | [⚔️ Attack Simulation](./07-Attack-Simulation/) | ✅ Complete |
| 08 | [🩸 BloodHound & Attack Path Analysis](./08-BloodHound-Analysis/) | ✅ Complete |
| 09 | [🛡️ Detection Engineering](./09-Detection-Engineering/) | ✅ Complete |
| 10 | [📖 Lessons Learned](./10-Lessons-Learned/) | ✅ Complete |

---

## ⚔️ Attack Coverage

| Attack | Technique | MITRE ATT&CK |
|--------|-----------|:------------:|
| SMB Enumeration | Network service & share discovery | T1046 |
| LDAP Enumeration | Active Directory object discovery | T1087.002 |
| RID Brute Force | Sequential account enumeration via SAMR | T1110 |
| BloodHound Recon | Graph-based AD attack path analysis | T1069, T1087 |
| Kerberoasting | SPN-based service ticket extraction | T1558.003 |
| AS-REP Roasting | Accounts without pre-authentication | T1558.004 |
| Pass-the-Hash | NTLM hash reuse for lateral movement | T1550.002 |

---

## 🔎 Detection Reference

| Event ID | Description | Attack Relevance |
|----------|-------------|-----------------|
| 4625 | Failed logon attempt | Brute force, RID cycling, password spray |
| 4768 | Kerberos TGT request | Authentication baseline, AS-REP Roasting |
| 4769 | Kerberos service ticket request | Kerberoasting detection |
| 4771 | Kerberos pre-auth failure | Password spray, brute force |
| 4672 | Special privileges assigned | Privilege escalation, admin logons |
| 4728 | Member added to security group | Persistence, group membership abuse |
| 4662 | AD object access | BloodHound-style ACL enumeration |
| 1644 | LDAP query (Directory Services) | Bulk LDAP enumeration detection |
| 5140 | Network share accessed | SMB enumeration, lateral movement prep |

---

## 🧠 Key Learnings

- ACL misconfigurations are the most critical AD risk - a standard user
  with `WriteDACL` on Domain Admins means full domain compromise
- Tools are interchangeable; **behavior is what matters** in detection
- BloodHound surfaces attack paths invisible to standard AD tools
- LDAP + SMB enumeration is consistently the precursor to lateral movement
- Detection requires correlation across multiple event sources - single-event
  alerts generate noise without context
- Most attack surface is visible in Windows event logs - the challenge is
  correlation, not data availability

---

## 📚 References

- [Microsoft AD DS Documentation](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/)
- [MITRE ATT&CK - Enterprise Matrix](https://attack.mitre.org/matrices/enterprise/)
- [BloodHound CE Documentation](https://bloodhound.specterops.io/)
- [SpecterOps - Kerberoasting Revisited](https://specterops.io/blog/2019/02/20/kerberoasting-revisited/)