# ROOT CAUSE ANALYSIS  
## Website Performance Degradation — fincart.com (WordPress on Azure App Service)

---

## 1. Introduction

The production WordPress website **fincart.com** experienced significant performance degradation between **09:47 AM and 10:12 AM IST on April 27, 2026**.

During this period, the website became slow and intermittently unavailable to end users. Investigation identified a sustained spike in CPU utilization on the Azure App Service Plan, which exhausted application processing capacity and impacted request handling.

---

## 2. Key Information (Metadata)

| Field | Details |
|------|--------|
| Incident Type | Website Performance Degradation / Partial Outage |
| Severity | Critical |
| Environment | Production |
| Platform | Azure App Service (Linux – WordPress) |
| App Service Plan | fincart-wordpress-app-plan-001 |
| Start Time | April 27, 2026, 09:47 AM IST |
| End Time | April 27, 2026, ~10:12 AM IST |
| Duration | ~25 minutes |
| Resolution | Manual Web App Restart |
| Status | Resolved |

---

## 3. Issue Summary

Between **09:47 AM and 10:12 AM IST**, the website experienced severe slowness and intermittent downtime.

### Observations

- CPU utilization spiked to **~96–97.6%**
- CPU remained elevated for ~25 minutes
- Memory utilization remained stable (~34%) → confirms **CPU-bound issue**
- High request failure rate (~98–99%)
- Application response times increased significantly

---

## 4. Root Cause Analysis

### Problem

The website became slow and intermittently unavailable due to **CPU saturation**, which prevented the application from processing incoming requests effectively.

---

### Primary Root Cause

A **sudden surge of automated scanning and probing traffic** targeted multiple application endpoints during the incident window.

This included repeated requests to both valid and invalid paths, along with POST requests to resource-intensive WordPress endpoints. The volume and nature of these requests caused excessive PHP processing, resulting in CPU utilization reaching **~97.6%** and exhausting available application resources.

---

### Supporting Evidence

| Metric | Value |
|------|------|
| Total Requests | ~10,651 |
| Total Errors | ~10,540 (~98.9%) |
| Peak Window | ~10:00 AM IST |
| Requests (Peak 5-min) | ~3,478 |
| POST Requests (Peak) | ~548 |
| Failed Requests (Peak) | ~3,467 |

---

### High-Frequency Endpoints Observed

| Endpoint | Requests | Observation |
|---------|---------|------------|
| `/manager/html` | 1,524 | Common exploit probe |
| `/conf/defaults.ini` | 1,113 | Configuration probing |
| `/wp-cron.php` | 779 | High-cost WordPress execution |
| `/` | 534 | Root endpoint traffic |
| `/etc/passwd` | 74 | Linux exploit attempt |
| `/windows/win.ini` | 22 | Windows probe attempt |

---

### Abnormal Traffic Pattern

- Majority of requests targeted **non-existent or sensitive system paths**
- Traffic pattern indicates **automated vulnerability scanning / bot activity**
- Extremely high error rate (~99%) confirms requests were not legitimate user traffic
- Repeated POST requests to WordPress endpoints contributed significantly to CPU load

---

### Cascade Effect

```

Automated scanning & probing traffic
→ High volume of HTTP requests
→ Repeated POST requests to resource-intensive endpoints
→ CPU utilization spikes (~97%)
→ PHP-FPM worker pool fully consumed
→ Legitimate requests queued
→ Increased response time and failures
→ Website becomes slow / unavailable

```

---

## 5. Impact and Mitigation

### Customer Impact

- Website degraded / intermittently unavailable for ~25 minutes
- Users experienced slow responses and request timeouts

---

### Mitigation Steps

| Step | Action |
|------|--------|
| 1 | Identified CPU spike via Azure Metrics |
| 2 | Analyzed traffic patterns using Azure Observability |
| 3 | Restarted Web App via Azure Portal |
| 4 | Application recovered and CPU normalized |

---

## 6. What Was NOT the Root Cause

- Not a memory issue (memory stable)
- Not a database failure
- Not an Azure platform outage
- Infrastructure remained operational

---

## 7. Preventative Actions

### High Priority

- Block unnecessary endpoints:
  - `/xmlrpc.php`
  - `/wp-cron.php` (or control execution)
- Implement request filtering at web server layer (nginx rules)
- Restrict administrative endpoints access

---

### Medium Priority

- Enable Azure WAF (Front Door / Application Gateway)
- Implement rate limiting for high-frequency requests
- Configure auto-scaling based on CPU thresholds

---

### Low Priority

- Perform security audit of WordPress installation
- Remove publicly exposed unnecessary files
- Monitor unusual traffic patterns

---

## 8. Conclusion

The incident was caused by a **burst of automated scanning and probing traffic**, which generated a high volume of requests to multiple endpoints, including resource-intensive WordPress paths.

This resulted in CPU saturation on the App Service Plan, exhausting application resources and leading to temporary performance degradation and downtime.

The issue was mitigated by restarting the application, which restored normal service operation.
```
