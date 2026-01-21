Hello Team,

As requested, please find below the consolidated details covering **current usage metrics**, **data classification**, **cleanup plan**, and **risk assessment** for the SonarQube server. This should help in evaluating a stable, long-term solution.

---

## 1. Current Usage Metrics (Observed State)

**Server Capacity**

* Total Disk Size: **29 GB**
* Disk Utilization: **~95% (Critical)**
* Total RAM: **~4 GB**
* Available RAM: **~1.1 GB**
* Swap: **Not configured**

**Top Memory Consumers**

* SonarQube / Elasticsearch (Java processes): **~2 GB RAM**
* PostgreSQL: **~400–450 MB RAM**

> Note: PostgreSQL’s 15 GB usage refers to **disk storage**, not memory consumption.

---

## 2. Data & Storage Breakdown (What Data Exists and Where)

| Component                 | Data Type      | Path                | Size    | Retention  | Safe to Delete | Remarks                       |
| ------------------------- | -------------- | ------------------- | ------- | ---------- | -------------- | ----------------------------- |
| PostgreSQL (SonarQube DB) | Database data  | /var/lib/postgresql | ~15 GB  | N/A        | ❌ No           | Critical SonarQube dependency |
| systemd journal           | System logs    | /var/log/journal    | ~3 GB   | ~48 days   | ✅ Yes          | Old system logs               |
| System logs               | OS logs        | /var/log            | ~3.1 GB | Varies     | ⚠️ Partial     | Only rotated logs             |
| SonarQube logs            | App logs       | /opt/sonarqube/logs | ~120 MB | ~7–10 days | ✅ Yes (old)    | Keep active logs              |
| Docker data               | Images / cache | /var/lib/docker     | ~547 MB | N/A        | ⚠️ Conditional | If unused                     |

---

## 3. Detailed Cleanup Plan

### Immediate (Low Risk, Recommended)

* Reduce systemd journal log retention from ~48 days to **7 days**
* Apply a hard cap of **200 MB** on journal logs
* Remove rotated OS logs (`*.gz`, `.1`, `.2`)
* Remove dated SonarQube application logs

### Controlled / Maintenance Actions

* Run PostgreSQL **VACUUM FULL** to reclaim unused space
* Enable SonarQube housekeeping to limit analysis history

### Explicitly Excluded Actions

* No deletion of PostgreSQL data files
* No table drops or schema changes

---

## 4. Risk Assessment

| Area                 | Risk Level | Mitigation                        |
| -------------------- | ---------- | --------------------------------- |
| systemd journal logs | Low        | Safe cleanup via retention limits |
| OS rotated logs      | Low        | Only old logs removed             |
| SonarQube app logs   | Low        | Active logs retained              |
| PostgreSQL data      | High       | No deletion; maintenance only     |
| Disk saturation      | High       | Cleanup + capacity planning       |
| Low RAM (4 GB)       | High       | Upgrade recommended               |

**Overall Risk:**
The cleanup activities are **low risk** and do not impact application or database integrity. The primary long-term risks are **undersized disk and RAM** for SonarQube workloads.

---

## 5. Long-Term Optimization Recommendations

* Increase VM disk size to **50–100 GB**
* Increase RAM to **8–16 GB** (SonarQube minimum recommendation)
* Keep PostgreSQL as SonarQube dependency but monitor growth
* Enforce standard log retention (7–10 days)
* Schedule periodic PostgreSQL maintenance
* Add disk and memory usage alerts

---

### Summary

* PostgreSQL size (~15 GB) is **SonarQube internal data**, not project DB and not memory usage
* Disk pressure and low RAM are the root causes of recent instability
* Cleanup actions are safe and immediately reduce risk
* Capacity upgrade is required for long-term stability

Please let me know if further breakdowns or supporting command outputs are required.


