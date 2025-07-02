# Synthetic Monitoring (Canaries) for Hybrid Workloads

_Last updated: 01 Jul 2025_

---

## 1 · Synthetic Monitoring Methodology

1. **Discovery & Scoping**  
   • Identify critical user journeys (login, checkout, API health).  
   • Choose geographic locations and protocols (HTTP, REST, gRPC).  

2. **Script Development**  
   • Author canary scripts in Node.js or Python using the CloudWatch Synthetics SDK.  
   • Include assertions for status codes, DOM elements, payload contents, and performance thresholds.  
   • Store scripts under `observability/synthetics/` in Git with PR-based reviews.

3. **CI/CD Pipeline Integration**  
   • Build & test canary scripts in CodeBuild: lint, unit–test (mock endpoints), and deploy.  
   • On merge to `main`, deploy or update canaries via CloudFormation or CDK.

4. **Operation & Maintenance**  
   • Schedule canaries to run at business-defined intervals (e.g., every 5 minutes).  
   • Implement auto-rollover of canary versions on script changes.  
   • Rotate IAM roles and API keys every 90 days.

5. **Alerting & Incident Response**  
   • CloudWatch Alarms trigger on canary failures or SLA breaches (e.g., p95 > 1 s).  
   • Alarms → SNS → Slack channel & ServiceNow incident.  
   • Runbook: **`runbook-synthetic-failure.md`** outlines triage (re-run, diff script, escalate).

---

## 2 · Reference Architecture

```text
          +---------------------+
          |  CloudWatch         |
          |  Synthetics Canary  |
          |  (Node/Python SDK)  |
   script └───► runs on schedule  │
                 every 5m         ▼
          +---------------------+      metrics &       +------------------+
          |   Canary Results    |───────────────────────►| CloudWatch       |
          |   (Screenshots,     |                        | Alarms           |
          |    Logs, Metrics)   |                        +------------------+
          +----------┬----------+                                |
                     │ logs/metrics                              ▼
                     ▼                                        +------------------+
          +---------------------+   alerts & incidents       | SNS Topic        |
          |    S3 Canary Logs   |────────────────────────────►| (finops-alerts)  |
          |  (Screenshots, raw  |                              +--------┬---------+
          |   logs)             |                                       |
          +----------┬----------+                                       |
                     │                                                  ▼
                     ▼                                        +-------------------+
          +---------------------+                              | Slack #synthetics |
          |  Athena / Glue      |  ad-hoc analysis            | & ServiceNow API  |
          |  Queries for trends |◄─────────────────────────────| webhook           |
          +---------------------+                              +-------------------+
```

### Component Descriptions

* **CloudWatch Synthetics**
  Runs your canary scripts in chosen Regions, captures screenshots, logs, and performance metrics.

* **S3 Bucket** (`synthetics-logs`)
  Stores raw canary artifacts (screenshots, HAR files, console logs) for forensic analysis.

* **CloudWatch Alarms**
  Trigger on metrics like `FailureCount > 0` or `Duration > threshold`; configured via Alarm DSL.

* **SNS Topic** (`finops-alerts`)
  Fan-out to Slack, ServiceNow, Email; archived for audit.

* **Athena + Glue**
  Crawls the S3 logs daily; provides SQL-driven trend analysis of failure rates and latency.

---

## 3 · Runbook Excerpt (Embedded)


# Runbook – Synthetic Canary Failure

1. **Verify**: In CloudWatch Synthetics console, click “Run details” → view logs & screenshots.  
2. **Re-run**:  
   ```bash
   aws synthetics start-canary --name user-flow-canary
```

3. **Compare**: Diff latest script in Git against `main`—look for recent changes.
4. **Remediate**:

   * If assertion outdated, update script and push through CI/CD.
   * If application issue, open dev ticket with failure snapshot.
5. **Close Alarm** once next run passes; annotate incident in ServiceNow.


---

## 4 · Training & Handover

If needed, ZirconTech can provide a 2-hour workshop on:
- Writing effective canary scripts  
- Integrating canaries into CI/CD  
- Interpreting results & maintaining scripts  

Materials (slides & hands-on labs) are prepared during project kickoff and version-controlled in the customer’s repo.


