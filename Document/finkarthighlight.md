# Fincart DevOps – Key Highlights

### September 2025 – May 2026

Over the past year, the DevOps team supported Fincart across Cloud Infrastructure, DevOps Automation, Monitoring, Security, Database Management, and Cost Optimization initiatives to improve platform reliability, operational efficiency, and scalability.

---

# Infrastructure & Cost Optimization

### Log Analytics Workspace Optimization

* Analyzed Log Analytics Workspace and identified excessive log ingestion across applications.
* Optimized logging strategy for Assets, CMS, and LMS applications by removing unnecessary logs while maintaining monitoring and troubleshooting capabilities.
* Improved monitoring efficiency and reduced overall Log Analytics costs.

### App Service Consolidation & Rightsizing

* Migrated fincart-core and additional production services to fincart-prod-linux-app-plan-003.
* Consolidated workloads to improve resource utilization and application performance.
* Successfully downgraded ASP-FincartResourcesIndia-BFF4 from Premium v3 P3V3 to Premium v3 P2V3 after migration validation.
* Reduced infrastructure costs through App Service optimization and rightsizing.

### Kafka Infrastructure Optimization

* Decommissioned the UAT Confluent Kafka cluster after validating business dependencies.
* Consolidated environment configurations and removed redundant infrastructure.
* Simplified platform management and reduced recurring cloud costs.

### Database Cost Optimization

* Optimized SQL Database resource utilization and overall database infrastructure costs.
* Reduced PostgreSQL and MySQL infrastructure costs through continuous utilization review and optimization activities.
* Improved database efficiency while maintaining performance and availability.

### Cost Governance & Monitoring

* Conducted regular cloud cost reviews and optimization exercises.
* Configured cost anomaly alerts and monthly cost reporting.
* Continuously monitored Azure resource utilization and identified optimization opportunities.

### Estimated Monthly Cost Savings

| Initiative                              | Description                                                                                                                                      | Estimated Monthly Savings |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------- |
| Kafka / Confluent Optimization          | Decommissioned and optimized Kafka infrastructure                                                                                                | ₹39,000                   |
| Log Analytics Optimization              | Reduced unnecessary log ingestion and optimized monitoring costs                                                                                 | ₹31,000                   |
| App Service Consolidation & Rightsizing | Migrated fincart-core and additional services to fincart-prod-linux-app-plan-003 and downgraded ASP-FincartResourcesIndia-BFF4 from P3V3 to P2V3 | ₹19,920                   |
| SQL Database Optimization               | Optimized SQL Database resource utilization and costs                                                                                            | ₹6,230                    |
| PostgreSQL & MySQL Optimization         | Optimized database infrastructure and resource consumption                                                                                       | ₹5,879                    |

---

# Database Setup, Migration & Reliability

### PostgreSQL Platform Enablement

* Provisioned Azure PostgreSQL environments for Production and Stage.
* Executed database backup and restoration activities.
* Configured monitoring and alerting for PostgreSQL environments.

### Database Connectivity Improvements

* Removed dependency on local host file entries for database connectivity.
* Improved Production and UAT database connectivity architecture.
* Established secure VPN connectivity for Portfolio database access.

### Database Monitoring & Governance

* Implemented User Activity Monitoring for databases.
* Configured database alerts and monitoring dashboards.
* Performed Redgate Monitor proof of concept for enhanced database observability.
* Supported critical database activities during maintenance windows and production support periods.

---

# Monitoring & Observability

### Centralized Monitoring Platform

* Built a centralized monitoring solution using Grafana, OpenTelemetry (OTEL), and Loki.
* Configured secure access through Azure AD Single Sign-On.

### Unified Monitoring Dashboards

* Created centralized dashboards for UAT, Stage, and Production environments.
* Enabled monitoring of application health, CPU utilization, memory utilization, response times, and database metrics.

### Service Availability Monitoring

* Implemented automated service health monitoring and service-down alerting.
* Improved incident detection and operational visibility across environments.

### OpenTelemetry Adoption

* Enabled OpenTelemetry across UAT microservices.
* Improved application observability through centralized logs, traces, and metrics.

### New Relic Integration

* Integrated key applications and databases with New Relic.
* Expanded application performance monitoring capabilities.

### Infrastructure Monitoring

* Configured VM monitoring alerts for CPU, memory, and disk utilization.
* Implemented log retention policies and monitoring reports.

---

# Security & Compliance

### SSL Certificate Management

* Renewed and managed SSL certificates across Production and UAT applications.
* Ensured uninterrupted secure communication and compliance.

### Compliance & Audit Support

* Prepared and maintained monthly compliance documentation covering:

  * Patch Management
  * Backup Verification
  * Encryption Controls
  * MFA Validation
  * Firewall Reviews
  * NSG Reviews
  * Audit Logs

### Network Security Improvements

* Reviewed and optimized Azure NSG configurations.
* Restricted UAT environment access to VPN and internal network users.
* Improved overall network security posture.

### User Access Governance

* Conducted access reviews and removed inactive users.
* Managed user access lifecycle and governance activities.

### Security Assessment

* Evaluated Microsoft Defender capabilities and reviewed security posture.
* Conducted Cyber Security Checklist reviews and documented remediation actions.

---

# DevOps Automation & Platform Engineering

### CI/CD Improvements

* Optimized CI/CD pipelines and reduced build execution time.
* Implemented automated deployment strategies for UAT environments.
* Created deployment and operational SOPs.

### Infrastructure Automation

* Developed startup and shutdown automation pipelines.
* Implemented scale-down automation for non-production environments.
* Reduced manual operational effort through automation.

### Azure CDN Migration

* Successfully migrated Azure CDN Classic workloads to Azure CDN Standard.

### OS Patch Management

* Performed regular OS patching for critical infrastructure.
* Enabled Azure Update Manager for automated patch management.

### SFTP Platform Setup

* Created and configured Azure-based SFTP infrastructure.
* Implemented operational controls to optimize usage and costs.

### Storage & Data Management

* Managed Azure Storage Accounts and containers.
* Implemented object replication and data migration activities.

### OneAssure Production Setup

* Provisioned Production infrastructure components, Key Vault, SSL certificates, and required configurations for OneAssure.

---

# SonarQube & Code Quality

### SonarQube Compatibility & Scanning Resolution

* Resolved SonarQube branch and pull request scanning issues.
* Addressed compatibility challenges between the existing SonarQube version and third-party analysis plugins.
* Ensured uninterrupted code quality and security analysis processes.
* Documented SonarQube integration and scanning workflows for the development team.

---

# New Initiatives & Project Support

### New Application Enablement

* Provisioned infrastructure and CI/CD pipelines for new applications and services.
* Supported onboarding of restructuring project applications and microservices.

### Branching & Release Support

* Established deployment workflows and QA branching strategies.
* Performed branch synchronization activities across environments.

### Azure DevOps Reporting

* Developed Azure DevOps code review and pull request reporting capabilities.

### VPN Evaluation & Network Improvements

* Performed NetBird VPN proof-of-concept and validated network routing scenarios for future enhancements.

---

# Summary

Throughout the year, the DevOps team focused on improving platform reliability, monitoring, security, automation, database governance, and cloud cost optimization. Key initiatives included infrastructure optimization, centralized observability, security enhancements, application hosting optimization, database modernization, operational automation, and proactive monitoring. These efforts helped improve system stability, strengthen governance controls, optimize cloud resource utilization, and support the continued growth of the Fincart platform.
