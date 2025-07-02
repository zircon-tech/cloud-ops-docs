# Observability for Modern Application Architectures  
*Containers & Serverless Practices*

_Last updated : 30 Jun 2025_

---

## 1 · Containers Observability Practice

### 1.1 Reference Architecture

```text
                     +-----------------------------+
  EKS / ECS Cluster  |  Node / Pod Telemetry       |
  ──────────────────▶|  • cAdvisor / kubelet       |
                     |  • AWS Distro OTEL Agent    |
                     +--------------┬--------------+
                                    | metrics / traces
                     +--------------v--------------+
                     | Amazon Managed Prometheus   |
                     |  (AMP)                      |
                     +--------------┬--------------+
                                    | PromQL
                     +--------------v--------------+
                     | Amazon Managed Grafana      |
                     +--------------┬--------------+
  logs (Fluent Bit)  |              |
       +-------------v-----+        |
       | CloudWatch Logs    |<------+  alerts via SNS →  Slack / OpsGenie
       +-------------^-----+
                     | control-plane logs (CloudTrail, AWS Health)
                     |
             +-------v-------+
             |   S3 / Athena |  ad-hoc SQL / top-N analysis
             +---------------+
```

### 1.2 Process & Tooling

| Phase                      | Activities                                                                         | Outputs                    |
| -------------------------- | ---------------------------------------------------------------------------------- | -------------------------- |
| **Assessment**             | *eksctl* / *kubectl* inventory, map workloads, capture resource limits, DaemonSets | Cluster assessment doc     |
| **Instrumentation**        | Deploy AWS Distro for OTEL sidecar/DaemonSet, Fluent Bit for logs                  | OTEL manifest, Helm chart  |
| **Aggregation & Storage**  | Metrics → AMP, Logs → CW Logs, Traces → X-Ray                                      | Central data lake          |
| **Visualization**          | Provision Grafana dashboards (resource, service, golden KPIs)                      | Dashboard JSON in Git      |
| **Alerting**               | Grafana alert rules, CloudWatch alarms (e.g., pod OOM, restart loops)              | SNS → Slack + ServiceNow   |
| **Correlation / RCA**      | Use Grafana Explore + X-Ray service map; drill-down to pod/container               | Runbook “K8s Incident RCA” |
| **Continuous Improvement** | Weekly review of top contributor panels, cust-action backlog                       | Optimisation tickets       |

*Best-of-breed* add-ons: PromLens (PromQL tutor), Loki (if OpenSearch not desired), Datadog or New Relic on request.

---

## 2 · Serverless Observability Practice

### 2.1 Reference Architecture

```text
 ┌───────────────┐    logs/traces/metrics     ┌────────────────────┐
 │ API Gateway   │───────────────┐            │ AWS Lambda Powertools│
 └───────────────┘               │            └─────────┬───────────┘
          ▼                      │ OTEL SDK             │
 ┌───────────────┐               │              +-------v--------+
 │  Lambda Fn A  │───────────────┘              |   AWS X-Ray    |
 └───────────────┘                              +-------+--------+
          ▼ logs (CW)                               traces and insights
 ┌───────────────┐                              +-------v--------+
 │ StepFunctions │  execution logs              | CloudWatch     |
 └───────────────┘  + metrics (Insights)        |  Logs & Metrics|
          ▼                                       +-------+------+
 ┌───────────────┐  DynamoDB Streams   logs      | Lambda Insights|
 │ DynamoDB      │─────────────────────┘        +-------+--------+
 └───────────────┘                                       |
           ^  cost & usage (Athena)                      |
           └──────────┬──────────────────────────────────┘
                      ▼
              QuickSight / Grafana
          (KPIs, latency, cost per tx)
```

### 2.2 Process & Tooling

| Requirement           | Implementation                                                                                                     |
| --------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Assessment**        | SAM inventory script lists all Lambda functions, API GW stages, Step Functions; tags each with `Owner`, `KPI`.     |
| **Metrics & Logs**    | CloudWatch Lambda Insights, Function URLs metrics; Powertools logger adds structured JSON.                         |
| **Traces**            | OTEL or embedded X-Ray SDK, ServiceLens map for latency/error drill-down.                                          |
| **Event Correlation** | CloudWatch Log Insights queries join `@requestId` across API GW, Lambda, DynamoDB Streams; Grafana Tempo optional. |
| **Alerts**            | Lambda-specific CloudWatch alarms (p95 duration, error > 1 %), Budget alert for invocation spend.                  |
| **Visualization**     | QuickSight dashboard “Serverless Health” – invocation count, cold start %, cost per thousand calls.                |
| **Best-of-breed**     | Lumigo or Datadog for deep payload tracing when required.                                                          |

---

## 3 · Security & Access for Dashboards

| Control                          | Containers                              | Serverless                           |
| -------------------------------- | --------------------------------------- | ------------------------------------ |
| Workspace isolation              | Grafana folders per cluster/environment | QuickSight namespaces (Prod vs Dev)  |
| IAM SSO RBAC                     | “Observer”, “Dev”, “Admin” groups       | Same groups via IAM Identity Center  |
| Row-level / stream-level masking | IRSA with limited Prometheus RBAC rules | CW Logs subscription filter per tag  |
| Audit trail                      | CloudTrail + Grafana API logs           | CloudTrail + QuickSight session logs |

---

## 4 · Deliverables

* Cluster or workload **Observability Assessment Report** (Markdown).
* Terraform / Helm modules for OTEL Collector, Fluent Bit, Lambda Powertools layer.
* **Grafana/QuickSight dashboards** (JSON or template) with golden KPIs.
* Alert rule set (CloudWatch & Grafana).
* Runbooks:

  * `runbook-container-oom.md` – container crash debug
  * `runbook-lambda-timeout.md` – serverless latency hot-path

---

## 5 · Training & Enablement (Optional)

If the customer needs hands-on guidance, ZirconTech prepares a custom half-day enablement session covering:

* OTEL instrumentation for EKS and Lambda
* Building Grafana/QuickSight panels from collected signals
* Alert tuning and incident runbooks

Materials (slides, lab guide, sample repo) are created during project planning and stored in the customer’s Git repo for future reference.
