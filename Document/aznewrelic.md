# Azure SQL Logs Integration with New Relic

## Architecture Options and Cost Considerations

---

# 1. Purpose

This document explains the **possible methods to send Azure SQL logs to New Relic** and highlights **potential cost considerations from both Azure and New Relic**.

The objective is to evaluate the available architecture options before enabling centralized log visibility in New Relic.

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

* Azure SQL **metrics are visible in New Relic**
* Azure SQL **logs are stored and investigated in Azure Log Analytics**
* New Relic is currently used mainly for **monitoring dashboards and alerts**

This architecture **does not send full database logs to New Relic**.

---

# 3. Requirement

If there is a requirement to **view Azure SQL logs directly in New Relic**, logs must be forwarded from Azure to New Relic using an integration pipeline.

Multiple architecture options exist, each with **different Azure infrastructure considerations and potential New Relic ingestion charges**.

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
* Highly scalable architecture
* Common enterprise integration pattern

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

### New Relic Cost Components

* Logs forwarded to New Relic count toward **New Relic data ingestion limits**

### Advantages

* Logs are already available in **Log Analytics**
* No additional streaming infrastructure required

### Disadvantages

* Duplicate log ingestion may occur
* Additional integration configuration required

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

* Azure Function executions
* Execution duration and memory usage

Azure Functions provide a **free monthly execution allowance**, so for low-volume workloads the cost may remain minimal.

### New Relic Cost Components

* Logs sent through the API count toward **New Relic data ingestion**

### Advantages

* Logs can be **filtered before sending to New Relic**
* Reduced log volume sent to New Relic
* No Event Hub infrastructure required

### Disadvantages

* Requires custom configuration
* Additional operational maintenance

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

Minimal ingestion impact as **only metrics and limited telemetry are sent**, not full database logs.

### Advantages

* Simplest architecture
* Minimal operational overhead
* Suitable for performance monitoring and alerting

### Disadvantages

* Full database logs are **not available in New Relic**
* Root cause investigation must be performed in Azure

---

# 5. Important Clarification – Database Size vs Log Volume

Database storage size is **not directly related to log ingestion volume**.

Log generation depends on factors such as:

* Query execution activity
* Application workload
* Diagnostic log categories enabled
* Error and exception events
* Monitoring configuration

Therefore, log ingestion costs depend on **actual log volume generated**, not database storage size.

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

Forwarding only required categories helps control ingestion volume and associated costs.

---

# 7. Cost Impact Summary

| Component                 | Azure Cost                           | New Relic Cost                                 |
| ------------------------- | ------------------------------------ | ---------------------------------------------- |
| Azure Event Hub Streaming | Yes                                  | Yes                                            |
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
→ Deadlock analysis
→ Root cause diagnostics

### Benefits of this approach

* Avoids duplicate log ingestion
* Reduces infrastructure cost
* Simplifies operational management

---

# 9. If Logs Must Be Available in New Relic

Recommended architecture:

Azure SQL Database
→ Diagnostic Settings
→ Azure Function (filter logs)
→ New Relic Logs API

Only forward essential categories such as:

* Errors
* Deadlocks
* Timeouts

This helps **control Azure and New Relic ingestion costs** while still providing centralized log visibility.

---

# 10. Conclusion

Azure SQL logs can be forwarded to New Relic through several architecture options including Event Hub streaming, Log Analytics integration, or filtered forwarding using Azure Functions.

Each option introduces **different Azure infrastructure considerations and potential New Relic ingestion charges**.

Before enabling log forwarding, organizations should evaluate:

* Expected log generation volume
* Azure infrastructure considerations
* New Relic ingestion limits
* Operational monitoring requirements

Selecting the appropriate architecture helps maintain **observability capabilities while managing operational costs effectively**.
