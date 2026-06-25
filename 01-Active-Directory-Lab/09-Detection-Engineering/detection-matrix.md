# Detection Engineering Matrix - Phase 9

## Objective

Map adversary behaviors observed in previous phases to
detectable security events in a SOC environment.

---

## Detection Coverage Matrix

| Attack Technique | Behavior | Detection Signal | MITRE ID |
|------------------|----------|------------------|----------|
| SMB Enumeration | NetExec scanning port 445 | Spike in SMB connections / share probing | T1046 |
| LDAP Enumeration | AD queries / BloodHound collection | LDAP query bursts / unusual binds | T1087.002 |
| RID Brute Force | Sequential RID enumeration | Authentication failures + SAMR access patterns | T1110 |
| Domain Recon | Network scanning / discovery | Port scanning detection (nmap patterns) | T1046 |

---

## SOC Detection Logic

### Key Indicators

- Multiple failed authentication attempts in short time window
- Unusual LDAP query volume from single host
- SMB share enumeration without user interaction
- Sequential RID / SID enumeration patterns

---

## Detection Strategy

1. Baseline normal AD traffic
2. Detect deviations in authentication behavior
3. Correlate SMB + LDAP + RPC activity
4. Trigger alerts on multi-signal patterns

---

## Conclusion

Detection engineering focuses on identifying attacker behaviors
rather than individual tools. The same technique can be used by
different tools, so behavior-based detection is critical.