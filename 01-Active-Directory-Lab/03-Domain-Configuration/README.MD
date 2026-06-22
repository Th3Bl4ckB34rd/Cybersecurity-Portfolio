# 🌐 Phase 3 — Domain Configuration (DNS & Validation)

## 🎯 Objective

This phase validates the proper configuration of DNS and Active Directory Domain Services (AD DS) in the `corp.local` domain. The goal is to ensure correct name resolution, service registration, and communication between the Domain Controller (DC01) and the client machine (WS01).

---

## 🧱 Environment Overview

- Domain Controller: DC01
- Client Machine: WS01
- Domain: corp.local
- IP Range: 192.168.10.0/24
- DC01 IP: 192.168.10.10
- Network: VMware Host-Only

---

## 🔧 DNS Configuration

DC01 is configured as the primary DNS server for the domain.

Key configuration details:
- DNS Server installed on DC01
- Forward Lookup Zone: corp.local
- DC01 configured to point to itself as DNS (192.168.10.10)
- Active Directory–integrated DNS zones enabled

📸 Evidence:
- `01-DNS-Manager`
- `02-IPConfig-All`

---

## 🔍 Validation Tests

### 1. DNS Resolution (DC01)

Domain resolution was tested using `nslookup` from the Domain Controller.

- Query: corp.local
- Result: Successfully resolved to 192.168.10.10

📸 Evidence:
- `03-01-nslookup-corp-local-success`

---

### 2. Active Directory Health Check

The `dcdiag` tool was used to validate the health of Active Directory services.

Results:
- All major tests passed
- Services, replication, and NetLogons are functional
- No critical errors detected

📸 Evidence:
- `03-02-dcdiag-success`

---

### 3. Client DNS Resolution (WS01)

From the Windows 11 client (WS01), DNS resolution was tested against DC01.

- Query: corp.local via 192.168.10.10
- Result: Successful resolution

📸 Evidence:
- `03-03-ws01-nslookup-corp-local`

---

### 4. Network Connectivity Test (WS01 → DC01)

Basic network connectivity between client and domain controller was verified.

- Ping to DC01 (192.168.10.10)
- Result: Successful replies

📸 Evidence:
- `03-04-ws01-ping-dc01`

---

## ⚠️ Issues Encountered & Notes

### 1. DNS Initial Timeout Delay
- `nslookup` occasionally showed a short timeout before resolving correctly
- Root cause: VMware Host-Only network latency and DNS retry behavior
- Impact: No functional failure, only initial delay

---

### 2. Dynamic DNS and System Warnings
- Warnings related to dynamic DNS updates and system services (wpad, SPNs, etc.)
- These are expected in lab environments and do not affect functionality

---

### 3. Netlogon and IPv6 Testing Issues (Initial Troubleshooting)
- Temporary service restart issues and command compatibility checks were performed during troubleshooting
- No permanent impact on system stability

---

## 🧠 Key Learnings

- DNS is a core dependency of Active Directory functionality
- AD automatically generates SRV records for service discovery
- dcdiag is essential to validate domain health beyond GUI tools
- VMware Host-Only networks can introduce minor DNS latency
- Not all system warnings indicate real issues in lab environments

---

## 📸 Evidence Summary

- DNS Manager configuration
- IP configuration validation
- DNS resolution from DC01
- DNS resolution from WS01
- Domain Controller health check (dcdiag)
- Network connectivity test (ping)

---

## ✅ Conclusion

The `corp.local` domain is fully functional and correctly integrated with DNS services. Both server-side and client-side validation confirm stable Active Directory operation.

This phase confirms that the infrastructure is ready for:

👉 Phase 4 — Identity Management (OUs, Users, Groups)