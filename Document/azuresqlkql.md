## 1. Show all executed SQL statements

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_id_s in ("BCM","RCM")
| project TimeGenerated,
          action_name_s,
          server_principal_name_s,
          database_name_s,
          client_ip_s,
          application_name_s,
          statement_s
| order by TimeGenerated desc
```

---

## 2. SELECT statements

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_id_s in ("BCM","RCM")
| where statement_s startswith_cs "SELECT"
| project TimeGenerated,
          server_principal_name_s,
          database_name_s,
          statement_s
| order by TimeGenerated desc
```

---

## 3. INSERT / UPDATE / DELETE

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_id_s in ("BCM","RCM")
| where statement_s matches regex @"^(INSERT|UPDATE|DELETE)"
| project TimeGenerated,
          server_principal_name_s,
          database_name_s,
          statement_s
| order by TimeGenerated desc
```

---

## 4. CREATE / ALTER / DROP

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_id_s in ("BCM","RCM")
| where statement_s matches regex @"^(CREATE|ALTER|DROP)"
| project TimeGenerated,
          server_principal_name_s,
          database_name_s,
          statement_s
| order by TimeGenerated desc
```

---

## 5. Stored Procedure Execution

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_id_s == "RCM"
| project TimeGenerated,
          server_principal_name_s,
          database_name_s,
          statement_s
| order by TimeGenerated desc
```

---

## 6. Queries executed by a specific user

Replace the username as needed.

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where server_principal_name_s == "fincartmainserver"
| where action_id_s in ("BCM","RCM")
| project TimeGenerated,
          statement_s,
          database_name_s
| order by TimeGenerated desc
```

---

## 7. Top users executing SQL

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_id_s in ("BCM","RCM")
| summarize TotalQueries=count() by server_principal_name_s
| order by TotalQueries desc
```

---

## 8. Top applications

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_id_s in ("BCM","RCM")
| summarize TotalQueries=count() by application_name_s
| order by TotalQueries desc
```

---

## 9. Top client IPs

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_id_s in ("BCM","RCM")
| summarize TotalQueries=count() by client_ip_s
| order by TotalQueries desc
```

---

## 10. Failed Logins

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_id_s == "DBAF"
| project TimeGenerated,
          server_principal_name_s,
          client_ip_s,
          application_name_s
| order by TimeGenerated desc
```

## One thing to verify

Before using the filtering queries, run this once:

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_id_s in ("BCM","RCM")
| where isnotempty(statement_s)
| project TimeGenerated, statement_s
| take 20
```
