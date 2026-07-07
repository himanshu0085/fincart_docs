# Root Cause Analysis (RCA)

## CPU Spike Analysis – ASP-FincartResourcesIndia-bff4 (Last 7 Days)

---

# 1. Executive Summary

A performance investigation was conducted on the **ASP-FincartResourcesIndia-bff4** Azure App Service Plan after observing increased CPU utilization. The objective was to identify which hosted application was contributing most to the CPU consumption over the **last seven days**.

CPU Time (Maximum) metrics were collected for each application hosted under the App Service Plan and compared to determine the workload pattern and identify the primary contributor.

---

# 2. Environment Details

| Parameter        | Value                          |
| ---------------- | ------------------------------ |
| Cloud Platform   | Microsoft Azure                |
| Resource Type    | App Service Plan               |
| App Service Plan | ASP-FincartResourcesIndia-bff4 |
| Analysis Period  | Last 7 Days                    |
| Metric Used      | CPU Time (Maximum)             |

---

# 3. Applications Hosted

| Application        | Status  |
| ------------------ | ------- |
| fincart-assets     | Running |
| fincart-fp         | Running |
| fincart-lms        | Running |
| fincart-mis        | Running |
| fincart-thirdparty | Running |
| fincart-batch      | Stopped |

Since **fincart-batch** was stopped, it was excluded from the analysis.

---

# 4. Investigation Methodology

Each application was individually analyzed using Azure Monitor.

**Metric Selected**

* CPU Time (Maximum)
* Time Range: Last 7 Days
* Aggregation: Maximum

The CPU utilization pattern of each application was compared to identify sustained or recurring workloads.

---

# 5. Individual Application Analysis

---

## 5.1 fincart-lms

### Observation

* Highest CPU consumption among all applications.
* CPU Time reached approximately **3.06 minutes**.
* Repeated CPU spikes observed across multiple days.
* Long-duration CPU activity instead of isolated peaks.
* Clear recurring workload pattern.

### Analysis

The application consistently consumed CPU across several days, indicating recurring processing rather than random activity.

Possible reasons include:

* Scheduled Jobs
* Batch Processing
* Heavy API traffic
* Background Services
* Large database operations

### Conclusion

**Highest contributor to CPU utilization.**

---

## 5.2 fincart-fp

### Observation

* Maximum CPU Time approximately **2.30 minutes**.
* Multiple spikes observed.
* Most spikes were isolated.
* No prolonged CPU usage.

### Analysis

The application executes periodic CPU-intensive operations but returns to normal quickly.

### Conclusion

**Moderate contributor.**

---

## 5.3 fincart-assets

### Observation

* Maximum CPU Time approximately **1.54 minutes**.
* CPU spikes observed on multiple days.
* Moderate workload.
* Activity remains intermittent.

### Analysis

The application processes periodic tasks but does not maintain sustained CPU utilization.

### Conclusion

**Moderate contributor.**

---

## 5.4 fincart-thirdparty

### Observation

* Maximum CPU Time approximately **1.46 minutes**.
* Multiple isolated spikes.
* No long-running CPU utilization.

### Analysis

CPU spikes appear related to external integrations or periodic API calls.

### Conclusion

**Low to Moderate contributor.**

---

## 5.5 fincart-mis

### Observation

* Maximum CPU Time approximately **7.78 seconds**.
* CPU remained almost flat throughout the week.
* Only very small spikes.

### Analysis

Application shows negligible CPU utilization.

### Conclusion

**Not a contributor.**

---

# 6. Comparative Analysis

| Application        | Maximum CPU Time | Pattern                     | Contribution |
| ------------------ | ---------------- | --------------------------- | ------------ |
| **fincart-lms**    | **3.06 min**     | Continuous recurring spikes | **High**     |
| fincart-fp         | 2.30 min         | Periodic spikes             | Medium       |
| fincart-assets     | 1.54 min         | Moderate spikes             | Medium       |
| fincart-thirdparty | 1.46 min         | Isolated spikes             | Low-Medium   |
| fincart-mis        | 7.78 sec         | Minimal activity            | Very Low     |

---

# 7. Ranking by CPU Consumption

| Rank | Application        | Observation                |
| ---- | ------------------ | -------------------------- |
| 1    | **fincart-lms**    | Highest CPU utilization    |
| 2    | fincart-fp         | Medium CPU utilization     |
| 3    | fincart-assets     | Medium CPU utilization     |
| 4    | fincart-thirdparty | Low-Medium CPU utilization |
| 5    | fincart-mis        | Negligible CPU utilization |

---

# 8. Root Cause

Based on Azure Metrics collected over the last seven days:

**Primary Contributor**

**fincart-lms**

Evidence:

* Highest maximum CPU Time (3.06 minutes)
* Recurring CPU spikes across multiple days
* Sustained workload compared to all other applications
* Consistent CPU pattern indicating scheduled or background processing

Secondary contributors include:

* fincart-fp
* fincart-assets

These applications also generate CPU spikes but their duration and frequency are significantly lower than fincart-lms.

---

# 9. Impact Assessment

The recurring CPU consumption from **fincart-lms** contributes to:

* Increased App Service Plan CPU utilization
* Potential increase in API response time
* Resource contention with other applications hosted on the same App Service Plan
* Reduced capacity during peak workload windows

No evidence of continuous CPU saturation or application outage was observed from the metrics.

---

# 10. Recommendations

## Immediate Actions

### Application Investigation

Review the following for **fincart-lms**:

* Scheduled jobs
* Background services
* Cron jobs
* Batch processing
* Long-running API requests

---

### Application Insights

Analyze:

* Slow Requests
* Failed Requests
* Exceptions
* Dependency Calls
* Performance Trends

---

### Database Investigation

Review:

* Long-running SQL queries
* Stored Procedures
* Blocking
* Deadlocks
* Database CPU utilization

---

### External Dependencies

Investigate:

* Third-party API calls
* Retry mechanisms
* Network latency
* Timeout handling

---

### Infrastructure

Configure Azure Monitor Alerts for:

* CPU Percentage
* Memory Percentage
* Response Time
* HTTP 5xx Errors

Consider enabling Autoscale or scaling the App Service Plan if the observed workload is expected during business operations.

---

# 11. Final Conclusion

Based on the seven-day CPU Time analysis of all running applications hosted under **ASP-FincartResourcesIndia-bff4**, the investigation identifies **fincart-lms** as the primary contributor to CPU utilization. It exhibits the highest CPU Time (3.06 minutes) and a recurring workload pattern across multiple days, indicating regular CPU-intensive processing.

While **fincart-fp**, **fincart-assets**, and **fincart-thirdparty** also show intermittent CPU spikes, their impact is comparatively lower and does not match the sustained behavior observed in **fincart-lms**.

**Final Assessment:**

* **Primary Contributor:** `fincart-lms`
* **Secondary Contributors:** `fincart-fp`, `fincart-assets`
* **Minimal Impact:** `fincart-thirdparty`
* **Negligible Impact:** `fincart-mis`

Further investigation should focus on the application logic, scheduled workloads, database interactions, and external dependencies of **fincart-lms** to determine the exact operation responsible for the recurring CPU spikes.
