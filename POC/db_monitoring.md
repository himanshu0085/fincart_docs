# Proof of Concept (POC)

## Database Activity Monitoring via VPN Users (Storage-based, No Log Analytics)

---

## 1. Objective

The objective of this POC is to **validate the feasibility** of identifying **which VPN user accessed the database and what database operations were performed** (SELECT / UPDATE / DELETE), using:

* Existing **PostgreSQL audit logs stored in Azure Storage**
* A **correlation-based approach** using IP address and timestamps

> This POC is **feasibility-focused only**. Actual implementation will be done **post client approval** and after obtaining access to NetBird VPN logs.

---

## 2. Problem Statement

Currently, database-level monitoring provides visibility into:

* Executed queries
* Query execution time

However, it does **not identify**:

* Which **human user** accessed the database through VPN
* Whether the activity was **human (VPN)** or **application-driven**

---

## 3. Scope & Constraints

### Scope

* Azure PostgreSQL Flexible Server
* Azure Storage Account: `fincartpsqldblogs`
* Storage-based diagnostic logs (no Log Analytics)

### Constraints

* No access to NetBird VPN logs as part of this POC
* No production changes
* No automation or scripts executed

---

## 4. Current Architecture Context

* Databases hosted on **Azure PostgreSQL Flexible Server**
* Human access via **NetBird VPN**
* Database diagnostic logs archived to **Azure Storage**
* Log Analytics intentionally not used due to cost considerations

---

## 5. Evidence Collected (Database Side – Real)

### 5.1 PostgreSQL Diagnostic Settings Enabled

PostgreSQL diagnostic settings are configured to archive the following logs to Azure Storage:

* PostgreSQL Server Logs
* PostgreSQL Session Logs
* PostgreSQL Query Runtime Logs

<img width="1825" height="926" alt="image" src="https://github.com/user-attachments/assets/92d4088a-da1f-408e-aae7-441d23fd0b0f" />

*PostgreSQL Diagnostic Settings showing logs archived to `fincartpsqldblogs`*

**Finding:**
Database audit logging is enabled without dependency on Log Analytics.

---

### 5.2 Database Logs Available in Azure Storage

Database audit logs are present in the storage account under structured, time-based folders.

Path structure:

```
resourceId/
 └── Microsoft.DBForPostgreSQL
     └── FlexibleServers
         └── <server-name>
             └── y/m/d/h
                 └── PT1H.json
```

<img width="719" height="365" alt="image" src="https://github.com/user-attachments/assets/f50bca7b-44a2-4962-81bb-a36ea262c0e4" />

*Storage account blob containers showing PostgreSQL log containers*

**Finding:**
Logs are continuously written to Azure Storage.

---

### 5.3 Database Activity Log File (PT1H.json)

Audit logs are stored as **hourly append blobs (PT1H.json)**.
These logs contain:

* Timestamp of activity
* Client IP address
* Database user
* Executed SQL statement / activity

<img width="1121" height="885" alt="Screenshot from 2026-01-12 16-30-38" src="https://github.com/user-attachments/assets/069745ac-08e2-436f-baf0-4574901a0f54" />

*PT1H.json opened (append blob details)*
*Preview of PT1H.json content (IP, time, query masked)*

**Finding:**
Database logs provide sufficient technical details to identify **what happened, when it happened, and from which IP**.

---

## 6. VPN User Identification – Current Gap

NetBird VPN logs are not currently ingested or accessible as part of this POC.

However, as a standard VPN behavior:

* Each authenticated VPN user is assigned a **VPN IP**
* Login and logout timestamps are recorded
* These logs are maintained by NetBird outside Azure

**Finding:**
VPN user identity exists but is currently **external to Azure**.

---

## 7. Correlation Logic (POC Core)

Since direct VPN username visibility at the database level is not natively possible, **correlation is required**.

### Correlation Rule

```
DB client IP  ==  VPN assigned IP
AND
DB activity timestamp BETWEEN VPN login and logout time
```

This rule is applied at the **analysis / reporting layer**, not within Azure Portal or the database itself.

---

## 8. Hypothetical Correlation Example (POC Demonstration)

> The following example demonstrates feasibility only.

### Database Log (Real)

* Time: 2026-01-09 15:10
* Client IP: 100.96.12.34
* Statement: SELECT

### VPN Log (Assumed – NetBird)

* VPN User: [test.user@company.com](mailto:test.user@company.com)
* VPN IP: 100.96.12.34
* Login: 15:00
* Logout: 16:00

### Correlated Output

| VPN User                                              | VPN IP       | DB Operation | Time  |
| ----------------------------------------------------- | ------------ | ------------ | ----- |
| [test.user@company.com](mailto:test.user@company.com) | 100.96.12.34 | SELECT       | 15:10 |

**Finding:**
Database activity can be attributed to a VPN user using IP and timestamp correlation.

---

## 9. POC Conclusion

This POC confirms that:

* Database audit logs are already available in Azure Storage
* Logs contain client IP, timestamp, and executed statements
* VPN systems provide user-to-IP mappings
* IP + timestamp-based correlation enables VPN user attribution
* No architectural or technical blockers exist

> Full end-to-end implementation requires client approval and access to NetBird VPN logs.

---

## 10. Next Steps (Post Approval)

1. Enable NetBird VPN log export (read-only)
2. Store VPN logs in Azure Storage
3. Automate correlation via script or Azure Function
4. Generate reports and alerts for sensitive DB operations

---

## 11. Final Statement

> This POC validates feasibility using existing infrastructure. No production changes were made, and implementation will proceed only after client approval and required access enablement.

---

