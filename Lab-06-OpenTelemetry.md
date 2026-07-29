# Lab 6: OpenTelemetry (OTEL) Collector & Telemetry Standardization

## 📌 Objective
Design and configure an OpenTelemetry (OTEL) Collector pipeline architecture to standardize telemetry collection (metrics, logs, and traces) across multi-vendor cloud environments, establishing a vendor-neutral observability foundation.

## 🛠️ Core Concepts & Architecture
Modern enterprise infrastructures (such as semiconductor or cloud operations) require decoupled, standardized telemetry layers. OpenTelemetry solves vendor lock-in by providing:
- **Unified Collectors:** Receivers that accept data in various formats (OTLP, Jaeger, Prometheus, Syslog).
- **Processors:** Batching, memory limiters, and filtering to sanitize and optimize performance before export.
- **Exporters:** Multi-backend routing capabilities to forward telemetry seamlessly to Splunk, Microsoft Sentinel, or Azure Data Explorer (ADX).

## 💻 Practical OTEL Configuration (`otel-collector-config.yaml`)

### 1. Collector Pipeline Setup

```yaml
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318

processors:
  memory_limiter:
    check_interval: 1s
    limit_percentage: 80
    spike_limit_percentage: 20
  batch:
    send_batch_size: 1024
    timeout: 5s

exporters:
  otlp/splunk:
    endpoint: "splunk-indexer.internal:4317"
    tls:
      insecure: false
  azuremonitor:
    connection_string: "InstrumentationKey=your-instrumentation-key"

service:
  pipelines:
    traces:
      receivers: [otlp]
      processors: [memory_limiter, batch]
      exporters: [otlp/splunk]
    metrics:
      receivers: [otlp]
      processors: [batch]
      exporters: [azuremonitor]
```
Purpose: Configures an enterprise-grade collector instance to ingest OTLP traffic safely, manage memory pressure during traffic spikes, and split metrics/traces to separate operational platforms.

### 2. Validation & Health Check Commands

# Validate OpenTelemetry Collector configuration syntax
otelcol-contrib --config=otel-collector-config.yaml --validate

# Run collector locally in debug mode to inspect incoming telemetry payloads
otelcol-contrib --config=otel-collector-config.yaml --set=service.telemetry.logs.level=debug

Purpose: Ensures strict syntax accuracy and enables real-time troubleshooting for distributed tracing nodes.

## 🧠 Key Learnings & Architecture Notes

- Vendor Agnosticism: Standardizes instrumentation at the application level, eliminating dependency on proprietary SDKs.
- Resource Protection: Implementing memory limiters prevents collector crashes during high-volume telemetry surges from intensive workloads.
