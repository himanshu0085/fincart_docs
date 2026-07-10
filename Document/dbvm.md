---

# Secure VM Solution Proposal for Stage & Production Database Access

**Version:** 1.0

**Prepared By:** DevOps Team

**Purpose:** Design Proposal for Secure Database Access using Azure Virtual Machine, NetBird VPN and Audit Logging

---

# 1. Objective

The objective of this proposal is to provide a secure Azure Virtual Machine (VM) through which authorized users can access the Stage and Production SQL Server databases.

The proposed solution aims to:

* Provide secure access to Stage & Production databases.
* Allow only one active user session at a time.
* Provide centralized access through a dedicated VM.
* Record complete audit logs.
* Track VPN login/logout activity.
* Record SQL Server access and executed queries.
* Support security monitoring and compliance requirements.

---

# 2. Existing Environment

Currently, users access the Stage and Production databases directly from their local systems using NetBird VPN.

```text
User Laptop
      │
NetBird VPN
      │
      ▼
Stage / Production SQL Server
```

## Current Challenges

* Database access is performed directly from user systems.
* No centralized jump server.
* Limited centralized audit trail.
* Difficult to correlate user activity with database operations.

---

# 3. Proposed Architecture

```text
                User Laptop
                     │
          Remote Desktop (RDP/xRDP)
                     │
                     ▼
           Azure Virtual Machine
                     │
            NetBird VPN Login
                     │
                     ▼
      Stage / Production SQL Server
```

---

# 4. Proposed Solution

Two implementation options have been evaluated.

---

# Option A – Ubuntu 22.04 LTS

## VM Configuration

| Component        | Configuration                                     |
| ---------------- | ------------------------------------------------- |
| Cloud            | Microsoft Azure                                   |
| Operating System | Ubuntu 22.04 LTS (Canonical)                      |
| VM Size          | Standard D2as_v5                                  |
| CPU              | 2 vCPU                                            |
| Memory           | 8 GB RAM                                          |
| Storage          | 128 GB Standard SSD                               |
| Remote Access    | xRDP                                              |
| GUI              | Ubuntu Desktop (to be installed after deployment) |
| VPN              | NetBird                                           |
| Database Client  | DBeaver                                           |

---

## User Workflow

1. User connects to Ubuntu VM using xRDP.
2. User logs in using assigned Linux credentials.
3. User logs in to NetBird using individual VPN credentials.
4. VPN connection is established.
5. User opens DBeaver.
6. User connects to Stage/Production SQL Server.
7. User executes required SQL queries.
8. User disconnects NetBird and logs out from the VM.

---

## Monthly Cost

| Resource                        |          Monthly Cost |
| ------------------------------- | --------------------: |
| Ubuntu VM                       |             ₹3,376.71 |
| 128 GB Standard SSD             |               ₹878.54 |
| Public IP                       |               ₹303.66 |
| Estimated Outbound Data (10 GB) |                ₹91.51 |
| **Estimated Total**             | **₹4,650.42 / Month** |

---

## Advantages

* Lower infrastructure cost.
* Official Canonical image.
* Fully compatible with NetBird.
* DBeaver supported.
* Complete audit logging can be implemented.

---

## Limitations

* Ubuntu Desktop GUI must be installed after VM deployment.
* SQL Server Management Studio (SSMS) is not supported on Linux.
* Database access will be performed using DBeaver.

---

# Option B – Windows Server 2025

## VM Configuration

| Component        | Configuration                                |
| ---------------- | -------------------------------------------- |
| Cloud            | Microsoft Azure                              |
| Operating System | Windows Server 2025 Datacenter               |
| VM Size          | Standard D2as_v5                             |
| CPU              | 2 vCPU                                       |
| Memory           | 8 GB RAM                                     |
| Storage          | 128 GB Standard SSD                          |
| Remote Access    | RDP                                          |
| GUI              | Available by default                         |
| VPN              | NetBird                                      |
| Database Client  | SQL Server Management Studio (SSMS), DBeaver |

---

## User Workflow

1. User connects to Windows VM using RDP.
2. User logs in using assigned Windows credentials.
3. User logs in to NetBird using individual VPN credentials.
4. VPN connection is established.
5. User opens SSMS or DBeaver.
6. User connects to Stage/Production SQL Server.
7. User executes required SQL queries.
8. User disconnects NetBird and logs out from the VM.

---

## Monthly Cost

| Resource                        |           Monthly Cost |
| ------------------------------- | ---------------------: |
| Windows VM                      |              ₹8,988.35 |
| 128 GB Standard SSD             |                ₹878.54 |
| Public IP                       |                ₹303.66 |
| Estimated Outbound Data (10 GB) |                 ₹91.51 |
| **Estimated Total**             | **₹10,262.06 / Month** |

---

## Advantages

* Native Windows GUI.
* Full support for SQL Server Management Studio (SSMS).
* Minimal post-deployment configuration.
* Familiar Windows environment.

---

## Limitations

* Higher infrastructure cost due to Windows licensing.

---

# 5. User Authentication

Each authorized user will be assigned:

* Individual VM login credentials.
* Individual NetBird VPN credentials.

### Authentication Flow

```
VM Login
      │
NetBird Login
      │
VPN Connected
      │
DBeaver / SSMS
      │
SQL Server
```

> **Note:** SSH access (Linux) will be restricted to administrators for system administration only. End users will access the VM through the graphical desktop (RDP/xRDP).

---

# 6. Audit Strategy

The proposed solution provides audit logging at three levels.

| Audit Layer      | Information Captured                                                                      |
| ---------------- | ----------------------------------------------------------------------------------------- |
| VM Logs          | Username, Login Time, Logout Time, Session Duration                                       |
| NetBird Logs     | VPN Login, Logout, Connection Duration                                                    |
| SQL Server Audit | Database Login, Database Access, Executed Queries, INSERT, UPDATE, DELETE, DDL Operations |

---

## End-to-End Audit Flow

```
VM Login
     │
NetBird Login
     │
VPN Connected
     │
DBeaver / SSMS
     │
Execute SQL Queries
     │
SQL Audit
     │
Disconnect VPN
     │
VM Logout
```

---

# 7. Security Controls

* Individual VM user accounts.
* Individual NetBird VPN accounts.
* One active user session at a time.
* Database access only through NetBird VPN.
* Centralized audit logging.
* SQL Server Audit enabled.
* Administrative access restricted to authorized administrators.

---

# 8. Assumptions

* Azure subscription is available.
* NetBird Management Server is already deployed.
* SQL Server is accessible through NetBird VPN.
* Individual NetBird user accounts are available.
* SQL Server Audit will be enabled.
* Only one concurrent user session will be permitted.

---

# 9. Estimated Implementation Effort

**Estimated Implementation Duration:** **1–2 Working Days**

The implementation includes:

* Azure VM provisioning
* Operating system configuration
* NetBird installation and configuration
* SQL client installation
* User account configuration
* Audit logging configuration
* Testing and validation

---

# 10. Feature Comparison

| Feature       | Ubuntu                                      | Windows             |
| ------------- | ------------------------------------------- | ------------------- |
| GUI           | Ubuntu Desktop (Installed after deployment) | Native GUI          |
| SQL Client    | DBeaver                                     | SSMS + DBeaver      |
| NetBird       | Supported                                   | Supported           |
| Audit Logging | Supported                                   | Supported           |
| Monthly Cost  | **₹4,650**                                  | **₹10,262**         |
| Best Fit      | Lower Infrastructure Cost                   | Native SSMS Support |

---

# 11. Recommendation

## Option A – Ubuntu 22.04 LTS

Recommended when:

* Lower infrastructure cost is the priority.
* DBeaver is acceptable as the SQL client.

**Estimated Cost:** **₹4,650/month**

---

## Option B – Windows Server 2025

Recommended when:

* SQL Server Management Studio (SSMS) is required.
* A native Windows environment is preferred.

**Estimated Cost:** **₹10,262/month**

---
