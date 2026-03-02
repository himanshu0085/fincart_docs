# Stage Azure SQL Database Monitoring - SOP (Tabular Format)

------------------------------------------------------------------------

## 1. Document Information

  Field              Details
  ------------------ -------------------------
  Environment        Stage
  Database Name      FINSTAGE
  SQL Server         stagefincart
  Resource Group     fincart_stage_resources
  Monitoring Tool    New Relic
  Integration Type   Azure Monitor Metrics

------------------------------------------------------------------------

## 2. Integration Steps

  -----------------------------------------------------------------------
  Step                    Description
  ----------------------- -----------------------------------------------
  Step 1                  Navigate to Infrastructure → Azure in New Relic

  Step 2                  Click "Add an Azure account" and select Azure
                          Monitor Metrics

  Step 3                  Provide Subscription ID, Tenant ID, Client ID,
                          Client Secret

  Step 4                  Assign Reader & Monitoring Reader roles to
                          Service Principal

  Step 5                  Enable "Limit to Resource Group" →
                          fincart_stage_resources

  Step 6                  Enable Azure SQL Database metrics and Save
                          configuration

  Step 7                  Verify connection status shows Connected
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 3. Validation Queries

  --------------------------------------------------------------------------------------
  Validation Type                            NRQL Query
  ------------------------------------------ -------------------------------------------
  CPU Validation                             SELECT \* FROM Metric WHERE metricName =
                                             'azure.sql.servers.databases.cpu_percent'
                                             SINCE 30 minutes ago LIMIT 10

  Storage Validation                         SELECT \* FROM Metric WHERE metricName =
                                             'azure.sql.servers.databases.storage' SINCE
                                             30 minutes ago LIMIT 10
  --------------------------------------------------------------------------------------

------------------------------------------------------------------------

## 4. Dashboard Queries (Stage Monitoring)

  ----------------------------------------------------------------------------------------------
  Metric                        NRQL Query
  ----------------------------- ----------------------------------------------------------------
  Storage (GB)                  SELECT
                                latest(azure.sql.servers.databases.storage.sum)/1024/1024/1024
                                FROM Metric SINCE 30 minutes ago FACET entity.name

  CPU %                         SELECT latest(azure.sql.servers.databases.cpu_percent) FROM
                                Metric SINCE 30 minutes ago FACET entity.name

  Physical Data Read %          SELECT
                                latest(azure.sql.servers.databases.physical_data_read_percent)
                                FROM Metric SINCE 30 minutes ago FACET entity.name

  Failed Connections            SELECT latest(azure.sql.servers.databases.connection_failed)
                                FROM Metric SINCE 30 minutes ago FACET entity.name
  ----------------------------------------------------------------------------------------------

------------------------------------------------------------------------

## 5. Recommended Alerts

  Metric               Threshold   Duration
  -------------------- ----------- ------------
  CPU %                \> 80%      5 minutes
  Storage Usage        \> 85%      10 minutes
  Failed Connections   \> 5        5 minutes

------------------------------------------------------------------------

## 6. Troubleshooting Checklist

  Checkpoint           Action
  -------------------- --------------------------------------------------
  Azure Subscription   Verify correct Stage subscription is connected
  Resource Group       Confirm filter includes fincart_stage_resources
  Polling Interval     Ensure polling is active (5 minutes recommended)
  Time Range           Use Last 30 minutes or 3 hours
  Metrics Enabled      Ensure Azure SQL Database metrics are enabled

------------------------------------------------------------------------

End of Document
