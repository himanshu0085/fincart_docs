# Proof of Concept (POC)

## Database Activity Monitoring via VPN Users

---

## 1. Objective

The objective of this POC is to validate the **feasibility** of monitoring database activity performed by users connecting through **NetBird VPN**, in a **production-ready and audit-acceptable manner**, using Azure-native logging and correlation — **without making any configuration changes** (read-only access).

---

## 2. Scope & Constraints

### Scope

* Log Analytics Workspace analysis
* App Services (Linux)
* Database activity visibility (Azure SQL / MySQL – conceptual)
* VPN user visibility (NetBird – conceptual)

### Constraints

* **Read-only access** to Azure resources
* No ability to enable DB auditing or VPN log ingestion
* POC focused on **feasibility and gap analysis**, not implementation

---

## 3. Current Architecture Context

* Applications hosted on **Azure App Services / AKS**
* Databases (Azure SQL / MySQL) accessed via **Private Endpoints**
* Human access through **NetBird VPN**
* Centralized logging via **Log Analytics Workspace**:

  * `fincart-uat-linux-appsvc-log-analytics-ws`

---

## 4. Evidence Collected (POC Findings)

---

### 4.1 Centralized Logging – Current State

**Query Used:**

```kql
search *
| summarize LogCount = count() by Type
| order by LogCount desc
```

**Observation:**

* Logs are successfully ingested into Log Analytics
* `AppServiceConsoleLogs` dominate log volume

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d90bb8d2-3225-4340-a14c-e581d75f93bd" />


**Finding:**

> Centralized logging is operational; AppServiceConsoleLogs are the highest-volume log source.

---

### 4.2 Console Log Nature (Noise Analysis)

**Query Used:**

```kql
AppServiceConsoleLogs
| take 20
```

**Observation:**

* Logs are Informational / Debug-level
* Unstructured stdout/stderr messages

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/06538a7e-f577-4e0f-b567-413e932ca8d3" />


**Finding:**

> AppServiceConsoleLogs are high-volume, low-audit-value logs generated via stdout/stderr.

---

### 4.3 Database Audit Logs – Current Gap

**Query Used:**

```kql
search "sql"
```

**Observation:**

* No Azure SQL / MySQL audit logs present

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2dea8870-b3b7-429d-9709-4d4703154186" />

<img width="1830" height="1006" alt="image" src="https://github.com/user-attachments/assets/f15eab61-358a-49c7-8142-20a17eb83034" />



**Finding:**

> Database audit logs are not currently ingested into Log Analytics.

---

### 4.4 VPN (NetBird) Logs – Current Gap

**Query Used:**

```kql
search "netbird"
```

**Observation:**

* No VPN user identity logs available

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5113274e-6cf4-4ab2-b284-622f5fe74bbd" />

<img width="1830" height="1006" alt="image" src="https://github.com/user-attachments/assets/475f04d3-8956-4ebe-a6a0-8073ac31f0fb" />


**Finding:**

> NetBird VPN logs are not currently integrated with Log Analytics.

---

### 4.5 Cost & Volume Indicator

**Observation:**

* High ingestion driven by console logs

**📸 Screenshot to attach:**

* *Screenshot‑5:* Azure Cost Management / Ingestion volume view

**Finding:**

> Excessive console logging has a direct cost and ingestion impact.

---

### 4.2 Console Log Nature (Noise Analysis)

**Query Used:**

```kql
AppServiceConsoleLogs
| take 20
```

**Observation:**

* Logs are **Informational / Debug-level**
* Unstructured stdout/stderr messages
* Framework and application debug output

**Screenshot Reference:**

* Screenshot showing DEBUG / informational console logs

**Finding:**

> AppServiceConsoleLogs are high-volume, low-audit-value logs generated via stdout/stderr.

---

### 4.3 Database Audit Logs – Current Gap

**Query Used:**

```kql
search "sql"
```

**Observation:**

* No Azure SQL / MySQL audit logs present in workspace

**Screenshot Reference:**

* Screenshot showing only AppServiceConsoleLogs in results

**Finding:**

> Database audit logs are not currently ingested into Log Analytics.

---

### 4.4 VPN (NetBird) Logs – Current Gap

**Query Used:**

```kql
search "netbird"
```

**Observation:**

* No VPN user identity logs available in workspace

**Screenshot Reference:**

* Screenshot showing “No results found” for NetBird

**Finding:**

> NetBird VPN logs are not currently integrated with Log Analytics.

---

### 4.5 Cost & Volume Indicator

**Observation:**

* Storage and bandwidth costs visible in Azure Cost Management
* High log volume correlates with AppServiceConsoleLogs ingestion

**Screenshot Reference:**

* Cost Management screenshot highlighting storage/bandwidth usage

**Finding:**

> Excessive console logging has a direct cost and ingestion impact.

---

## 5. Correlation-Based Monitoring (POC Core Logic)

Although direct VPN username visibility at the database level is not natively possible, **correlation enables attribution**.

### Correlation Model

```
NetBird VPN Logs:
User → VPN IP → Login/Logout Time

Database Audit Logs:
Client IP → Query → Query Type → Timestamp

Correlation Keys:
- VPN IP == DB Client IP
- Timestamp overlap
```

### Resulting Attribution

| VPN User                                    | VPN IP    | DB User    | Operation | Time  |
| ------------------------------------------- | --------- | ---------- | --------- | ----- |
| [user@example.com](mailto:user@example.com) | 100.x.x.x | fincart_rw | UPDATE    | 10:12 |

**Key Point:**

> Database does not know VPN username, but both systems log IP and time — enabling deterministic correlation.

---

## 6. POC Conclusion

With read-only access, this POC demonstrates that:

* Centralized logging is functional
* AppServiceConsoleLogs are the dominant and noisy log source
* Database audit logs and VPN identity logs are currently missing
* IP + timestamp based correlation is **technically feasible**
* There are **no architectural or technical blockers**

> The proposed approach is production-ready and audit-acceptable once required logs are enabled.

---

## 7. Recommended Next Steps (Post-POC)

1. **Enable Database Auditing (UAT only)**

   * Azure SQL Auditing / MySQL audit logs
   * Send logs to existing Log Analytics workspace

2. **Integrate NetBird VPN Logs**

   * Export VPN user activity logs
   * Ingest into Log Analytics / Sentinel

3. **Correlation & Alerting**

   * KQL-based correlation queries
   * Alerts for sensitive operations (DELETE, UPDATE, DDL)

---

## 8. Access Required for Implementation

* Contributor access on **UAT database**
* Permission to configure **Diagnostic Settings**
* NetBird VPN log export approval

---

## 9. Final Statement

> This POC validates feasibility and readiness. Implementation requires only enablement of existing native logging features and does not require architectural changes.
