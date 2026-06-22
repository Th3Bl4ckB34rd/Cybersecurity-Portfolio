# 🌐 Phase 3 — Domain Configuration (DNS & Validation)

## 🎯 Objective

This phase validates DNS and Active Directory Domain Services (AD DS) functionality in the `corp.local` domain. The goal is to ensure proper name resolution, domain health, and client connectivity.

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

Key points:
- DNS role installed on DC01
- Forward Lookup Zone: corp.local
- DC01 uses itself as DNS (192.168.10.10)
- Active Directory integrated DNS enabled

### 📸 Evidence

![DNS Manager](Screenshots/01-DNS-Manager.png)

![IP Configuration](Screenshots/02-IPConfig-All.png)

---

## 🔍 Validation Tests

### 1. DNS Resolution (DC01)

- Query: corp.local
- Result: Resolved to 192.168.10.10

📸 Evidence:

![nslookup success](Validation/03-01-nslookup-corp-local-success.png)

---

### 2. Active Directory Health Check

- Tool: dcdiag
- Result: All tests passed successfully

📸 Evidence:

![dcdiag success](Validation/03-02-dcdiag-success.png)

---

### 3. Client DNS Resolution (WS01)

- Query: corp.local via DC01 DNS

📸 Evidence:

![WS01 nslookup](Validation/03-03-ws01-nslookup-corp-local.png)

---

### 4. Network Connectivity Test (WS01 → DC01)

- Ping to 192.168.10.10 successful

📸 Evidence:

![WS01 ping DC01](Validation/03-04-ws01-ping-dc01.png)

---

## ⚠️ Issues Encountered & Notes

### DNS Initial Delay
- nslookup showed brief timeout before resolving
- Cause: VMware Host-Only network latency

### dcdiag Warnings
- Some DNS-related warnings (wpad, SPNs)
- Expected in lab environments

### Troubleshooting Actions
- DNS flush
- Netlogon restart
- Connectivity revalidation

---

## 🧠 Key Learnings

- DNS is critical for Active Directory functionality
- SRV records enable automatic service discovery
- dcdiag is essential for validating domain health
- Virtual networks may introduce latency
- Not all warnings indicate failures

---

## 📸 Evidence Summary

- DNS Manager configuration
- IP configuration
- DNS resolution tests
- Domain health check (dcdiag)
- Client connectivity validation

---

## ✅ Conclusion

The `corp.local` domain is fully functional and correctly integrated with DNS services.

All validation tests confirm stable AD operation.

👉 Ready for Phase 4 — Identity Management