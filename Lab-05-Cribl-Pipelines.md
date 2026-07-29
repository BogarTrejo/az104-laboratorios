# Lab 5: Cribl Stream & Data Pipelining (Ingestion, Filtering & Routing)

## 📌 Objective
Design, configure, and optimize enterprise-grade telemetry data pipelines using Cribl Stream to process, filter, enrich, and route logs and metrics efficiently across multi-destination architectures (Splunk, Microsoft Sentinel, and Azure Data Explorer), minimizing storage and ingestion overhead.

## 🛠️ Core Concepts & Architecture
In large-scale enterprise environments (such as semiconductor or cloud operations), raw log volumes can overwhelm analytics backends. Cribl Stream acts as an architectural telemetry broker that allows engineers to:
- **Collect & Ingest:** Receive logs from Universal Forwarders, OpenTelemetry, and Cloud Agents.
- **Transform & Enrich:** Add contextual metadata, parse timestamps, and redact sensitive or redundant data.
- **Filter & Drop (Cost Optimization):** Drop routine health-check logs or debug lines before they reach expensive storage tiers.
- **Route (Multi-Destination):** Direct security-relevant logs to Sentinel and operational application logs to Splunk or ADX based on use case and retention requirements.

## 💻 Practical Pipeline Configuration (Cribl Architecture Simulation)

### 1. Defining a Log Processing Pipeline (`cribl-stream-pipeline.json` simulation)

```json
{
  "id": "enterprise-log-optimization",
  "description": "Filters redundant INFO logs and routes security events to Sentinel while sending operational metrics to Splunk.",
  "functions": [
    {
      "id": "eval",
      "filter": "true",
      "conf": {
        "add": [
          { "name": "processed_by", "value": "'Cribl-Edge-Node-01'" },
          { "name": "environment", "value": "'production'" }
        ]
      }
    },
    {
      "id": "filter",
      "filter": "LogLevel == 'DEBUG' || message contains 'heartbeat_check'",
      "description": "Drop low-value noise logs to optimize license usage and ingestion costs."
    },
    {
      "id": "routing",
      "filter": "security_flag == true",
      "destination": "microsoft-sentinel-siem"
    }
  ]
}

Purpose: Automates preprocessing at the edge/stream layer, ensuring high-cost indexers only receive actionable, high-value security and operational telemetry.

2. Validating Pipeline Routes & Cost Efficiency Metrics

# Check Cribl worker node status and health metrics
cribl status

# Validate pipeline configuration syntax and parser rules
cribl group validate -g default

# Test routing performance against sample log payloads
cribl logstream test --pipeline enterprise-log-optimization --file sample-app-logs.json

Purpose: Ensures cluster stability, low processing latency, and zero data loss during high-volume traffic spikes.

🧠 Key Learnings & Architecture Notes
Cost Mitigation: Intelligent filtering and sampling at the Cribl layer significantly reduce per-gigabyte licensing fees in enterprise SIEMs.

Decoupled Architecture: Separating log collection from final destinations provides flexibility to migrate or dual-route data (e.g., during a Splunk-to-Sentinel migration) without modifying application codebases.