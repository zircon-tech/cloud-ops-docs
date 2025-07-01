# Monitoring & Observability Baseline

## 1 · Purpose  
Provide a repeatable framework to define KPIs, collect end-to-end telemetry (logs, metrics, traces, events), and surface actionable insights for AWS workloads from infrastructure to application level.

---

## 2 · Methodology – KPI & SLO Definition

| Step | Activity | Output |
|------|----------|--------|
| 1. Business Mapping | Workshop with product owner & ops to list user journeys | Draft KPI list (e.g., checkout latency < 200 ms) |
| 2. Technical Mapping | Decompose workload into services; map each KPI to metric/log/trace | KPI ↔ telemetry matrix |
| 3. SLO Draft | Define targets & error budgets (e.g., 99.9 % availability, ≤ 1 % 5xx) | SLO document |
| 4. Instrumentation Plan | Decide agents, SDKs, log formats, sampling rates | Instrumentation backlog |
| 5. Review & Sign-off | Stakeholders approve; goals stored in Git `observability/kpis.yaml` | Baseline locked |

---

## 3 · Reference Architecture


```
               +--------------------------------------------------+
               |  AWS Control-Plane Telemetry                     |
               |  • CloudTrail  • AWS Health  • Service Quotas    |
               +------------------------+-------------------------+
                                        |
                                        v
+--------------------------+   metrics   +-------------------------+
|  CloudWatch Logs &       |<-----------►|  Amazon Managed         |
|  CloudWatch Metrics      |             |  Prometheus (AMP)       |
+------------+-------------+             +------------+------------+
             | logs/metrics                            | metrics
             v                                         v
+-------------------------+   traces   +---------------------------+
| OpenTelemetry Collector |----------► |        AWS X-Ray         |
+------------+------------+            +------------+--------------+
             | traces/logs                          |
             |                                      v
             |                          +---------------------------+
             |                          | Amazon Managed Grafana    |
             |                          +------------+--------------+
             |                                       |
             |  alerts (SNS)                         v
             +-------------------------------> Slack / ServiceNow /
                                                 Email / OpsGenie


```

### Description of Key Components

| Layer | AWS Service(s) | Optional 3rd-Party |
|-------|---------------|--------------------|
| **Control-plane telemetry** | CloudTrail Lake, AWS Health API, Service Quotas | — |
| **Infra metrics** | CloudWatch, CloudWatch Agent | Prometheus node_exporter |
| **Application telemetry** | OpenTelemetry Collector to AWS X-Ray | Datadog, New Relic |
| **Logs** | CloudWatch Logs, Fluent Bit (EKS/ECS) | ElasticSearch / OpenSearch |
| **Synthetic/RUM** | CloudWatch Synthetics, RUM | Pingdom, Catchpoint |
| **Dashboards** | Amazon Managed Grafana, QuickSight | Grafana Cloud, Kibana |
| **Alerting** | CloudWatch Alarms + SNS → Email, Slack, ServiceNow | OpsGenie, PagerDuty |

---

## 4 · Metrics, Logs, Traces Mapping (Sample)

| KPI / SLO | Telemetry | Threshold |
|-----------|-----------|-----------|
| Checkout latency P95 < 200 ms | X-Ray segment `CheckoutService` + CW metric `Latency` | Alarm at 180 ms |
| EC2 CPU Util < 80 % | CW metric `CPUUtilization` | Alarm at 75 % 5-min average |
| Error budget 0.1 % | X-Ray error rate | Alert on 0.08 % |
| Quota utilization | Service Quotas metric | Alert on 80 % quota |

---

## 5 · Runbook Excerpts (embedded)

### 5.1 “Investigate High Error Rate”

1. Open Grafana panel **API Errors** (link).  
2. Correlate X-Ray trace IDs to service pods in EKS.  
3. Check recent deploy in CodeDeploy; rollback if < 30 m ago.  
4. Document RCA in Jira ticket.

### 5.2 “Quota Breach Mitigation”

```bash
aws service-quotas request-service-quota-increase \
  --service-code ec2 --quota-code L-1216C47A \
  --desired-value 500
```

Notify CloudOps Slack channel.

---

## 6 · Training & Handover

* **Workshop:** “AWS Observability 101” (2 h)
* **Labs:** Instrument a Lambda with OTEL; build Grafana alert.

---

*Last updated: 30 Jun 2025*
