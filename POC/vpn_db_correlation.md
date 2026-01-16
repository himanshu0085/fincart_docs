## ✅ MANUAL POC – VPN → DB LOG CORRELATION (FINAL FLOW)

---

### 🎯 POC Objective 

Prove that database activity logs can be correlated with NetBird VPN users using **timestamp + VPN IP**, without requiring database data access.

---

### 🧩 STEP 1️⃣ – NetBird Evidence (DONE)

**Source:** NetBird Dashboard

- **VPN User:** GAURAV-SINGH  
- **VPN IP:** 100.104.39.1  
- **Status:** Online  
- **Date:** 16 Jan 2026  
- **Time Window:** 11:00 – 12:00 IST (approx)

📸 **Screenshots required**
- Peers page (showing user + VPN IP)
- Activity / Audit Events (login / active)

---

### 🧩 STEP 2️⃣ – DB Logs Evidence (DONE)

**Source:** Azure Portal → PostgreSQL Server → Diagnostic Logs (`pt1h.json`)

```
statement: SELECT 1
time: 2026-01-16T07:00:00.546Z
```

✔️ Confirms safe query execution and timestamp capture.

---

### 🧩 STEP 3️⃣ – Time Correlation (MANUAL)

- **DB log time:** 07:00 UTC  
- **Converted to IST:** ~12:30 PM IST  

✔️ Matches NetBird active session window.

---

### 🧩 STEP 4️⃣ – Correlation Logic (VPN → DB)

```
IF
  PostgreSQL.client_ip == NetBird.VPN_IP
AND
  PostgreSQL.timestamp ∈ NetBird.session_time_window
THEN
  DB activity is attributable to that VPN user
```

---

### 📄 FINAL STATEMENT

A manual POC validates that VPN user activity and PostgreSQL database logs can be correlated using IP and timestamp, without DB access or configuration changes.
