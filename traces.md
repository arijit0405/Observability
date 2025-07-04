# 🧭 What is Distributed Tracing?

Distributed Tracing is a technique to **track requests as they move through multiple services** in a microservices-based system.

It helps answer:

* Where did the request go?
* Which service was slow?
* Where did the error happen?

## 🔍 Why Do We Need Tracing?

**In monoliths:**

* Logs + Metrics = Enough

**In microservices:**

* A single request might touch 10+ services.
* Logs alone won’t help trace the entire lifecycle of a request.

## 🔧 What is Jaeger?

Jaeger is an **open-source tool for distributed tracing**.

* Originally built by Uber
* Now part of the **CNCF**

## 🧱 Jaeger Architecture Components

| Component        | Purpose                                    |
| ---------------- | ------------------------------------------ |
| Client Libraries | Instrument applications to send trace data |
| Agent            | Receives trace data from services          |
| Collector        | Processes and stores trace data            |
| Storage          | Backend storage (e.g., Elasticsearch)      |
| Query UI         | Web interface to view traces               |

## 🧪 How Tracing Works

* **Trace**: One complete user request across all microservices
* **Span**: Each service call in that trace is one span

**Example:**

```
Trace: "Buy Item"
|- Span 1: frontend
  |- Span 2: auth-service
  |- Span 3: inventory-service
     |- Span 4: db call
```

## 🎯 Example Workflow with Jaeger

1. User hits the frontend
2. Frontend calls auth-service, then inventory-service
3. Each service creates a span
4. Spans are linked into a trace
5. Jaeger displays the trace as a **visual timeline**

## ⚙️ How to Instrument Services

Most languages/frameworks support Jaeger through **OpenTelemetry**.

**Install (Python):**

```bash
pip install opentelemetry-sdk opentelemetry-exporter-jaeger
```

**Example (Python):**

```python
from opentelemetry import trace
from opentelemetry.exporter.jaeger.thrift import JaegerExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor

trace.set_tracer_provider(TracerProvider())
tracer = trace.get_tracer(__name__)

jaeger_exporter = JaegerExporter(
    agent_host_name='localhost',
    agent_port=6831,
)
trace.get_tracer_provider().add_span_processor(
    BatchSpanProcessor(jaeger_exporter)
)

with tracer.start_as_current_span("my-span"):
    print("Doing some work here")
```

## 🚀 Deploying Jaeger in Kubernetes (Minikube)

```bash
kubectl create namespace observability
kubectl apply -n observability -f https://github.com/jaegertracing/jaeger-operator/releases/download/v1.49.0/jaeger-operator.yaml
```

Then create the Jaeger instance:

```yaml
# jaeger.yaml
apiVersion: jaegertracing.io/v1
kind: Jaeger
metadata:
  name: simplest
  namespace: observability
spec:
  strategy: allInOne
```

Access Jaeger UI:

```bash
kubectl port-forward svc/simplest-query -n observability 16686:16686
```

Open: [http://localhost:16686](http://localhost:16686)

## 📊 Benefits of Jaeger

* ✅ Visualizes request flows
* ✅ Detects bottlenecks
* ✅ Helps in root cause analysis
* ✅ Integrates with OpenTelemetry, Prometheus, Grafana

## 🧠 Summary

| Concept         | Description                   |
| --------------- | ----------------------------- |
| Trace           | Complete request journey      |
| Span            | Each step or service call     |
| Jaeger          | Tool to visualize traces      |
| Instrumentation | Code added to generate traces |
