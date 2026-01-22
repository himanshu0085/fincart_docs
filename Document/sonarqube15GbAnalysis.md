### 🔍 SonarQube PostgreSQL (15 GB) – Analysis Summary

We analyzed the SonarQube server storage, specifically the ~15 GB PostgreSQL data, to understand what it contains and the associated risks.

---

### 📌 Key Findings
- **Database name:** `ddsonarqube`
- PostgreSQL size (~15 GB) is **not logs** and **not memory usage**
- It contains **SonarQube internal analysis data**, including:
  - Code metrics & measures
  - Issues (bugs, vulnerabilities)
  - Issue change history
  - Source code snapshots
- Data retention verified using **read-only queries**:
  - **Oldest analysis:** 16 Apr 2024  
  - **Latest analysis:** 22 Jan 2026  
  → Approx **21 months of accumulated analysis history**

---

### 📊 Major Space-Consuming Tables
- `live_measures` – ~7.3 GB (latest metrics per file/project)
- `file_sources` – ~3.0 GB (source code snapshots)
- `issue_changes` – ~2.6 GB (issue history)
- `issues`, `components`, `project_measures` – remaining data

➡️ This confirms the database is dominated by **analysis history**, not logs.

---

### ⚠️ Risk Assessment (PostgreSQL – 15 GB)
- **Risk level:** HIGH *if touched incorrectly*
- Manual deletion or truncation at DB level can:
  - Corrupt SonarQube
  - Cause permanent loss of analysis history and issues
- Therefore:
  - ❌ No manual DB cleanup
  - ❌ No file or table deletion

---

### ✅ What Is Safe
- Read-only queries
- Planned PostgreSQL maintenance (e.g. `VACUUM` during low-usage window)
- SonarQube-level housekeeping

---

### 🛠 Retention – Can We Control DB Growth?
- ✔️ Yes, **via SonarQube housekeeping**, not directly in PostgreSQL
- SonarQube supports:
  - Limiting analysis history (e.g. last 6–12 months)
  - Cleaning old background task metadata
- Retention controls **future growth**
- `VACUUM` helps reclaim space after housekeeping

---

### 🧠 Bottom Line
- 15 GB PostgreSQL usage is **expected behavior** for ~21 months of SonarQube analysis
- Root issue is **undersized disk (29 GB) and low RAM (4 GB)**, not database misuse
- Correct approach:
  - Keep DB intact
  - Enable SonarQube retention
  - Perform safe maintenance
  - Plan capacity upgrade for long-term stability
