# Azure SQL Database Monitoring Setup

This document outlines the step-by-step configuration of monitoring for Azure SQL Database, including enabling diagnostic settings, configuring alerts, and best practices.

---

## **1. Configure Diagnostic Settings for Azure SQL Database**

### Steps:
1. Navigate to the **Azure SQL Database** resource in the Azure Portal.
2. Under the **Monitoring** section, click on **Diagnostic settings**.
3. Add a new diagnostic setting:
   - **Log Categories**:
     - `SQLInsights`: Captures query performance insights.
     - `Errors`: Tracks error details.
     - `DatabaseWaitStatistics`: Analyzes resource contention.
     - `Deadlocks`: Helps diagnose concurrency issues and omprove design.
     -  `timeout`: Useful for identifying poorly optimazed queries or overloaded resourecs.
   - **Destination**:
     - **Log Analytics Workspace**: Stores and allows querying logs.
4. Save the configuration and verify it by generating database activity and checking logs in the Log Analytics workspace.

**Image Placeholder:**
![Diagnostic Settings Configuration](path/to/your/image1.png)

---

## **2. Configure Alerts for Azure SQL Database**

### Steps:
1. Go to **Azure Monitor** in the Azure Portal.
2. Select **Alerts > New Alert Rule**.
3. Configure the alert rule:
   - **Scope**: Choose the Azure SQL Database resource.
   - **Condition**:
     - Metric: For example, `CPU percentage` greater than 80% for 5 minutes.
     - Other metrics: `DTU Consumption`, `Storage Usage`, `Deadlocks`.
   - **Action Group**:
     - Name: `prod-sql-monitor-actiongrp`.
     - Actions: Email, SMS, or integrations like ServiceNow.
   - **Alert Rule**:
     - Name: `prod-sql-alert-cpu-high`.
     - Severity: Assign levels based on importance (e.g., 0 for critical).
4. Save and test the alert rule.

**Image Placeholder:**
![Alert Configuration](path/to/your/image2.png)

---

## **3. Analyze Logs in Log Analytics Workspace**

### Steps:
1. Open the **Log Analytics Workspace** linked to your SQL Database.
2. Use Kusto Query Language (KQL) for analysis. Example queries:
   - **Long-running queries**:
     ```kql
     AzureDiagnostics
     | where ResourceType == "SQLDatabase"
     | where EventCategory == "SQLInsights" and DurationMs > 1000
     | project TimeGenerated, DatabaseName, DurationMs, Statement
     ```
   - **Errors**:
     ```kql
     AzureDiagnostics
     | where ResourceType == "SQLDatabase"
     | where EventCategory == "Errors"
     | project TimeGenerated, DatabaseName, ErrorNumber, Severity, Message
     ```
3. Save frequent queries as dashboards for easy access.

**Image Placeholder:**
![Log Analytics Workspace](path/to/your/image3.png)

---

## **4. Application Insights for SQL Database Performance**

### Steps:
1. Navigate to **Application Insights** associated with the app service.
2. Under **Performance**, review SQL dependency calls.
3. Set alerts in Application Insights for:
   - Dependency failures.
   - High response times.
4. Use **Transaction Search** to trace specific database interactions.

**Image Placeholder:**
![Application Insights Performance](path/to/your/image4.png)

---

## **5. Naming Conventions**

- **Diagnostic Settings**: `prod-sql-db-diagnostic-01`
- **Alerts**: `prod-sql-alert-[metric]-[severity]`
- **Action Groups**: `prod-sql-monitor-actiongrp`
- **Log Analytics Workspace**: `prod-log-analytics-sql-01`


---

