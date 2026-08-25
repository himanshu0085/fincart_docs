# Day-to-Day Activities & Operational Checklist

## 1. Purpose

This document defines the regular operational activities required for monitoring, cost optimization, infrastructure maintenance, application health, security, and Azure DevOps pipeline management.

The objective is to ensure that all environments remain healthy, pipelines operate properly, alerts are addressed on time, infrastructure costs are monitored, and required maintenance activities are completed within the defined schedule.

---

## 2. Daily Activities

### 2.1 Azure Cost Monitoring

* Check Azure cost on a daily basis.
* Review the cost consumption of Azure resources.
* Identify which resources are contributing to the overall Azure cost.
* Monitor for any unexpected increase in resource usage or cost.
* Investigate unusual cost increases and identify the reason.
* Identify possible opportunities to optimize or reduce unnecessary costs.
* Maintain awareness of the major cost-generating resources so that abnormal increases can be identified quickly.

### 2.2 Automated Pipeline Monitoring

* Check the automated pipelines executed during the day.
* Verify that the required pipelines have completed successfully.
* Check for failed, partially completed, skipped, or cancelled pipeline executions.
* Review pipeline warnings even when the pipeline has completed successfully.
* Investigate recurring failures or warnings.
* Before making any changes to a pipeline, inform the Fincart team through email.

### 2.3 UAT and Stage Application Health

* Check the dashboard for both **UAT** and **Stage** environments.
* Verify that all required services are up and running.
* Check the overall health of the applications and services.
* Ensure that UAT and Stage are healthy before **10:00 AM**.
* If any service is down or unhealthy, investigate the issue.
* Inform the Fincart team when an issue requires their attention or may impact the environment.

### 2.4 Alert Monitoring

* Regularly check all infrastructure and application alerts.
* Read every alert carefully and understand the actual issue before taking any action.
* Do not ignore or miss alerts.
* Determine whether an alert is informational, a warning, or requires immediate action.
* Investigate critical and recurring alerts.
* Escalate important issues to the Fincart team when required.

### 2.5 Grafana Log Monitoring

* Check Grafana logs every day.
* Review logs for errors, warnings, abnormal activity, or recurring issues.
* Investigate significant or recurring errors.
* Verify that logs are being generated and collected properly.
* Check for any abnormal behavior that could affect application or infrastructure health.

---

## 3. Monthly Activities

### 3.1 Monthly Cost Analysis

* Prepare a monthly Azure cost analysis sheet.
* Review the previous month's Azure cost consumption.
* Identify resources with high or increasing costs.
* Compare the current month's cost with previous months where applicable.
* Identify unused or underutilized resources.
* Identify activities or resources where cost can be optimized.
* Review possible cost-saving opportunities with the Fincart team before making impactful changes.
* Maintain the cost analysis sheet for future reference.

### 3.2 VM Updates Using Azure Update Manager

* Perform VM updates once every month using **Azure Update Manager**.
* Before starting the update activity, verify that the required backup has completed successfully.
* Confirm that the backup is available before proceeding with the update.
* Apply the required updates through Azure Update Manager.
* Verify VM health after the update.
* Check that the required services are running after patching.
* Report any issue observed after patching to the Fincart team.

### 3.3 Grafana VM Health Check

* Perform a detailed health check of the Grafana VM **twice per month**.
* Check CPU utilization.
* Check memory utilization.
* Check disk utilization and available disk space.
* Check overall VM health.
* Review relevant Grafana and system logs.
* Identify abnormal resource consumption or infrastructure issues.
* Take corrective action or report the issue to the relevant team when required.

---

## 4. Log Retention and Storage Account Monitoring

### 4.1 Log Retention Period

The current Grafana log retention requirements are:

| Environment | Grafana Log Retention |
| ----------- | --------------------: |
| UAT         |                7 days |
| Stage       |               90 days |
| Production  |              180 days |

Once **180 days of retention** is completed for Production logs in Grafana, the log files should be moved to the designated Azure Storage Account.

The Storage Account retention period is:

**5 years**

### 4.2 Storage Account and Log Transfer Verification

* Perform the verification in **November after the 20th**.
* Check the designated Storage Account.
* Verify that the expected logs are being transferred from Grafana to the Storage Account.
* Confirm that the expected log files are available in the Storage Account.
* Verify that the log-transfer script is working correctly.
* Check whether the script is executing successfully.
* Confirm that the log archival process is working as expected.
* Verify that the retention process is functioning correctly.
* If logs are not being transferred or the script is not working, investigate the issue and inform the Fincart team.

---

## 5. Quarterly Activities

### 5.1 Sonar Token Update

* Update the **Sonar token in Azure DevOps once every 3 months**.
* Verify that the updated token is configured correctly in the required Azure DevOps configuration.
* Run the relevant pipeline after updating the token.
* Confirm that Sonar analysis is working correctly.
* Verify that there are no authentication or authorization failures.
* Confirm that the pipeline completes successfully after the token update.

---

## 6. Security Monitoring

### 6.1 Microsoft Defender Recommendations

* Regularly check **Microsoft Defender** recommendations and security suggestions.
* Review new security recommendations.
* Understand the priority and impact of each recommendation.
* Identify recommendations that require action.
* Inform the Fincart team about relevant security recommendations.
* Do not implement changes that may impact the application or infrastructure without informing the Fincart team.
* Track important recommendations until they are resolved or appropriately addressed.

---

## 7. Azure DevOps Pipeline Maintenance

### 7.1 Regular Pipeline Checks

* Regularly verify that all required Azure DevOps pipelines are working properly.
* Check for:

  * Failed pipelines
  * Pipeline warnings
  * Deployment failures
  * Configuration issues
  * Authentication or token issues
  * Agent-related issues
  * Environment-related failures
* Review warnings even when the pipeline completes successfully.
* Investigate recurring warnings and failures.
* Verify that automated deployments are functioning as expected.

### 7.2 Pipeline Change Management

Before making any change to an existing pipeline:

1. Identify and understand the issue.
2. Review the potential impact of the proposed change.
3. Inform the Fincart team through email.
4. Obtain the required confirmation or approval where applicable.
5. Make the required change.
6. Execute the pipeline.
7. Verify the pipeline result.
8. Confirm that the affected environment is working properly.
9. Document the change where required.

**Important:** Do not directly modify a working pipeline without informing the Fincart team, especially when the change can affect UAT, Stage, or Production.

---

## 8. Communication and Escalation

The Fincart team should be informed when:

* A critical alert is received.
* A pipeline fails repeatedly.
* A deployment issue is identified.
* A UAT or Stage service is down.
* A significant increase in Azure cost is observed.
* A security recommendation requires action.
* A VM update may have an application impact.
* A log-transfer or retention issue is identified.
* A pipeline change is required.
* Any operational change may impact the existing application or environment.
* Any unexpected production or infrastructure issue is identified.

All important changes and operational decisions should be communicated through the appropriate email or communication channel and documented where required.

---

## 9. Operational Frequency Summary

| Activity                                         | Frequency                  |
| ------------------------------------------------ | -------------------------- |
| Azure cost monitoring                            | Daily                      |
| Automated pipeline monitoring                    | Daily                      |
| UAT and Stage service health check               | Daily, before **10:00 AM** |
| Alert monitoring                                 | Daily / Regularly          |
| Grafana log monitoring                           | Daily                      |
| Cost analysis                                    | Monthly                    |
| VM patching through Azure Update Manager         | Monthly                    |
| Grafana VM health check                          | Twice per month            |
| Microsoft Defender recommendations               | Regularly                  |
| Azure DevOps pipeline health check               | Regularly                  |
| Sonar token update                               | Every 3 months             |
| Production log archival verification             | November, after the 20th   |
| Storage Account/log-transfer script verification | November, after the 20th   |

---

## 10. Important Operational Guidelines

* Always read alerts carefully before taking any action.
* Do not ignore or miss important alerts.
* Do not ignore pipeline warnings simply because a pipeline has completed successfully.
* Verify backups before performing VM maintenance or updates.
* Do not make impactful changes without informing the Fincart team.
* Monitor Azure costs regularly and proactively identify optimization opportunities.
* Maintain the monthly cost analysis sheet.
* Verify UAT and Stage health before **10:00 AM** every day.
* Check Grafana logs every day.
* Perform Grafana VM health checks twice every month.
* Follow the defined Grafana and Storage Account retention periods.
* Verify the Storage Account and log-transfer script in November after the 20th.
* Update the Sonar token every 3 months.
* Verify the result after every maintenance or configuration change.
* Escalate critical issues promptly.
* Maintain proper records of significant operational changes.
* Never store passwords, API keys, connection strings, tokens, or other secrets directly in this documentation.
