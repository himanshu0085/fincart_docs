## 1. Successful Login

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_name_s == "DATABASE AUTHENTICATION SUCCEEDED"
| project TimeGenerated,
          server_principal_name_s,
          database_name_s,
          client_ip_s,
          application_name_s,
          host_name_s
| order by TimeGenerated desc
```

---

## 2. Failed Login

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_name_s == "DATABASE AUTHENTICATION FAILED"
| project TimeGenerated,
          server_principal_name_s,
          database_name_s,
          client_ip_s,
          application_name_s,
          host_name_s
| order by TimeGenerated desc
```

---

## 3. Login Summary by User

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_name_s contains "AUTHENTICATION"
| summarize TotalLogins=count() by server_principal_name_s
| order by TotalLogins desc
```

---

## 4. Activity by User

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| summarize TotalEvents=count() by server_principal_name_s
| order by TotalEvents desc
```

---

## 5. Activity by Client IP

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| summarize TotalEvents=count() by client_ip_s
| order by TotalEvents desc
```

---

## 6. JDBC / Application Usage

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| summarize TotalEvents=count() by application_name_s
| order by TotalEvents desc
```

---

## 7. Database-wise Activity

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| summarize TotalEvents=count() by database_name_s
| order by TotalEvents desc
```

---

## 8. Authentication Events Only

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where action_name_s contains "AUTHENTICATION"
| project TimeGenerated,
          action_name_s,
          server_principal_name_s,
          database_name_s,
          client_ip_s
| order by TimeGenerated desc
```

---

## 9. Audit Events in Last 24 Hours

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| where TimeGenerated >= ago(24h)
| summarize TotalEvents=count() by action_name_s
| order by TotalEvents desc
```

---

## 10. Show Different Audit Actions

```kusto
AzureDiagnostics
| where Category == "SQLSecurityAuditEvents"
| summarize Count=count() by action_name_s
| order by Count desc
```
