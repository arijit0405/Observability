# 📊 Metrics, Monitoring, and Prometheus

## 📌 What are Metrics?

Metrics are **quantitative measurements** collected over time. In a system or application, they can include:

* CPU usage
* Memory usage
* Disk I/O
* Network traffic
* Application-specific data (e.g., HTTP requests/sec, error rates)

These metrics help engineers monitor the **health**, **performance**, and **availability** of systems.

## 📈 What is Monitoring?

Monitoring is the practice of **collecting**, **processing**, and **analyzing** data (metrics/logs) to:

* Ensure system health
* Detect anomalies
* Alert in case of failures or performance issues
* Support debugging and root-cause analysis

**Types of Monitoring:**

* **Infrastructure Monitoring** (e.g., CPU, memory)
* **Application Monitoring** (e.g., response time, error rates)
* **Network Monitoring** (e.g., bandwidth, packet loss)

## 🔧 What is Prometheus?

Prometheus is an **open-source monitoring and alerting toolkit** designed for **reliability and scalability**.

### 🔹 Key Features:

* Pull-based model for metrics collection
* Multidimensional time-series data model
* PromQL (Prometheus Query Language)
* Built-in alert manager
* Visualization support with Grafana

## 📦 Prometheus Architecture

```
+-------------+      +-------------+
|  Target(s)  | ---> | Prometheus  |
| (e.g., App) |      |  Server     |
+-------------+      +-------------+
                         |
                         v
                 +---------------+
                 | AlertManager  |
                 +---------------+
                         |
                         v
                      Alerts
                         |
                         v
                     Grafana UI
```

## 🔍 How Prometheus Works

* Targets (applications, services) expose metrics at `/metrics` endpoint
* Prometheus **scrapes** data at regular intervals
* Stores metrics in **time-series database**
* Supports **PromQL** for querying metrics
* Can trigger alerts based on defined rules
* Works with **Grafana** for visual dashboards

## 🚀 Example Prometheus Configuration (`prometheus.yml`)

```yaml
scrape_configs:
  - job_name: 'my-app'
    static_configs:
      - targets: ['localhost:8080']
```

## ⚠️ Alerting with Prometheus

Using alerting rules, Prometheus can:

* Detect when CPU > 90%
* Trigger an alert to Alertmanager
* Send notifications via Email, Slack, PagerDuty, etc.

```yaml
groups:
- name: example
  rules:
  - alert: HighCPUUsage
    expr: process_cpu_seconds_total > 90
    for: 2m
    labels:
      severity: warning
    annotations:
      summary: "High CPU Usage detected"
```

## 📊 Visualization with Grafana

Prometheus integrates seamlessly with **Grafana** to:

* Create dashboards
* Visualize real-time and historical data
* Use PromQL queries for charts

## ✅ Benefits of Prometheus

* Easy to set up
* Powerful query language
* Scalable and modular
* Works well with Kubernetes
* Strong community and ecosystem

## 🧠 Summary

| Concept    | Description                               |
| ---------- | ----------------------------------------- |
| Metrics    | Numeric measurements of system behavior   |
| Monitoring | Observing system to detect issues         |
| Prometheus | Tool for collecting and analyzing metrics |

Prometheus is the go-to solution for modern **observability** and is often paired with **Grafana** for dashboards and **AlertManager** for alerting.
