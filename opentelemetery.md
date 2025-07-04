# 📡 OpenTelemetry (OTel) — Explained

## 🔷 What is OpenTelemetry?

OpenTelemetry (or OTel) is an open-source project by the CNCF that provides a unified standard for collecting telemetry data (logs, metrics, and traces) from applications.

It allows developers and DevOps engineers to instrument their applications and collect:

* ✅ Traces (Distributed Tracing)
* ✅ Metrics (Performance Data)
* ✅ Logs (Structured Logs – in progress)

## 🌎 Why Use OpenTelemetry?

Before OpenTelemetry, teams used:

* **OpenTracing** for traces
* **OpenCensus** for metrics
* **Vendor-specific SDKs** (Datadog, New Relic, etc.)

**OTel combines OpenTracing + OpenCensus into ONE vendor-neutral standard.**

## 🧱 OpenTelemetry Architecture

| Component                 | Description                                         |
| ------------------------- | --------------------------------------------------- |
| Instrumentation Libraries | Code added to your app to generate telemetry        |
| SDKs                      | APIs to manage spans, metrics, etc.                 |
| Collectors                | Services that receive telemetry data and export it  |
| Exporters                 | Send data to Prometheus, Jaeger, Grafana, etc.      |
| Backends                  | Final destinations (e.g. Grafana, Datadog, Elastic) |

## 📊 What Does OTel Collect?

| Signal Type | What it Tracks                         | Example Tool |
| ----------- | -------------------------------------- | ------------ |
| Traces      | Lifecycle of a request across services | Jaeger       |
| Metrics     | CPU, Memory, HTTP requests, etc.       | Prometheus   |
| Logs        | (In progress) Structured logs & errors | Loki, ELK    |

## 🚀 How It Works

1. Instrument your code using OpenTelemetry SDK
2. Collect telemetry using the OpenTelemetry Collector
3. Export to tools like:

   * Jaeger (traces)
   * Prometheus (metrics)
   * Grafana (dashboards)
   * Loki (logs)
   * Cloud providers (AWS X-Ray, Azure Monitor, etc.)

## 💻 Supported Languages

* ✅ Python
* ✅ Java
* ✅ Node.js
* ✅ Go
* ✅ .NET
* ✅ Ruby
* ✅ PHP
* ✅ C++

## ⚖️ Example: Python + OpenTelemetry + Jaeger

### Installation

```bash
pip install opentelemetry-api opentelemetry-sdk
pip install opentelemetry-exporter-jaeger
```

### Instrumenting the Code

```python
from opentelemetry import trace
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
from opentelemetry.exporter.jaeger.thrift import JaegerExporter

trace.set_tracer_provider(TracerProvider())
tracer = trace.get_tracer(__name__)

jaeger_exporter = JaegerExporter(
    agent_host_name='localhost',
    agent_port=6831,
)
trace.get_tracer_provider().add_span_processor(
    BatchSpanProcessor(jaeger_exporter)
)

with tracer.start_as_current_span("main-task"):
    print("📦 Doing something important...")
```

## 🏗️ OpenTelemetry Collector

The collector is a standalone service that receives telemetry from your app and routes it to backends.

* **Receiver**: Gets data (e.g., OTLP, Jaeger, Prometheus)
* **Processor**: Modifies data (batching, filtering, etc.)
* **Exporter**: Sends to external systems

```yaml
# Example OTel collector config
treceivers:
  otlp:
    protocols:
      grpc:

exporters:
  logging:

service:
  pipelines:
    traces:
      receivers: [otlp]
      exporters: [logging]
```

## 🧠 Key Benefits

* ✅ Vendor-neutral standard
* ✅ One unified SDK for all telemetry
* ✅ Easily integrates with observability tools
* ✅ Cloud-native and Kubernetes-friendly
* ✅ Backed by CNCF (like Kubernetes)

## 🚰 Common Use Cases

* Microservices tracing (with Jaeger)
* App performance monitoring (with Prometheus/Grafana)
* Collect and route logs (via FluentBit/OTel logs)
* Monitor CI/CD pipelines
* Alerting and incident response workflows

## ⚖️ OpenTelemetry vs Prometheus vs Jaeger

| Feature        | OpenTelemetry                         | Prometheus   | Jaeger      |
| -------------- | ------------------------------------- | ------------ | ----------- |
| Scope          | All telemetry (traces, metrics, logs) | Metrics only | Traces only |
| Vendor-neutral | ✅                                     | ✅            | ✅           |
| Standard       | ✅ (W3C-compliant)                     | ❌            | ❌           |
| Pluggable      | ✅ (export anywhere)                   | Somewhat     | Jaeger only |
