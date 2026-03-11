# Azure SQL Logs Integration with New Relic

## Architecture Options and Cost Considerations

---

# 1. Purpose

This document describes the **possible methods to send Azure SQL logs to New Relic** and outlines **all potential cost implications on the Azure side and New Relic side**.

The goal is to help determine the most suitable architecture for enabling centralized logging in New Relic while understanding the associated operational and financial impact.

---

# 2. Current Monitoring Setup

The current monitoring architecture for the environment is as follows:

Azure SQL Database
→ Azure Diagnostic Settings
→ Azure Log Analytics Workspace
→ Logs investigated in Azure

Azure SQL Database
→ Azure Monitor Metrics
→ New Relic Azure Integration
→ Metrics visible in New Relic dashboards

### Current State

* Azure SQL **metrics are visible in New Relic**
* Azure SQL **logs are stored and analyzed in Azure Log Analytics**
* New Relic is currently used primarily for **monitoring and alerting**

This setup **does not create additional log ingestion costs in New Relic**.

---

# 3. Requirement

If there is a requirement to **view Azure SQL logs directly in New Relic**, then logs must be forwarded from Azure to New Relic through an integration pipeline.

This introduces additional infrastructure components and **potential costs on both the Azure and New Relic sides**.

---

# 4. Possible Architectures

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
* Event streaming data volume
* Data retention configuration

Cost depends on:

* Amount of log data generated
* Throughput configuration
* Retention settings

### New Relic Cost Components

* Logs count toward **New Relic data ingestion**
* New Relic provides a **free ingest allowance (approximately 100 GB/month)**
* Additional charges apply if ingestion exceeds the free allowance

### Advantages

* Real-time log streaming
* Scalable architecture
* Reliable integration pattern
* Commonly used for centralized observability platforms

### Disadvantages

* Azure Event Hub operational cost
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
* Query execution within the workspace

### New Relic Cost Components

* Logs ingested via API count toward **New Relic ingest volume**

### Advantages

* Logs already stored in Azure
* No additional streaming infrastructure required

### Disadvantages

* Duplicate ingestion (Azure + New Relic)
* Increased operational complexity

---

## Option 3 – Azure Function Forwarding Logs to New Relic

### Architecture

Azure SQL Database
→ Diagnostic Settings
→ Log Analytics Workspace
→ Azure Function
→ New Relic Log API
→ New Relic Logs

### Azure Cost Components

Azure may charge for:

* Log Analytics ingestion
* Azure Function executions
* Compute duration
* Outbound data transfer

### New Relic Cost Components

* Logs sent through API count toward **New Relic ingest volume**

### Advantages

* Fine control over filtering and transformation
* Ability to forward only selected logs

### Disadvantages

* Custom development required
* Additional operational management
* Multiple Azure services involved

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

Minimal ingestion impact as only **metrics and limited telemetry are sent**, **not full database logs**.

### Advantages

* Simple and stable configuration
* No additional log ingestion infrastructure
* Low operational overhead
* Suitable for monitoring dashboards and alerts

### Disadvantages

* Full database logs are **not available in New Relic**
* Root cause analysis must be performed in Azure

---

# 5. Important Clarification – Database Size vs Log Volume

It is important to note that **database storage size is not directly related to log ingestion volume**.

The amount of log data generated depends on operational factors such as:

* Query execution activity
* Application traffic volume
* Error and exception events
* Diagnostic log categories enabled in Azure
* Frequency of database operations
* Monitoring and telemetry configuration

Therefore, **log ingestion cost cannot be estimated based solely on database size**. Instead, the cost is determined by the **actual volume of log data generated and forwarded to external systems such as New Relic**.

Any architecture that forwards logs to New Relic should consider:

* Expected application workload
* Diagnostic categories enabled
* Data retention and log filtering strategy

Proper filtering and selection of log categories is recommended to **control data ingestion volume and manage costs effectively**.

---

# 6. Azure Diagnostic Log Categories Impact

Some diagnostic categories produce significantly more logs than others.

### High Volume Categories

* QueryStoreRuntimeStatistics
* QueryStoreWaitStatistics
* SQLInsights

### Lower Volume Categories

* Errors
* Deadlocks
* Timeouts

To control ingestion volume and costs, it is recommended to forward **only critical diagnostic categories where possible**.

---

# 7. Cost Impact Summary

| Component                 | Azure Cost | New Relic Cost                                 |
| ------------------------- | ---------- | ---------------------------------------------- |
| Azure Event Hub Streaming | Yes        | Yes                                            |
| Log Analytics Storage     | Yes        | Possible                                       |
| Azure Function Processing | Yes        | Yes                                            |
| Metrics Only Integration  | Minimal    | Minimal (metrics only, full logs not included) |

---

# 8. Recommended Approach (Industry Practice)

Most organizations implement the following architecture:

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
* Reduces platform costs
* Keeps monitoring and debugging responsibilities separated

---

# 9. If Logs Must Be Centralized in New Relic

Recommended architecture:

Azure SQL Database
→ Diagnostic Settings
→ Azure Event Hub
→ New Relic Logs

Only forward essential diagnostic categories such as:

* Errors
* Deadlocks
* Timeouts

Avoid forwarding high-volume diagnostic categories unless required.

---

# 10. Conclusion

Sending Azure SQL logs to New Relic is technically possible through several integration architectures. However, each method introduces **additional Azure infrastructure costs and potential New Relic ingestion charges**.

Before enabling log forwarding, the following factors should be evaluated:

* Expected log generation volume
* Azure infrastructure costs
* New Relic ingestion limits
* Operational monitoring requirements

A carefully designed architecture should balance **observability requirements, operational complexity, and cost efficiency**.
