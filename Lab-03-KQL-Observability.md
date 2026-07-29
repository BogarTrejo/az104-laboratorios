# Lab 3: Cloud Observability & Kusto Query Language (KQL)

## 📌 Objective
Learn and apply Kusto Query Language (KQL) fundamentals to analyze, filter, and aggregate structured log data, mimicking enterprise-grade cloud monitoring platforms (such as Azure Monitor and Microsoft Sentinel).

## 🛠️ Concepts & Syntax
KQL operates on tabular data streams using a pipe (`|`) architecture, passing data sequentially from left to right through filter and transformation operators.

**Key Operators Used:**
- **`where`**: Filters a table to a subset of rows that satisfy a predicate.
- **`project`**: Selects specific columns to include, rename, or drop.
- **`summarize`**: Groups rows by specific columns and aggregates data (e.g., using `count()`).

## 💻 Practical KQL Queries Implemented

### 1. Filtering Critical Errors

```kusto
AppLogs
| where LogLevel == "ERROR"
| project Timestamp, ContainerName, Message
```
Purpose: Isolates severe application failures from routine operational logs to accelerate troubleshooting.

### 2. Aggregating Error Counts by Container

```kusto
AppLogs
| where LogLevel == "ERROR"
| summarize TotalErrors = count() by ContainerName
```
Purpose: Groups filtered errors to identify problematic infrastructure components or services at scale.

## 🧠 Key Learnings & Architecture Notes
- **Scalable Telemetry:** KQL allows fast searching across millions of log lines without performance degradation.
- **Proactive Security & Operations:** Grouping and counting log events help establish baseline behaviors and detect anomalous spikes in real-time.
