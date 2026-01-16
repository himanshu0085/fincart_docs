# ✅ MANUAL POC – VPN → DATABASE LOG CORRELATION

---

## 🎯 POC Objective

Prove that **PostgreSQL database activity logs** can be correlated with **NetBird VPN users** using **timestamp-based correlation**, without requiring database data access or DB client access.

---

## 🧩 Architecture Overview

### VPN Layer
- **VPN Tool:** NetBird  
- **Evidence Captured:**
  - User identity  
  - VPN session status  
  - Login / activity timestamps  

### Database Layer
- **Database:** Azure PostgreSQL Server  
- **Evidence Captured:**
  - Query execution logs  
  - UTC timestamps  
  - Query type (non-invasive)

---

## 🧩 STEP 1️⃣ – NetBird Evidence (VPN Side)

**Source**
- NetBird Dashboard → Peers  
- NetBird Dashboard → Activity → Audit Events  

**Observed**
- **VPN User:** ANKIT  
- **VPN IP:** `100.104.8.55`  
- **Status:** Online  
- **Date:** 16 Jan 2026  
- **Active Window:** ~11:00–12:30 IST  

**Proof**
- User identity visible  
- VPN session active  
- Timestamped login events  

---

## 🧩 STEP 2️⃣ – Database Logs Evidence (DB Side)

**Source**
- Azure Storage Account → `fincartpsqldblogs`
- File: `PT1H.json`

**Highlighted Log Entry**
```json
"message": "execute <unnamed>: SELECT 1",
"time": "2026-01-16T07:00:00.546Z",
"category": "PostgreSQLLogs"
```

**What This Shows**
- Query execution recorded
- Timestamp captured
- Query is safe (`SELECT 1`)
- No data access or modification

---

## 🧩 STEP 3️⃣ – Time Correlation

- **DB Log Time:** 07:00 UTC  
- **IST:** 12:30 PM IST  

✅ Timestamp aligns with VPN active window

---

## 🧩 STEP 4️⃣ – Correlation Logic (VPN → DB)

Correlation is performed **outside the database**:

```
IF
  PostgreSQL.timestamp ∈ NetBird.session_time_window
THEN
  DB activity is attributable to that VPN session
```

This safely links:
- User identity (NetBird)
- Database activity (PostgreSQL)

Without modifying DB or applications.

---

## ✅ What This POC Proves

- VPN logs provide user identity and time
- DB logs provide query activity and time
- Manual correlation is possible
- No DB access required

---

## ❌ What This POC Does NOT Claim

- No attribution of SQL user
- No data read/write
- No schema or table access

---

## 📌 Final Statement

A manual POC confirms that NetBird VPN audit logs and Azure PostgreSQL logs can be correlated using timestamps alone. This enables secure auditing of database activity without granting database access or modifying applications.

---
