# MITRE ATT&CK Mapping - AD Attack Simulation

## Reconnaissance

| Technique | ID | Description |
|----------|----|-------------|
| Active Scanning | T1046 | Nmap service discovery on DC01 |
| Network Service Discovery | T1135 | SMB share enumeration |
| Remote System Discovery | T1018 | Host discovery (WS01 / DC01) |

---

## Credential Access / Authentication

| Technique | ID | Description |
|----------|----|-------------|
| Valid Accounts | T1078 | SMB authentication using jsmith |
| Brute Force | T1110 | RID enumeration attempts |

---

## Discovery (Active Directory)

| Technique | ID | Description |
|----------|----|-------------|
| Account Discovery | T1087 | rpcclient / netexec user enumeration |
| Group Discovery | T1069 | Domain group enumeration |
| LDAP Querying | T1087.002 | BloodHound / LDAP queries |

---

## Lateral Movement / AD Analysis

| Technique | ID | Description |
|----------|----|-------------|
| Remote Services | T1021 | SMB / RPC communication |
| Domain Trust Discovery | T1482 | AD relationship mapping via BloodHound |