# RID Brute Force Detection

## Technique
T1110 - Brute Force / User Enumeration

---

## Behavioral Pattern

RID brute force attacks generate repeated authentication or enumeration attempts
against sequential RID values in Active Directory.

---

## Detection Logic

### Indicators

- High number of authentication attempts in short time window
- Sequential RID queries (500, 501, 502...)
- Repeated access to LSAD / SAMR interfaces
- Elevated failure rate (Event ID 4625 spikes)

---

## SIEM Detection Rule (Conceptual)

IF:
- Authentication failures > threshold (e.g. 20 in 1 min)
OR
- Sequential RID enumeration detected
THEN:
- Trigger alert: "Possible RID Brute Force Attack"

---

## MITRE Mapping

- T1110.001 - Password Guessing
- T1087 - Account Discovery