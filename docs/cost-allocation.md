# Cost Allocation, Measurement & Accountability

## 1 · Purpose
Give customers full visibility into AWS spend—down to individual workloads and business metrics—so they can optimize, budget, and hold teams accountable.

---

## 2 · Capabilities Overview

| Requirement (per checklist) | How ZirconTech Enables It |
|-----------------------------|---------------------------|
| **Filter / group costs by org, account, resource, tag, category, region** | • Enable AWS Cost Explorer + **Cost Categories**  <br>• Enforce standardized **tagging strategy** (`CostCenter`, `Project`, `Team`)  <br>• Publish curated Athena views (`vw_ce_daily`, `vw_ce_monthly`) |
| **Create unit economics** | • Join Cost & Usage Report (CUR) with business KPIs (e.g., requests, MAU) in Athena  <br>• QuickSight dashboards show cost per 1 k API calls, per customer, per feature  |
| **Allocate shared-service costs** | • Define **allocation rules** in Cost Categories (“Shared-Infra”, “LogArchive”)  <br>• Pro-rate via Athena query or third-party FinOps tools  |
| **Granular CUR / usage reporting** | • CUR delivered to S3 daily with resource IDs  <br>• Glue crawler → Athena tables → QuickSight / Tableau exports  |

---

## 3 · Methodology & Process

### 3.1 Discovery
1. Workshop with Finance, DevOps, and BU leads  
2. Identify shared services (logging, VPN, Transit Gateway)  
3. Gather business drivers for unit metrics (transactions, seats, GB stored)

### 3.2 Foundation
| Step | Action | Tool |
|------|--------|------|
| 1 | Turn on **Cost Explorer**, **CUR** (hourly, resource-level) | Billing Console |
| 2 | Implement tagging baseline via SCP + Tag Policies | AWS Organizations |
| 3 | Create **Cost Categories** (< 50 rules) | Cost Explorer API |
| 4 | Build **Glue crawler** & partitioned Athena external table | Glue, Athena |
| 5 | Publish initial QuickSight dashboard “All-Up Spend” | QuickSight |

### 3.3 Unit Economics Pipeline
```

CUR (S3) ─► Glue Crawler ─► Athena View `vw_cost`
│
Business KPIs (S3 / RDS) ──────────┘
│
└──► Athena View `vw_unit_cost` ─► QuickSight “Cost per KPI”

```

### 3.4 Shared-Cost Allocation
* **Rule 1 – % Split**: `TransitGateway` cost split 40 % Prod, 60 % Non-Prod  
* **Rule 2 – Tag Inherit**: `SharedServices` account charges distributed by `CostCenter` tag weights  
* **Rule 3 – Fixed Fee**: Security tooling charged at flat monthly rate to “Security & Compliance” cost center

Rules implemented in Athena or FinOps SaaS; validation report generated monthly.

### 3.5 Reporting & Accountability
| Report | Cadence | Audience | Actionable KPI |
|--------|---------|----------|----------------|
| Exec Summary PDF | Monthly | C-suite | Total AWS spend vs. budget |
| BU Chargeback CSV | Monthly | Finance, BU heads | Cost by CostCenter |
| Unit-Cost Dashboard | Daily | Product owners | $ / 1 k API calls |
| Anomaly Alerts | Near-real-time | FinOps team | >20 % spike YoY or MoM |

---

## 4 · Tooling Stack

| Layer | Default AWS Service | Optional / Third-Party |
|-------|--------------------|-------------------------|
| **Cost ingestion** | Cost & Usage Report (CUR) | N/A |
| **Data lake / query** | S3 + Glue + Athena | Databricks, Snowflake |
| **Dashboards** | AWS QuickSight | Tableau, Power BI |
| **FinOps SaaS** | — | Third-party FinOps platforms |
| **Tag enforcement** | Tag Policies, SCPs | Infrastructure as Code validation hooks |
| **Anomaly detection** | Cost Anomaly Detection, Budgets alerts | Third-party cost management tools |

---

## 5 · Roles & Responsibilities

| Role | Key Tasks |
|------|-----------|
| **FinOps Analyst** | Maintain tag dictionaries, review anomalies, run chargeback |
| **DevOps Lead** | Ensure workloads propagate `Environment`, `Project` tags |
| **Finance Manager** | Approve shared-cost allocation rules, reconcile invoices |
| **Business Owner** | Monitor unit-cost dashboard, drive optimizations |

---

## 6 · Deliverables

1. **Cost Allocation Runbook** (Markdown + PDF)  
2. **Tag Dictionary** and **Cost Category Rule Set** (JSON)  
3. **Glue + Athena** CloudFormation stack templates  
4. **QuickSight dashboard link** + export template (CSV, PDF)  
5. **Monthly chargeback report** sample

---

_Last updated: 30 Jun 2025_

