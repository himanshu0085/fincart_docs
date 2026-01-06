# App Service Plan Optimization Proposal

---

**Prepared For:**  
Priyanshu  

**Prepared By:**  
Himanshu Parashar  

---

## 1. Objective

The objective of this proposal is to optimize the existing Azure App Service Plans for **Fincart Production workloads** by achieving the following goals:

- Reduce monthly infrastructure cost  
- Improve memory utilization efficiency  
- Right-size App Service Plans based on actual usage  
- Logically segregate workloads for better scalability and stability  

---

## 2. Current State Overview

### 2.1 Current App Service Plans

#### ASP-FincartResourcesIndia-bff4 (Current)

| Attribute | Details |
|---------|--------|
| SKU | Premium v3 – P2v3 |
| Capacity | 4 vCPU / 16 GB |
| Avg Memory Usage | 62% |
| Monthly Cost | 19,920 INR |

**Services Hosted:**
- fincart-assets (2 GB)
- fincart-lms (2.5 GB)
- fincart-mis (1 GB)
- fincart-fp (3 GB)
- fincart-thirdparty (1.2 GB)

---

#### fincart-prod-linux-app-plan-002 (Current)

| Attribute | Details |
|---------|--------|
| SKU | Premium v3 – P2v3 |
| Capacity | 4 vCPU / 16 GB |
| Avg Memory Usage | 55% |
| Monthly Cost | 19,920 INR |

**Services Hosted:**
- fincart-ai  
- fincart-python  
- fincart-api-registry  
- fincart-portfolio  
- fincart-report  
- fincart-transaction  

---

#### fincart-prod-linux-app-plan-003 (Current)

| Attribute | Details |
|---------|--------|
| SKU | Premium v3 – P1mv3 |
| Capacity | 2 vCPU / 16 GB |
| Avg Memory Usage | 51% |
| Monthly Cost | 11,952 INR |

**Services Hosted:**
- fincart-api-gateway-ind  
- fincart-cms-ind  
- fincart-communication-ind  
- fincart-core-ind  
- fincart-dmkt-ind  

---

### 2.2 Current Cost Summary

| Metric | Amount (INR) |
|------|-------------|
| Total Monthly Cost | 51,792 |

---

## 3. Proposed Optimized Architecture

### 3.0 App Service Plan Series Selection Rationale

To support production-grade workloads while balancing cost, performance, and scalability, **Azure App Service Premium v3 series** has been deliberately chosen for all proposed plans.

#### Why Premium v3 (Pv3 / Pmv3) Series?

- **Production-grade performance** with dedicated compute resources  
- **Predictable and consistent throughput** for business-critical workloads  
- **Memory-optimized SKUs (Pmv3)** for memory-intensive applications  
- **Advanced scaling capabilities**
  - Vertical scaling (scale up/down)
  - Horizontal scaling (scale out/in)
- **Better price-to-performance** compared to older Premium tiers  
- **Enterprise-grade networking**
  - VNet integration
  - Private endpoints
- **Zone redundancy support** for higher availability (if enabled)

#### Why Not Lower Tiers (Basic / Standard)?

- Limited memory and scaling capabilities  
- Shared infrastructure risk  
- Not suitable for sustained production workloads  

#### Why Mixed Pv3 and Pmv3 SKUs?

- **Pv3 (P2v3)** used where CPU-memory balance is sufficient  
- **Pmv3 (P1mv3 / P2mv3)** used for memory-heavy workloads (APIs, processing services)  
- Prevents overpaying for CPU while maintaining memory headroom  

---

### 3.1 Proposed App Service Plans

#### ASP-FincartResourcesIndia-bff4 (Optimized)

| Attribute | Details |
|---------|--------|
| SKU | Premium v3 – P2v3 |
| Capacity | 2 vCPU / 8 GB |
| Avg Memory Usage | 46% |
| Monthly Cost | 9,960 INR |

**Key Changes:**
- Removed underutilized workloads  
- Retained only required services  

---

#### fincart-prod-linux-app-plan-001 (New)

| Attribute | Details |
|---------|--------|
| SKU | Premium v3 – P2mv3 |
| Capacity | 4 vCPU / 32 GB |
| Avg Memory Usage | 36% |
| Monthly Cost | 23,904 INR |

---

#### fincart-prod-linux-app-plan-003 (Retained)

| Attribute | Details |
|---------|--------|
| SKU | Premium v3 – P1mv3 |
| Capacity | 2 vCPU / 16 GB |
| Avg Memory Usage | 53% |
| Monthly Cost | 11,952 INR |

---

### 3.2 Current vs Proposed Changes (Detailed)

#### ASP-FincartResourcesIndia-bff4

| Service | Current | Proposed | Action |
|------|--------|---------|--------|
| fincart-lms | bff4 | bff4 | Retained |
| fincart-assets | bff4 | plan-001 | Moved |
| fincart-fp | bff4 | plan-001 | Moved |
| fincart-mis | bff4 | plan-003 | Moved |
| fincart-thirdparty | bff4 | — | Excluded |

**Outcome:**  
Plan right-sized based on actual utilization, reducing cost while maintaining adequate headroom.

---

#### fincart-prod-linux-app-plan-002 → plan-001

| Service | Current Plan | Proposed Plan | Action |
|------|--------------|---------------|--------|
| fincart-ai | plan-002 | plan-001 | Retained |
| fincart-python | plan-002 | plan-001 | Retained |
| fincart-api-registry | plan-002 | plan-001 | Retained |
| fincart-portfolio | plan-002 | plan-001 | Retained |
| fincart-report | plan-002 | plan-001 | Retained |
| fincart-transaction | plan-002 | plan-001 | Retained |
| fincart-assets | bff4 | plan-001 | Added |
| fincart-fp | bff4 | plan-001 | Added |

**Outcome:**  
High-memory and core processing workloads consolidated for better utilization and future scalability.

---

#### fincart-prod-linux-app-plan-003

| Service | Current | Proposed | Action |
|------|--------|---------|--------|
| fincart-api-gateway-ind | plan-003 | plan-003 | Retained |
| fincart-cms-ind | plan-003 | plan-003 | Retained |
| fincart-communication-ind | plan-003 | plan-003 | Retained |
| fincart-core-ind | plan-003 | plan-003 | Retained |
| fincart-dmkt-ind | plan-003 | plan-003 | Retained |
| fincart-mis | bff4 | plan-003 | Added |

**Outcome:**  
No resizing required; logical grouping of integration and communication services achieved.

---

## 3.3 Proposed Changes

### 1️⃣ Create New App Service Plan – fincart-prod-linux-app-plan-001

A new App Service Plan will be created using **Azure App Service Premium v3 – P2mv3 (Memory-Optimized M-Series)**.  
Although it falls under the same Premium v3 cost category, it provides **double memory (32 GB)** due to the **M-Series SKU**.

**Services to be moved:**
- fincart-ai  
- fincart-python  
- fincart-api-registry  
- fincart-portfolio  
- fincart-report  
- fincart-transaction  
- fincart-assets  
- fincart-fp  

**Capacity & Monitoring Impact:**
- Current projected average memory utilization: **~36%**
- Even after enabling **New Relic APM**, memory utilization is expected to **remain below 50%**
- This leaves sufficient headroom for **peak load and future service deployments**

---

### 2️⃣ Optimize Existing Plan – ASP-FincartResourcesIndia-bff4

After moving **assets, fp, and mis** services out, this plan will mainly run:

- fincart-lms  
- fincart-thirdparty  

**Capacity & Monitoring Impact:**
- Even after enabling **New Relic on LMS**, memory consumption is expected to **remain below 50%**
- Ensures cost optimization without impacting application performance or availability

---

### 3️⃣ Optimize Plan – fincart-prod-linux-app-plan-003

- Current average memory utilization on this plan is approximately **~51%**
- The **fincart-mis** service will be moved from **bff4** to this plan
- **New Relic is already enabled on fincart-core**

**Capacity & Monitoring Impact:**
- After enabling **New Relic on MIS**, total memory utilization is expected to **remain below 60%**
- No App Service Plan resizing is required

---

### 4️⃣ Decommission App Service Plan – fincart-prod-linux-app-plan-002

- The **fincart-prod-linux-app-plan-002** will be decommissioned after service migration
- This plan is no longer required and is **not aligned with the memory-optimized (M-Series) plan strategy**

**Outcome:**
- Reduced infrastructure footprint  
- Simplified App Service Plan architecture  
- Direct contribution to cost savings  

---

## 4. Cost Comparison & Savings

| Description | Amount (INR) |
|-----------|--------------|
| Current Monthly Cost | 51,792 |
| Proposed Monthly Cost | 45,816 |
| Monthly Savings | 5,976 |
| Annual Savings | 71,712 |
| Cost Reduction | ~11.5% |

---

## 5. Key Benefits

- ~11.5% monthly infrastructure cost reduction  
- Improved memory utilization across all plans  
- Clear workload segregation (Core, APIs, Support services)  
- Enhanced scalability and operational clarity  
- No compromise on performance or availability  

---

## 6. Risks & Mitigation

| Risk | Mitigation |
|----|-----------|
| Resource contention | Adequate headroom maintained (<60% average usage) |
| Migration impact | Phased migration with validation |
| Scaling needs | Premium v3 supports vertical & horizontal scaling |

---

## 7. Recommendation

It is recommended to proceed with the proposed optimized App Service Plan architecture to achieve cost savings, improved utilization, and operational clarity while maintaining production-grade reliability.

---

**Prepared By:**  
Himanshu Parashar
