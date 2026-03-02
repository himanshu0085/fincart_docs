
# Standard Operating Procedure (SOP)
## Monitoring Stage Azure SQL Database in New Relic

---

## Document Information

| Field | Details |
|--------|----------|
| Environment | Stage |
| Database | FINSTAGE |
| SQL Server | stagefincart |
| Resource Group | fincart_stage_resources |
| Monitoring Tool | New Relic |
| Integration Type | Azure Monitor Metrics |

---

# 1. Purpose

This document defines the standard procedure to integrate, validate, and monitor the **Stage Azure SQL Database (FINSTAGE)** in **New Relic** using Azure Monitor metrics.

---

# 2. Scope

This SOP applies only to:

- Stage Azure SQL Database
- Azure Subscription associated with Stage environment
- Monitoring through New Relic Azure integration

This document does not cover UAT or Production environments.

---

# 3. Prerequisites

Before proceeding, ensure:

- Access to Azure Portal (Stage Subscription)
- Reader or Contributor access to Stage resources
- Service Principal created in Azure
- Access to New Relic account
- Azure Monitor enabled for SQL Database

---

# 4. Integration Steps – Stage Azure SQL

## Step 1: Navigate to Azure Integration

1. Log in to **New Relic**
2. Navigate to:

```

Infrastructure → Azure

```

---

## Step 2: Add Azure Account

1. Click **Add an Azure account**
2. Select **Azure Monitor Metrics**
3. Click **Connect**

---

## Step 3: Provide Azure Credentials

Enter the following details for the Stage subscription:

- Subscription ID
- Tenant ID
- Client ID (Application ID)
- Client Secret

### Required Roles for Service Principal

- Reader role on Stage subscription
- Monitoring Reader role (recommended)

Click **Connect**.

---

## Step 4: Configure Resource Scope (Stage Only)

After connection:

1. Enable **Limit to Resource Group**
2. Add:

```

fincart_stage_resources

```

3. Click **Save**

This ensures only Stage resources are monitored.

---

## Step 5: Enable Azure SQL Database Metrics

Within integration settings:

- Ensure **Azure SQL Database metrics** are enabled
- Set polling frequency (Recommended: 5 minutes)
- Save configuration

---

## Step 6: Verify Integration Status

Confirm:

- Azure account status shows **Connected**
- No authentication errors
- Stage resources visible in New Relic Infrastructure

---

# 5. Validation of Metrics (Post Integration)

Navigate to:

```

New Relic → Query your data

````

---

## 5.1 CPU Validation

```sql
SELECT *
FROM Metric
WHERE metricName = 'azure.sql.servers.databases.cpu_percent'
SINCE 30 minutes ago
LIMIT 10
````

**Expected Result:**

* Records returned
* entity.name = finstage
* Azure region = Central India

---

## 5.2 Storage Validation

```sql
SELECT *
FROM Metric
WHERE metricName = 'azure.sql.servers.databases.storage'
SINCE 30 minutes ago
LIMIT 10
```

**Expected Result:**

* Summary object visible
* storage.sum field present
* entity.name = finstage

Integration is considered successful once both queries return data.

---

# 6. Dashboard Configuration – Stage Monitoring

Create a custom dashboard:

```
Dashboards → Create Dashboard
Name: Azure SQL - Stage Monitoring
```

Add the following charts:

---

## 6.1 Storage Usage (GB)

```sql
SELECT latest(azure.sql.servers.databases.storage.sum)/1024/1024/1024
FROM Metric
SINCE 30 minutes ago
FACET entity.name
```

Recommended Chart Type: Billboard or Bar

---

## 6.2 CPU Percentage

```sql
SELECT latest(azure.sql.servers.databases.cpu_percent)
FROM Metric
SINCE 30 minutes ago
FACET entity.name
```

Recommended Chart Type: Line or Billboard

---

## 6.3 Physical Data Read Percentage

```sql
SELECT latest(azure.sql.servers.databases.physical_data_read_percent)
FROM Metric
SINCE 30 minutes ago
FACET entity.name
```

---

## 6.4 Failed Connections

```sql
SELECT latest(azure.sql.servers.databases.connection_failed)
FROM Metric
SINCE 30 minutes ago
FACET entity.name
```

---

# 7. Recommended Alert Configuration (Stage)

Navigate to:

```
Alerts & AI → Policies
```

Recommended thresholds:

| Metric             | Threshold | Duration   |
| ------------------ | --------- | ---------- |
| CPU %              | > 80%     | 5 minutes  |
| Storage Usage      | > 85%     | 10 minutes |
| Failed Connections | > 5       | 5 minutes  |

---

# 8. Troubleshooting

## Issue: No Data Visible

Verify:

* Correct Azure Subscription connected
* Resource Group filter includes `fincart_stage_resources`
* Polling interval is active
* Time range is set to last 30 minutes or 3 hours
* Azure SQL metrics are enabled in integration settings

---

## Issue: Storage Metric Not Displaying in Chart

Reason:
Storage metric is a summary type and requires `.sum` attribute.

Correct query format:

```sql
SELECT latest(azure.sql.servers.databases.storage.sum)
FROM Metric
SINCE 30 minutes ago
```

---

# 9. Success Criteria

The Stage Azure SQL integration is considered successful when:

* CPU metrics return data
* Storage metrics return summary values
* FINSTAGE database appears in query results
* Dashboard displays real-time values
* Alerts can be configured successfully

---

# End of Document

```
