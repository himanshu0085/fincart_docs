# Azure SQL Logs Integration with New Relic

## Architecture Options and Cost Considerations

---

# 1. Purpose

This document explains the **possible methods to send Azure SQL logs to New Relic** and identifies **all potential cost points on the Azure side and the New Relic side**.

The objective is to help evaluate the architecture choices before enabling centralized logging in New Relic.

---

# 2. Current Monitoring Setup

Current monitoring configuration:

Azure SQL Database
→ Azure Diagnostic Settings
→ Azure Log Analytics Workspace
→ Logs analyzed in Azure

Azure SQL Database
→ Azure Monitor Metrics
→ New Relic Azure Integration
→ Metrics visible in New Relic dashboards

### Current State

* **Metrics** are visible in New Relic
* **Logs** are stored in Azure Log Analytics
* New Relic is currently used mainly for **monitoring and alerting**

This architecture does **not send full database logs to New Relic**.

---

# 3. Requirement

If logs must also be visible in **New Relic**, they must be forwarded from Azure to New Relic through an integration pipeline.

Multiple architectures can achieve this, each with **different Azure infrastructure costs and New Relic ingestion implications**.

---

# 4. Architecture Options

## Option 1 – Azure Event Hub → New Relic Logs

### Architecture

Azure SQL Database
→ Diagnostic Settings
→ Azure Event Hub
→ New Relic Log Ingestion
→ New Relic Logs

### Azure Cost Components

Azure may charge for:

* Event Hub Namespace
* Event Hub Throughput Units
* Data streaming volume
* Retention configuration

### New Relic Cost Components

* Logs count toward **New Relic data ingestion**
* New Relic provides a **free ingest allowance (~100 GB/month)**
* Additional ingestion beyond this threshold may incur charges

### Advantages

* Real-time log streaming
* Highly scalable
* Standard enterprise architecture

### Disadvantages

* Additional Azure infrastructure cost (Event Hub)
* Potential New Relic ingestion cost

---

## Option 2 – Azure Log Analytics → New Relic Integration

### Architecture

Azure SQL Database
→ Diagnostic Settings
→ Log Analytics Workspace
→ Integration / API Export
→ New Relic Logs

### Azure Cost Components

Azure charges may apply for:

* Log Analytics data ingestion
* Log storage
* Query execution

Log Analytics pricing is generally based on **GB of data ingested**.

### New Relic Cost Components

* Logs forwarded to New Relic count toward **New Relic ingest limits**

### Advantages

* Logs already available in Log Analytics
* No additional streaming infrastructure

### Disadvantages

* Duplicate log ingestion
* Increased operational complexity

---

## Option 3 – Azure Function → New Relic Logs (Filtered Forwarding)

### Architecture

Azure SQL Database
→ Diagnostic Settings
→ Log Analytics Workspace
→ Azure Function
→ New Relic Log API
→ New Relic Logs

### Azure Cost Components

Azure may charge for:

1. **Log Analytics ingestion**

   * Logs stored in Log Analytics are charged based on **data volume ingested**

2. **Azure Function execution**

   * Billed based on:

     * Number of executions
     * Execution duration
     * Memory consumption

Azure provides a **free monthly allowance** for Function executions, so in many cases the cost remains minimal.

### New Relic Cost Components

* Logs sent through API count toward **New Relic data ingestion**

### Advantages

* Ability to **filter logs before sending**
* Reduced data volume sent to New Relic
* Lower overall ingestion cost
* No Event Hub infrastructure required

### Disadvantages

* Requires custom configuration
* Additional maintenance for the function

---

## Option 4 – Azure Native Integration (Metrics Only)

### Architecture

Azure SQL Database
→ Azure Monitor
→ New Relic Azure Integration
→ Metrics and limited telemetry in New Relic

### Azure Cost Components

Minimal Azure cost for metrics collection.

### New Relic Cost Components

Minimal ingestion impact as **only metrics and limited telemetry are sent**, **not full database logs**.

### Advantages

* Simplest architecture
* Minimal operational overhead
* Suitable for monitoring dashboards and alerts

### Disadvantages

* Full database logs are **not available in New Relic**
* Root cause analysis must be performed in Azure

---

# 5. Important Clarification – Database Size vs Log Volume

Database storage size is **not directly related to log ingestion volume**.

Log generation depends on factors such as:

* Query execution activity
* Application workload
* Diagnostic log categories enabled
* Error and exception events
* Monitoring configuration

Therefore, log ingestion costs depend on **actual log volume generated**, not on the size of the database.

---

# 6. Azure Diagnostic Categories Impact

Certain diagnostic categories generate significantly more logs than others.

### High-volume categories

* QueryStoreRuntimeStatistics
* QueryStoreWaitStatistics
* SQLInsights

### Lower-volume categories

* Errors
* Deadlocks
* Timeouts

Forwarding only required categories helps control ingestion costs.

---

# 7. Cost Impact Summary

| Component                 | Azure Cost                           | New Relic Cost                                 |
| ------------------------- | ------------------------------------ | ---------------------------------------------- |
| Azure Event Hub Streaming | Yes                                  | Yes                                            |
| Log Analytics Storage     | Yes                                  | Possible                                       |
| Azure Function Processing | Yes (execution-based, often minimal) | Yes                                            |
| Metrics Only Integration  | Minimal                              | Minimal (metrics only, full logs not included) |

---

# 8. Recommended Approach

A common industry approach separates monitoring and log analysis:

New Relic
→ Metrics monitoring
→ Performance dashboards
→ Alerting

Azure Log Analytics
→ Database logs
→ Error investigation
→ Root cause analysis

This architecture:

* Avoids duplicate log ingestion
* Reduces operational cost
* Maintains clear separation between monitoring and debugging

---

# 9. When Logs Must Be Available in New Relic

Recommended architecture:

Azure SQL Database
→ Diagnostic Settings
→ Azure Function (filter logs)
→ New Relic Logs API

Only forward essential log categories such as:

* Errors
* Deadlocks
* Timeouts

This approach helps **control Azure and New Relic ingestion costs** while still providing centralized log visibility.

---

# 10. Conclusion

Azure SQL logs can be forwarded to New Relic using several architectures, including Event Hub streaming, Log Analytics integration, or filtered forwarding using Azure Functions.

Each method introduces **different Azure infrastructure costs and potential New Relic ingestion charges**.

A cost-aware implementation should:

* Evaluate expected log volume
* Select the appropriate architecture
* Forward only required diagnostic categories
* Monitor ingestion volume regularly

Proper architecture selection helps maintain **observability capabilities while managing operational costs effectively**.
