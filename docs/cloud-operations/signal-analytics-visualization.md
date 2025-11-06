# Signal Analytics & Visualization

## 1 · Purpose  
Translate raw telemetry (metrics, logs, traces, events) into actionable insight through repeatable dashboard-creation processes, interactive analytics, and policy-driven alerting.

---

## 2 · Standard Dashboard-Creation Process

| Phase | Tasks | Output |
|-------|-------|--------|
| **1 – Signal Definition** | Map business KPIs to raw signals (CloudWatch metrics, X-Ray traces, Fluent Bit logs). | KPI-Signal matrix (`observability/signals.yaml`). |
| **2 – Data Ingestion** | Create Glue/Athena tables for CUR & logs; enable AMP for Prometheus metrics. | Unified query layer. |
| **3 – Query & Correlation** | • SQL in Athena for cost & usage  <br>• PromQL for infra metrics  <br>• X-Ray Insights for trace anomalies | Saved queries stored in Git (`queries/`). |
| **4 – Dashboard Build** | Use Amazon Managed Grafana panels or QuickSight visuals; apply colour/threshold conventions. | Dashboard JSON definition committed to Git. |
| **5 – Alert Rules** | Create CloudWatch alarms, Grafana alert rules, Budgets; tie to SNS topics feeding Slack & ITSM. | `alerts/` folder with rule JSON/YAML. |
| **6 – Review & Publish** | Peer review via PR; merge triggers CI pipeline to update dashboard via Grafana API. | Version-tagged dashboard in prod workspace. |

---

## 3 · Reference Dashboards

### 3.1 Executive Health Scorecard *(Grafana)*
* **Top Panels** – Overall Error Rate (X-Ray), Cost per 1 k API Calls (Athena), SLA Attainment.  
* **Trend Tabs** – Rolling 12-month spend, capacity vs. quota headroom.

### 3.2 Service Map *(X-Ray ServiceLens)*  
Node graph of micro-service latency and error rates; drill-down opens trace waterfall.

### 3.3 Top Contributors *(QuickSight)*  
| Metric | Insight |
|--------|---------|
| EC2 Spend | Break-down by `InstanceType`; highlights most expensive ten types. |
| Log Volume | 24-h heat-map by `LogGroup`. |
| API Errors | Table by `Path` and `StatusCode` (last 1 h). |

### 3.4 Commitments & Coverage *(Grafana)*  
RI/SP coverage gauge, commitment-vs-usage heat-map, alert if utilisation < 90 % for 7 days.

> *Each dashboard can be exported to JSON and committed to the customer’s Git repository when created; a sample definition is embedded below for reference.*

```json
{
  "title": "Exec Health Scorecard",
  "panels": [
    {
      "type": "stat",
      "title": "Error Rate (%)",
      "fieldConfig": {
        "defaults": {
          "thresholds": {
            "mode": "percentage",
            "steps": [
              { "color": "red", "value": 1 }
            ]
          }
        }
      }
    }
  ]
}
```

---

## 4 · Alert Management

| Alert Source | Condition Example                           | Destination           |
| ------------ | ------------------------------------------- | --------------------- |
| CloudWatch   | `CPUUtilization > 80 % for 15 min`          | Slack #on-call, email |
| Grafana      | `error_rate > 1 % AND latency_p95 > 300 ms` | PagerDuty             |
| Budgets      | Monthly spend > 90 % of forecast            | ServiceNow incident   |

All alerts publish to **SNS topic `finops-alerts`** for audit and replay.

---

## 5 · Dashboard Security & Access Control

| Control                  | Implementation                                                                                      |
| ------------------------ | --------------------------------------------------------------------------------------------------- |
| Workspace isolation      | Separate Grafana workspaces per environment (Prod, Non-prod).                                       |
| IAM Identity Center      | Groups: *Observer*, *Developer*, *Admin*.                                                           |
| Fine-grained permissions | Folder-level rights in Grafana; QuickSight row-level security by `CostCenter`.                      |
| Audit trail              | CloudTrail logs all Grafana API calls; logs stored in S3 Glacier.                                   |
| CI/CD approvals          | Dashboard JSON changes require two-reviewer PR; pipeline deploys via limited “Provisioner” API key. |

---

## 6 · Training & Handover

* **Workshop:** “AWS Observability 101” (2 h)
* **Hands-on Lab:** Instrument a Lambda with OpenTelemetry, build a Grafana alert
* **Runbook:** `runbook-dash-edit.md` – safe edit workflow
* **Office Hours:** Weekly 30-min drop-in for dashboard/alert tuning

---

*Last updated: 30 Jun 2025*

