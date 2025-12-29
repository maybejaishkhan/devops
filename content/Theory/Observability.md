The ability to understand the internal state of a system from its external signals.

> **“Can I understand what’s happening inside my system just by looking at its outputs?”**

1. **Telemetry** is the actual data collected from the systems.
2. **Monitoring** is watching specific signals over time and alerting when something breaks or crosses a threshold.
3. **Alerting** is notifying the output.

---

- [The Three Pillars of Observability](#the-three-pillars-of-observability)
- [Monitoring Types \& Methods](#monitoring-types--methods)
- [DevOps Observability Tools](#devops-observability-tools)
- [Advanced Concepts](#advanced-concepts)
  - [Alerting Strategies](#alerting-strategies)
  - [Correlation \& Enrichment](#correlation--enrichment)
  - [Security \& Compliance in Observability](#security--compliance-in-observability)
  - [Best Practices for Implementation](#best-practices-for-implementation)

## The Three Pillars of Observability

We have 3 types of telemetry data that we collect.

1. **Metrics** - Quantitative data like CPU usage, memory, request latency.
2. **Logs** - Textual records of events (with context) which help in figuring out (after an incident) what happened?
3. **Traces** - Show request flow across services (important in microservices)

## Monitoring Types & Methods

| Monitoring Type         | Focus                              | Key Metrics                             |
| :---------------------- | :--------------------------------------------------- | :------------------------------------------------- |
| **Infrastructure** | Health & performance of underlying hardware/VMs.     | Host health, Disk (I/O, space), Memory, CPU, Network utilization. |
| **Application (APM)**| Performance of software applications.                | Code-level performance, Errors, Bottlenecks, Transaction tracing, Throughput, Latency. |
| **Network** | Health & availability of network infrastructure.     | Latency, Packet loss, Connection status, Bandwidth utilization, Traffic flow. |
| **Synthetic** | Proactive simulation of user interactions from external points. | Simulate user journeys, Availability checks (ping, curl), API monitoring. |
| **Real User (RUM)** | Performance data from actual user sessions (frontend). | Page load time, Rendering time, UX metrics, Client-side errors, Geographical performance. |

> We can do monitring with or without an Agent (software installed directly on monitored device).
>
> - **Agent-based Monitoring** is best for important servers, apps and deep insights.
> - **Agentless Monitoring** is best for network devices, basic monitoring and broad coverage.
> - **Hybrid** using Agentless for broad coverage, Agent-based for deep insights on critical systems.
>
> Our monitoring can either be **Blackbox** (simulating external behaviour of a system) vs. **Whitebox** (measuring internal states of the system).

## DevOps Observability Tools

1. **LGTM Stack**
   2. [Loki](/docs/observability/loki.md) for Logs
   3. [Grafana](/docs/observability/grafana.md) for Dashboards
   4. [Tempo](/docs/observability/tempo.md) for Traces
   5. [Prometheus](/docs/observability/prometheus.md) for Metrics
      1. [Mimir](/docs/observability/mimir.md) as Long-term storage
      2. [Alertmanager] for Alerts
6. **ELK Stack**
   7. Elasticsearch for Indexing/Search
   8. Logstash for Data Pipeline
   9. Kibana for Visualization
   10. Beats for Lightweight Agents
11. **All-in-One Observability**
   12. Datadog
   13. New Relic
   14. Dynatrace
   15. Splunk
   16. Sumo Logic
   17. Honeycomb
18. **Cloud Stacks**
   19. AWS
      1. CloudWatch (metrics/logs/alerts)
      2. X-Ray (traces)
      3. CloudFront + Athena (querying)
   20. Azure
      1. Monitor
      2. Application Insights (APM & traces)
      3. Log Analytics (query engine)
   21. GCP
      1. Operations Suite (formerly Stackdriver)
      2. Cloud Trace / Logging / Monitoring
22. **Others**
   23. [OpenTelemetry](/docs/observability/opentelemetry.md) for Instrumentation and Collection standard
   24. [Vector] for Data Pipeline
   25. [InfluxDB] as Time-Series database
   26. [ClickHouse] as Columnar database for Analysis
27. Self-Hosted
   28. SigNoz (open source alternative to Datadog)
   29. Zabbix for legacy infra monitoring
   30. Netdata for system health monitoring
   31. VictoriaMetrics + VictoriaLogs (alternatives to Prometheus and Loki)

## Advanced Concepts

### Alerting Strategies

1. **Static Thresholds** - Trigger alerts when a metric goes over a fixed value.
2. **Rate of Change** - Alert when a value changes quickly over time, even if it's not past a hard threshold yet.
3. **Anomaly Detection** - Use statistical or machine learning models to detect abnormal behavior.
4. **Notification Channels** - Where alerts go once triggered (Slack, Teams, Email, PagerDuty)

> We should avoid alert fatigue: prioritize alerts (critical vs warning)

### Correlation & Enrichment

1. **Log/Metric/Trace Enrichment** - Add context to your telemetry to make it actionable/findable/understandable.
2. **Correlation for Root Cause Analysis (RCA)** - Link telemetry data together like a log error should link to the trace that caused it OR a spike in latency should link to the exact service in the trace path.

### Security & Compliance in Observability

- **Sensitive Data Handling** - Mask PII (emails, tokens, passwords) in logs.
- **Access Control** - Limit who can view logs and metrics.
- **Audit Trails** - Track who accessed what log data or dashboards.
- **Log Retention Policies** - Keep logs: short term: 7–30 days for high volume or Long term: archive to S3/Glacier or BigQuery for audits.

### Best Practices for Implementation

1. *Structured Logging* - Use **JSON or key-value pairs** as it is easy to search and parse in tools.
2. *Centralized Aggregation* - Send logs and metrics to one place for analysis.
3. *Consistent Labeling* - Every metric/log/trace should have standard tags
4. *Instrumentation Strategy* - Use **OpenTelemetry SDKs** (or vendor SDKs like Datadog, New Relic).



# Grafana Stack

| Tool      | What it does                        |
| ---       | ---                                 |
| Grafana   | Dashboards                          |
| Alloy     | Collection + Processing + Exporting |
| Mimir     | Metrics                             |
| Loki      | Logs                                |
| Tempo     | Traces                              |
| Pyroscope | Profiles                            |

This is a very modern and scalable observability stack. That uses FOSS tools made  by Grafana Labs. Alloy + Mimir replaces the need for Prometheus and Alloy itself replaces Otel Collector and Various Exporters.

## Setup

When setting up via Containers. The file structure would look like this:

```tree
grafana-stack/
├── docker-compose.yml
├── .env                        # Environment variables
├── grafana/
│   └── provisioning/
│       ├── dashboards/
│       └── datasources/
│   └── grafana.ini
├── alloy/
│   └── config.alloy            # Main config
│   └── metrics.alloy           # (Optional) Split config
│   └── logs.alloy              # (Optional)
│   └── traces.alloy            # (Optional)
├── loki/
│   └── config.yaml
├── tempo/
│   └── tempo.yaml
├── mimir/                      # Optional (or prometheus/)
│   └── mimir.yaml
└── data/
    ├── grafana/                # Persistent data volumes
    ├── loki/
    ├── tempo/
    └── mimir/
```

# LGTM Stack

Here's the completed table with the correct tools, their purposes, and default ports:

| Tool                    | Purpose                                    | Default Port                                    |
| ----------------------- | ------------------------------------------ | ----------------------------------------------- |
| Grafana                 | Dashboards                                 | 3000                                            |
| Prometheus              | Metrics collection & querying              | 9090                                            |
| Loki                    | Log aggregation                            | 3100                                            |
| Tempo                   | Distributed tracing backend                | 3200                                            |
| Alloy                   | Telemetry pipeline (logs, metrics, traces) | *None (depends on config)*                      |
| OpenTelemetry Collector | Receives and exports telemetry data        | 4317 (gRPC), 4318 (HTTP), 8888 (metrics scrape) |

**Exporters** for Prometheus

- `prom/node-exporter` at `:3100` - For getting linux system metrics.
- `gcr.io/cadvisor/cadvisor` at `:8080` - For getting docker container metrics.

## Setup

### Project Setup

First, create a project folder `mkdir observability && cd observability`.

- Create folders `mkdir -p grafana/provisioning/{datasources,dashboards}`.
  - Create grafana datasources and dashboards files (optional).
- Create config files for each tool (optionally you can also create individual folders). We can name them whatever but just have to be careful when mounting them.
- Create .env for docker compose.

<details>
  <summary><b>Grafana datasources and dashboards yml files</b></summary>

  `datasources.yml` - Tells grafana what datasources to load at startup and how.

  ```yml
  apiVersion: 1

  datasources:
    - name: Prometheus
      type: prometheus
      access: proxy
      url: http://prometheus:9090
      isDefault: true

    - name: Loki
      type: loki
      access: proxy
      url: http://loki:3100

    - name: Tempo
      type: tempo
      access: proxy
      url: http://tempo:3200
  ```

  `dashboards.yml` - Tells grafana where to find dashboard JSON files on disk (to load at startup) and where to place them inside of the Grafana UI.

  ```yml
  apiVersion: 1

  providers:
    - name: 'default'                # Internal name for this provider
      folder: ''                     # Folder name in Grafana UI ('' = General)
      type: file                     # Load dashboards from files
      options:
        path: /etc/grafana/provisioning/dashboards  # Path inside container
        recurse: false

  ```

</details>

<details>
  <summary><b>Prometheus config - <code>prometheus-config.yml</code></b></summary>

  ```yml
  global:
    scrape_interval: 10s

  scrape_configs:
    - job_name: 'prometheus'
      static_configs:
        - targets: ['prometheus:9090']
    - job_name: 'otel-collector'
      static_configs:
        - targets: ['otel-collector:8888']
  ```

</details>

<details>
  <summary><b>Loki config - <code>loki-config.yml</code></b></summary>

  Create loki config file `nano loki-config.yml`

  1. Create folders `mkdir -p grafana/provisioning/{datasources,dashboards}`
  2. (Optional) Create datasource file `datasources.yml`

  ```bash

  ```

</details>

<details>
  <summary><b>Tempo config - <code>tempo-config.yml</code></b></summary>

  1. Create folders `mkdir -p grafana/provisioning/{datasources,dashboards}`
  2. (Optional) Create datasource file `datasources.yml`

  ```bash
  echo "Code block"
  ```

</details>

<details>
  <summary><b>Alloy config - <code>alloy-config.alloy</code></b></summary>
  ```alloy
  ```
</details>

<details>
  <summary><b>OpenTelemetry - <code>otel-config.yml</code></b></summary>

  1. Create folders `mkdir -p grafana/provisioning/{datasources,dashboards}`
  2. (Optional) Create datasource file `datasources.yml`

  ```bash
  echo "Code block"
  ```

</details>

<details>
  <summary><b>Docker Compose <code>.env</code></b></summary>

  ```env
  echo "Code block"
  ```

</details>
