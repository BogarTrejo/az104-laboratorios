# Lab 7: Azure Telemetry, AMA (Azure Monitor Agent) & ADX Architecture

## 📌 Objective
Design and configure a hybrid enterprise telemetry collection and analytics pipeline utilizing the Azure Monitor Agent (AMA), Microsoft Sentinel (SIEM), and Azure Data Explorer (ADX) powered by Kusto Query Language (KQL) for high-performance log analysis.

## 🛠️ Core Concepts & Architecture
In modern multi-cloud and enterprise environments (such as semiconductor engineering operations), efficient log routing and high-speed telemetry analysis are critical. This architecture leverages:
- **Azure Monitor Agent (AMA):** Uses Data Collection Rules (DCRs) to centralize and streamline log/security event collection from virtual machines and hybrid nodes to Microsoft Sentinel.
- **Azure Data Explorer (ADX):** A fully managed, high-performance big data analytics platform designed for real-time querying of massive telemetry volumes using Kusto Query Language (KQL).
- **Hybrid SIEM Integration:** Seamlessly routes security-relevant data to Sentinel while offloading massive operational datasets to ADX for cost-effective, long-term retention and analytics.

## 💻 Practical Configurations & KQL Queries

### 1. Data Collection Rule (DCR) Simulation (`ama-dcr-config.json`)

```json
{
  "location": "eastus",
  "kind": "Linux",
  "properties": {
    "dataSources": {
      "syslog": [
        {
          "name": "enterprise-syslog-stream",
          "streams": ["Microsoft-Syslog"],
          "facilityNames": ["auth", "daemon", "syslog"],
          "logLevels": ["Info", "Notice", "Warning", "Error", "Critical"]
        }
      ],
      "performanceCounters": [
        {
          "name": "CPUAndMemory",
          "streams": ["Microsoft-Perf"],
          "samplingFrequencyInSeconds": 60,
          "counterSpecifiers": [
            "\\Processor(_Total)\\% Processor Time",
            "\\Memory\\Available MBytes"
          ]
        }
      ]
    },
    "destinations": {
      "logAnalytics": [
        {
          "workspaceResourceId": "/subscriptions/sub-id/resourceGroups/rg-observability/providers/Microsoft.OperationalInsights/workspaces/sentinel-workspace",
          "name": "sentinel-destination"
        }
      ]
    }
  }
}
```

Purpose: Configures the modern Azure Monitor Agent (AMA) via a centralized Data Collection Rule (DCR) to selectively harvest Linux system logs and critical performance metrics directly into Microsoft Sentinel.

### 2. High-Performance Telemetry Analysis using KQL (adx-query-optimization.kql)
```kusto
// Analyze high-frequency telemetry spikes and filter anomalous resource consumption
SecurityEvent
| where TimeGenerated > ago(24h)
| where EventID in (4625, 4624) // Failed and successful logon events
| summarize EventCount=count() by TargetAccount, AccountType, bin(TimeGenerated, 1h)
| order by EventCount desc
| take 10
```

Purpose: Utilizes Kusto Query Language (KQL) within Azure Data Explorer / Log Analytics to rapidly isolate security authentication spikes and detect potential lateral movement across infrastructure nodes.

### 3. Validation & Operational Commands

# Verify Azure Monitor Agent status and extension health on a target node
az vm extension show --resource-group rg-observability --vm-name linux-node-01 --name AzureMonitorLinuxAgent

# Test workspace connectivity and log ingestion flow
az monitor data-collection-rule list --resource-group rg-observability

Purpose: Ensures agent persistence, proper extension deployment, and structural integrity of cloud data collection policies.

## 🧠 Key Learnings & Architecture Notes

- Scalable Collection: AMA replaces legacy Log Analytics agents, offering granular stream-level control via DCRs to minimize network overhead.
- High-Speed Analytics: Leveraging KQL in ADX allows engineers to query terabytes of enterprise telemetry in seconds, unblocking security investigations and operational troubleshooting.
