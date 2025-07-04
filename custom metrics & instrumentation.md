# 📀 What is Instrumentation?

Instrumentation is the process of **adding code to your application to expose internal state as metrics**.

In simple terms, it's how your application talks to monitoring tools by producing metrics like **request counts**, **latency**, **errors**, etc.

## 🛠️ Why is it Needed?

Infrastructure tools (Prometheus, Grafana) **can't know about your application logic unless you tell them**.

Custom metrics let you monitor business logic, like:

* Number of items added to cart
* Average response time per endpoint
* User signup failures
* Custom latency buckets

## 📊 What are Custom Metrics?

Custom metrics are **user-defined metrics** created through instrumentation.

These are different from system metrics (like CPU, memory) provided by system exporters like Node Exporter.

**Example:**

```python
from prometheus_client import Counter

REQUEST_COUNT = Counter('my_app_requests_total', 'Total number of requests')

def my_handler():
    REQUEST_COUNT.inc()
```

Here, `my_app_requests_total` is a **custom metric** exposed via `/metrics` and scraped by Prometheus.

## 🔍 Types of Custom Metrics

| Type      | Description                              | Use Case                   |
| --------- | ---------------------------------------- | -------------------------- |
| Counter   | Monotonically increasing value           | Request count, error count |
| Gauge     | Value that can go up or down             | Memory usage, active users |
| Histogram | Measure and bucket observed values       | Request durations, sizes   |
| Summary   | Similar to histogram, includes quantiles | Latency quantiles          |

## 🧠 Instrumenting Applications

### ⚙️ Python Example (using `prometheus_client`)

```python
from prometheus_client import start_http_server, Summary
import random
import time

REQUEST_TIME = Summary('request_processing_seconds', 'Time spent processing request')

@REQUEST_TIME.time()
def process_request():
    time.sleep(random.random())

if __name__ == '__main__':
    start_http_server(8000)
    while True:
        process_request()
```

* Prometheus scrapes metrics from `localhost:8000/metrics`
* Metric exposed: `request_processing_seconds`

## 🚀 Custom Metrics in Kubernetes

In Kubernetes, these metrics are:

* Exposed via application pods
* Scraped via **ServiceMonitors** (from `kube-prometheus-stack`)
* Queried using **PromQL**
* Visualized via **Grafana dashboards**

**Example:** Track request latency per pod and auto-scale based on custom metrics using **HPA**.

## 📈 Visualization in Grafana

You can visualize custom metrics with:

```promql
rate(my_app_requests_total[1m])
```

Monitor latency buckets:

```promql
histogram_quantile(0.9, rate(http_request_duration_seconds_bucket[5m]))
```

## ✅ Best Practices

* Name your metrics using Prometheus naming conventions (e.g., `my_app_metric_total`)
* Always add **labels** (e.g., `endpoint`, `status_code`) for filtering
* Export `/metrics` endpoint in your app
* Use libraries for your stack:

  * **Python**: `prometheus_client`
  * **Node.js**: `prom-client`
  * **Go**: `prometheus/client_golang`
  * **Java**: `micrometer`, `simpleclient`

## 🧠 Summary

| Concept         | Description                                      |
| --------------- | ------------------------------------------------ |
| Instrumentation | Code that exposes metrics                        |
| Custom Metrics  | Business/application-level metrics               |
| Why Needed      | To monitor app internals like latency/errors     |
| How to Expose   | Prometheus client libraries and `/metrics` route |
