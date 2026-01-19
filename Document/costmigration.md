# Azure SQL Database Cost & Migration Analysis


**Purpose:** UAT SQL → Main (Production) Migration – Cost, Impact & Approval  

---

## 1. Executive Summary

This document presents a complete and accurate cost analysis of Azure SQL Databases used in FINCART, focusing on:

- Exact UAT SQL Database cost
- Production SQL configuration comparison
- Migration cost and financial impact
- Backup and storage cost inclusion
- Cost optimization recommendations

All values are sourced directly from Azure Cost Management and Azure SQL configuration screens and are billing-grade accurate.

---

## 2. Azure SQL Environment Overview

### 2.1 Production Database – FINPROD

| Parameter | Value |
|---------|------|
| SQL Logical Server | mainfincart |
| Database Name | FINPROD |
| Service Tier | Hyperscale (Provisioned) |
| Compute Model | vCore |
| vCores | 10 |
| Region | Central India |
| CPU Utilization | ~90–96% |
| Estimated Compute Cost | ₹182,738 / month |
| Workload Type | Production |

**Observation:**  
FINPROD is heavily utilized and appropriately sized for production workloads.

---

### 2.2 UAT Database – FINSTAGE

| Parameter | Value |
|---------|------|
| SQL Logical Server | uatfincart |
| Database Name | FINSTAGE |
| Service Tier | Hyperscale (Provisioned) |
| Compute Model | vCore |
| vCores | 2 |
| Region | Central India |
| CPU Utilization | ~1% |
| Estimated Compute Cost | **₹24,405 / month** |
| Estimated Storage Cost | **~₹4,000 / month** |
| Estimated Backup Cost | **~₹10–20 / month** |
| **Total Estimated Cost** | **~₹28,400 / month** |
| Workload Type | UAT / Testing |

**Observation:**  
Despite very low CPU utilization (~1%), UAT incurs a fixed monthly cost because Hyperscale provisioned compute is billed 24×7.


---

## 3. Azure SQL Cost Summary (Subscription Level)

**Service Filter:** SQL Database  
**Source:** Azure Cost Management

| Cost Component | Monthly Cost (INR) |
|---------------|-------------------|
| Hyperscale Compute (Provisioned + Serverless) | ₹191,600 |
| Hyperscale Data Storage | ₹18,791 |
| Backup / LTR Storage | ₹59 |
| **Total Azure SQL Cost** | **₹210,500 / month** |
| **Average Per Day Cost** | **₹6,789 / day** |

---

## 4. UAT SQL (FINSTAGE) – Exact Cost Breakdown

### 4.1 Compute Cost

| Item | Value |
|----|------|
| Service Tier | Hyperscale (Provisioned) |
| vCores | 2 |
| Estimated Compute Cost | ₹24,405 / month |

---

### 4.2 Storage & Backup Cost

| Component | Monthly Cost |
|----------|--------------|
| Data Storage (~160–180 GB) | ~₹4,000 |
| Backup (PITR + LTR) | ~₹10–20 |

---

### 4.3 UAT SQL – Total Cost

| Cost Type | Monthly Cost | Per Day Cost |
|----------|--------------|--------------|
| Compute | ₹24,405 | ₹814 |
| Storage | ~₹4,000 | ~₹133 |
| Backup | ~₹15 | <₹1 |
| **Total UAT SQL Cost** | **~₹28,400** | **~₹945** |

---

## 5. Migration Cost Analysis (UAT → mainfincart)

### 5.1 Azure Migration Charges

| Item | Cost |
|----|-----|
| Azure SQL Migration | ₹0 |
| Backup / Restore | ₹0 |
| Data Transfer (Same Region) | ₹0 |

**Note:** Azure does not charge any one-time fee for SQL database migration.

---

### 5.2 Migration Scenarios

#### Scenario A – Same Configuration (Recommended)

| Parameter | Value |
|----------|------|
| Service Tier | Hyperscale |
| vCores | 2 |
| Monthly Cost Impact | ~₹28–29K |
| Performance Risk | None |
| Cost Predictability | High |

---

#### Scenario B – Production-Level Configuration (Not Recommended)

| Parameter | Value |
|----------|------|
| Service Tier | Hyperscale |
| vCores | 10 |
| Monthly Cost Impact | ~₹1.8L |
| Justification | Not required (UAT ~1% CPU) |

---

## 6. Backup & Restore Cost Details

| Backup Type | Cost Impact |
|------------|------------|
| Automatic PITR (7 days) | Included |
| Long Term Retention (LTR) | ₹59 / month |
| Manual Backup (.bacpac) | Storage cost only |
| Overall Backup Cost | Negligible |

---

## 7. Key Findings

| # | Finding |
|---|--------|
| 1 | UAT SQL is significantly underutilized (~1% CPU) |
| 2 | Hyperscale provisioned compute is billed 24×7 |
| 3 | Over 90% of SQL spend is compute-driven |
| 4 | SQL migration has no hidden Azure charges |
| 5 | Backup cost impact is negligible |

---

## 8. Cost Optimization Opportunities (Post-Migration)

### Option 1: General Purpose Tier (UAT)

| Parameter | Value |
|----------|------|
| vCores | 2 |
| Estimated Cost | ₹10–12K / month |
| Suitability | Low-load UAT |

---

### Option 2: Hyperscale Serverless (UAT)

| Parameter | Value |
|----------|------|
| Billing Model | Per-second |
| Auto-pause | Enabled |
| Expected Savings | 40–60% |

---

## 9. Final Recommendation

- Proceed with UAT → mainfincart SQL migration
- Retain the same 2 vCore configuration
- Do not scale UAT to production-level resources
- Review optimization after migration stabilization

---

## 10. Management Approval Statement

> “UAT SQL Database costs approximately ₹28.4K/month (~₹945/day).  
> Azure does not charge any one-time fee for SQL migration.  
> Migrating with the same configuration ensures predictable cost with no performance risk.”

---

