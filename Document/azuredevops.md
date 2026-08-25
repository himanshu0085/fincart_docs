# Azure DevOps Project Inventory & KT Documentation

## 1. Purpose

This document provides an overview of the Azure DevOps organization, including all available projects, their known purpose, repositories, pipelines, infrastructure resources, branching policies, and work-item management.

The objective is to help the new team understand the Azure DevOps organization and identify which project should be used for a particular development, DevOps, infrastructure, or operational activity.

---

# 2. Azure DevOps Organization

The Azure DevOps organization currently contains **17 projects**.

## 2.1 Project Inventory

|  # | Project Name                    | Known Purpose / Description                                          |
| -: | ------------------------------- | -------------------------------------------------------------------- |
|  1 | Android_Ios_APP                 | Application development                                              |
|  2 | Development And Restructing     | New development and restructuring activities                         |
|  3 | Devops                          | DevOps-related activities                                            |
|  4 | Fincart AI Projects             | AI projects, including AI repository and pipeline                    |
|  5 | Fincart Dashboard (Web App)     | New UI/UX for existing web/mobile application                        |
|  6 | Fincart Python Projects         | Python services, scrapers, and related projects                      |
|  7 | Fincart_MF_Database             | Fincart Mutual Fund database-related activities                      |
|  8 | Fincart_Product                 | Product thinking and design activities                               |
|  9 | Maintenance Board               | Restructuring and internal support tickets                           |
| 10 | Production Bugs                 | Production bug tracking                                              |
| 11 | QA-Bug-Tracker                  | QA and bug tracking                                                  |
| 12 | Restructuring Project           | Backend restructuring and architecture-related activities            |
| 13 | Tech and Development FY-2025-26 | Technology and development activities for FY 2025-26                 |
| 14 | Tech and Development FY-2026-27 | Technology and development activities for FY 2026-27                 |
| 15 | UI Restructuring Project        | Restructuring of UI as microservices                                 |
| 16 | Web_APP                         | Web application                                                      |
| 17 | Workpoint                       | DevOps, IaC, automation, environment management, and sprint tracking |

---

# 3. Restructuring Project

The **Restructuring Project** contains the backend application repository and deployment pipelines.

## 3.1 Backend Repository

The project contains the **backend repository** used for:

* Backend application source code
* Application development
* Code changes and maintenance
* Pull Requests
* Code reviews
* Version control
* Integration with deployment pipelines

## 3.2 Application Pipelines

The backend pipelines are separated by environment:

* UAT
* Stage
* Production

### UAT Pipeline

Used to deploy the backend application to UAT.

### Stage Pipeline

Used to deploy the backend application to Stage.

### Production Pipeline

Used to deploy the backend application to Production.

Production deployments must be handled carefully because changes can directly impact the live environment.

---

# 4. Workpoint

The **Workpoint** project is used for DevOps, infrastructure, automation, environment management, and sprint/work-item tracking.

## 4.1 DevOps Repository

The project contains a **DevOps repository** for DevOps-related configuration and operational resources.

## 4.2 Infrastructure as Code

The Workpoint project contains IaC resources, including:

* Terraform
* Infrastructure configuration
* Automation scripts

Terraform is used to manage Azure infrastructure through code.

## 4.3 Automation Scripts

Automation scripts are maintained in Workpoint and are used for infrastructure and environment-related activities.

Before changing automation scripts, understand their dependencies and potential impact.

## 4.4 UAT and Stage Scale-Up/Scale-Down

Workpoint contains pipelines for managing UAT and Stage environments.

These include:

* UAT Scale-Up
* UAT Scale-Down
* Stage Scale-Up
* Stage Scale-Down

Both automated and manual pipelines are available.

### Scale-Up

Used when the required environment resources need to be brought online or increased.

### Scale-Down

Used when the environment is no longer required and resources can be reduced.

Always verify the target environment before executing these pipelines.

## 4.5 Sprint and Work Items

Sprint tickets should be created and tracked in the **Workpoint** project.

For each work item:

* Provide a clear title.
* Add a detailed description.
* Add acceptance criteria where applicable.
* Assign the ticket to the appropriate person/team.
* Associate it with the correct sprint.
* Add priority where required.
* Link related work items where applicable.

---

# 5. Fincart AI Projects

The **Fincart AI Projects** project is used for AI-related development.

## 5.1 AI Repository

The project contains an **AI repository** used for:

* AI application development
* AI-related source code
* Feature development
* Code maintenance
* Pull Requests
* Code reviews

## 5.2 AI Pipeline

The project contains an **AI pipeline** associated with the AI repository.

The pipeline should be checked for:

* Successful execution
* Build/deployment status
* Errors
* Warnings
* Repository integration
* Authentication or configuration issues

---

# 6. Other Projects

The following projects are present in the organization. Their detailed repository, pipeline, branching, and deployment structure should be explored and documented during KT rather than assumed.

## 6.1 Android_Ios_APP

Purpose: Application development.

**KT action:** Explore repositories, pipelines, branches, environments, and deployment process.

## 6.2 Development And Restructing

Purpose: New development and restructuring activities.

**KT action:** Explore repositories, pipelines, work items, and branching policies.

## 6.3 Devops

Purpose: DevOps-related activities.

**KT action:** Explore repositories, pipelines, automation, service connections, and infrastructure responsibilities.

## 6.4 Fincart Dashboard (Web App)

Purpose: New UI/UX work for the existing web/mobile application.

**KT action:** Explore repositories, pipelines, environments, and branching policies.

## 6.5 Fincart Python Projects

Purpose: Python services, scrapers, and related projects.

**KT action:** Explore repositories, pipelines, scheduled jobs, and branching policies.

## 6.6 Fincart_MF_Database

Purpose: Mutual Fund database-related activities.

**KT action:** Explore repositories, database scripts, pipelines, and deployment process.

## 6.7 Fincart_Product

Purpose: Product thinking and product design activities.

**KT action:** Explore boards, work items, repositories, and documentation.

## 6.8 Maintenance Board

Purpose: Tracking restructuring and internal support tickets.

**KT action:** Understand the ticketing process, board configuration, and relationship with other projects.

## 6.9 Production Bugs

Purpose: Production bug tracking.

**KT action:** Understand bug creation, assignment, prioritization, and resolution process.

## 6.10 QA-Bug-Tracker

Purpose: QA and bug tracking.

**KT action:** Understand QA workflow, bug lifecycle, assignments, and sprint association.

## 6.11 Tech and Development FY-2025-26

Purpose: Technology and development activities for FY-2025-26.

**KT action:** Explore active work items, repositories, pipelines, and current usage.

## 6.12 Tech and Development FY-2026-27

Purpose: Technology and development activities for FY-2026-27.

**KT action:** Explore active work items, repositories, pipelines, and current usage.

## 6.13 UI Restructuring Project

Purpose: Restructuring the UI as microservices.

**KT action:** Explore repositories, architecture, pipelines, environments, and branching policies.

## 6.14 Web_APP

Purpose: Web application development.

**KT action:** Explore repositories, pipelines, environments, and deployment process.

---

# 7. Branching Strategy and Branching Policy

The branching strategy must be reviewed for each repository before making code changes.

Do not assume that all projects use the same branching model.

## 7.1 Branching Policy Review

For each relevant repository:

1. Go to **Repos → Branches**.
2. Review the existing branches.
3. Identify the default/main branch.
4. Identify feature, development, release, UAT, Stage, or Production branches where applicable.
5. Review branch naming conventions.
6. Open the branch policy configuration.
7. Check Pull Request requirements.
8. Check minimum reviewer requirements.
9. Check build validation requirements.
10. Check linked work-item requirements.
11. Check comment-resolution requirements.
12. Review branch permissions and restrictions.

## 7.2 Pull Request Process

Before merging code:

1. Create a branch according to the repository's existing convention.
2. Make the required changes.
3. Commit and push the changes.
4. Create a Pull Request.
5. Link the relevant work item.
6. Add required reviewers.
7. Ensure all branch-policy checks pass.
8. Resolve review comments.
9. Obtain required approvals.
10. Merge the Pull Request according to the repository policy.

## 7.3 Important Branching Guidelines

* Do not bypass an existing branch policy without authorization.
* Do not directly push to a protected branch when Pull Requests are required.
* Follow the repository-specific branching convention.
* Review branch policies before making repository changes.
* Document the actual branch structure during KT.
* Inform the Fincart team before changing branch policies.

---

# 8. Pipeline Management

For every important pipeline, the following should be documented during KT:

* Pipeline name
* Purpose
* Source repository
* Trigger type
* Target environment
* Required parameters
* Service connections
* Approvals
* Deployment process
* Rollback process
* Common failures
* Troubleshooting steps

Always verify the target environment before running a deployment or infrastructure pipeline.

---

# 9. High-Level Azure DevOps Structure

```text
Azure DevOps
│
├── Android_Ios_APP
│
├── Development And Restructing
│
├── Devops
│
├── Fincart AI Projects
│   ├── AI Repository
│   └── AI Pipeline
│
├── Fincart Dashboard (Web App)
│
├── Fincart Python Projects
│
├── Fincart_MF_Database
│
├── Fincart_Product
│
├── Maintenance Board
│
├── Production Bugs
│
├── QA-Bug-Tracker
│
├── Restructuring Project
│   ├── Backend Repository
│   └── Application Pipelines
│       ├── UAT
│       ├── Stage
│       └── Production
│
├── Tech and Development FY-2025-26
│
├── Tech and Development FY-2026-27
│
├── UI Restructuring Project
│
├── Web_APP
│
└── Workpoint
    ├── DevOps Repository
    ├── IaC
    │   ├── Terraform
    │   └── Automation Scripts
    ├── UAT Scale-Up
    ├── UAT Scale-Down
    ├── Stage Scale-Up
    ├── Stage Scale-Down
    └── Work Items / Sprints
```

---

# 10. Project Quick Reference

| Project                         | Confirmed Details                                                         |
| ------------------------------- | ------------------------------------------------------------------------- |
| Android_Ios_APP                 | Application development                                                   |
| Development And Restructing     | Development and restructuring                                             |
| Devops                          | DevOps activities                                                         |
| Fincart AI Projects             | AI repository + AI pipeline                                               |
| Fincart Dashboard (Web App)     | Web/mobile UI/UX                                                          |
| Fincart Python Projects         | Python services + scrapers                                                |
| Fincart_MF_Database             | MF database activities                                                    |
| Fincart_Product                 | Product design/thinking                                                   |
| Maintenance Board               | Restructuring + internal support tickets                                  |
| Production Bugs                 | Production bug tracking                                                   |
| QA-Bug-Tracker                  | QA/bug tracking                                                           |
| Restructuring Project           | Backend repo + UAT/Stage/Production pipelines                             |
| Tech and Development FY-2025-26 | Technology/development activities                                         |
| Tech and Development FY-2026-27 | Technology/development activities                                         |
| UI Restructuring Project        | UI restructuring as microservices                                         |
| Web_APP                         | Web application                                                           |
| Workpoint                       | DevOps + IaC + Terraform + automation + scale-up/down + sprint work items |

---

# 11. Change Management

Before making changes to repositories, branches, branch policies, pipelines, Terraform, or automation scripts:

1. Understand the requirement.
2. Identify the Azure DevOps project.
3. Identify the repository or pipeline.
4. Identify the target environment.
5. Review existing configuration and policies.
6. Check potential impact.
7. Inform the Fincart team before impactful changes.
8. Obtain required confirmation/approval where applicable.
9. Make the change.
10. Test and validate.
11. Monitor the pipeline/deployment.
12. Verify the environment.
13. Document the change where required.

---

# 12. Important Operational Guidelines

* Always verify the correct Azure DevOps project before performing any activity.
* Do not assume that all projects use the same repository or branching strategy.
* Explore the repository and branch policy before making code changes.
* Always verify the target environment before executing a pipeline.
* Be especially careful with Production deployments.
* Review pipeline warnings and errors.
* Inform the Fincart team before impactful pipeline, infrastructure, or branch-policy changes.
* Track sprint work through Azure DevOps work items in Workpoint.
* Do not store passwords, tokens, API keys, connection strings, or other secrets directly in this documentation.
