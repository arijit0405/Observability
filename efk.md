# 📘 Logging and EFK Stack in DevOps

## 🧾 What is Logging?
Logging is the practice of **recording information about an application's runtime behavior**. These logs are essential for:

- Debugging errors
- Auditing activity
- Monitoring system health
- Understanding application usage

## 🧱 The EFK Stack
The EFK Stack is a powerful **centralized logging solution**. It consists of:

| Component      | Purpose                                 |
|----------------|------------------------------------------|
| Elasticsearch  | Stores logs and enables fast full-text search |
| Fluent Bit     | Lightweight log forwarder/collector (agent) |
| Kibana         | Visualizes logs with dashboards and search |

## 🧰 How Does It Work?
```
Application Logs --> Fluent Bit --> Elasticsearch --> Kibana
```
- Fluent Bit collects logs from files or stdout.
- It parses and filters logs.
- Then it forwards them to Elasticsearch.
- Kibana visualizes the logs stored in Elasticsearch.

## 🔄 EFK vs ELK Stack
| EFK Stack                  | ELK Stack                         |
|---------------------------|------------------------------------|
| Fluent Bit (lightweight)  | Logstash (heavier)                |
| More suitable for Kubernetes | Used in traditional environments |
| Better performance & low resource usage | Flexible but more resource-intensive |

## 🚀 Fluent Bit in Kubernetes
- Deployed as a **DaemonSet** (1 pod per node)
- Collects logs from `/var/log/containers/*.log`
- Parses and forwards logs to Elasticsearch

**Example Config:**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: fluent-bit-config
data:
  fluent-bit.conf: |
    [SERVICE]
        Flush        1
        Log_Level    info

    [INPUT]
        Name              tail
        Path              /var/log/containers/*.log
        Parser            docker
        Tag               kube.*

    [OUTPUT]
        Name  es
        Match *
        Host  elasticsearch-logging
        Port  9200
        Index fluentbit
        Type  _doc
```

## 📦 Elasticsearch
Elasticsearch is a **distributed, REST-based search engine**:
- Stores **JSON-based logs**
- Supports **indexing, searching, filtering** logs in real-time

**Common Elasticsearch Concepts:**
- **Index**: Like a database
- **Document**: Like a row (JSON log)
- **Shard**: Unit of scale
- **Cluster**: Group of Elasticsearch nodes

## 📊 Kibana
Kibana is a **web UI for log analysis**:
- Dashboard creation
- Filtering, searching logs visually
- Uses same **index names** from Elasticsearch

## 🔐 Benefits of EFK Stack
- ✅ Centralized log aggregation
- ✅ Real-time monitoring
- ✅ Search, filter, analyze logs
- ✅ Integrates well with Kubernetes
- ✅ Lightweight & scalable (Fluent Bit)

## 🧪 Example Use Case in Kubernetes
1. Deploy application
2. Deploy Fluent Bit as DaemonSet
3. Deploy Elasticsearch as StatefulSet
4. Deploy Kibana as Deployment
5. Access Kibana UI → view logs in real time

## 🧠 Summary
| Component      | Role                          |
|----------------|-------------------------------|
| Fluent Bit     | Collects & forwards logs      |
| Elasticsearch  | Stores and indexes logs       |
| Kibana         | UI for viewing and analyzing  |
