# Cost Reporting & Visualization

## 1 · Purpose  
Deliver a self-service reporting stack that measures, monitors, and drives accountability for cloud spend—from a single AWS account up to a consolidated, multi-account organization.

---

## 2 · Architecture Overview

```

CUR (S3, hourly, resource-level)
│
▼
Glue Crawler ─► Athena External Table ─►  Views (`vw_daily`, `vw_ttm`)
│                                      │
▼                                      ▼
Tag Enrichment Lambda (ensures tag purity)   QuickSight Dashboards

```

* **Cost & Usage Report (CUR)** delivered daily to S3 with resource IDs.  
* **Glue + Athena** provide SQL access across all linked accounts.  
* **Tag Enrichment Lambda** auto-adds missing `CostCenter`, `Project` tags for showback.  
* **QuickSight** dashboards visualize spend, forecast, and anomalies.

---

## 3 · Capability Matrix

| Checklist Capability | ZirconTech Implementation |
|----------------------|---------------------------|
| **Measure & monitor spend** | Athena views `vw_daily_cost` & `vw_ttm_cost`; QuickSight “Org-Wide Spend” dashboard with filters by Account, OU, Region |
| **Account-to-Org coverage** | Uses **AWS Organizations** payer ID; CUR “LinkedAccountId” column supports single or multi-account views |
| **Cost-allocation models** | `Cost Categories` for Shared-Infra, Security, etc.; showback CSV exported monthly |
| **Business-relevant tagging schema** | Mandatory tags: `CostCenter`, `Project`, `Environment`; validated by Tag Policies + SCPs |
| **Tag purity & automation** | Tag-enforcement Lambda fixes missing tags at creation; nightly Config rule audits tag compliance |
| **Visualization** | QuickSight dashboards: Exec Summary, BU Chargeback, Unit Economics; sample sections below |

---

## 4 · Sample QuickSight Dashboard Sections

### 4.1 Executive Summary (Top-level spend)
| Metric | Value | MoM Δ |
|--------|-------|-------|
| Current Month-to-Date | **$ 41 250** | +3.2 % |
| Forecast Month-End    | **$ 43 800** | ±4 %  |
| 12-Month Trend        | Line chart (rolling spend) |

### 4.2 Chargeback by CostCenter
```sql
SELECT cost_center,
       SUM(line_item_net_cost) AS total_cost
FROM vw_daily_cost
WHERE usage_date BETWEEN date_trunc('month', current_date) AND current_date
GROUP BY cost_center
ORDER BY total_cost DESC;
```

### 4.3 Unit-Economics (Cost per 1 k API Calls)

| Month   | API Calls | Total Cost | \$ / 1 k Calls |
| ------- | --------- | ---------- | -------------- |
| 2025-04 | 2 100 000 | \$ 820     | **\$ 0.39**    |
| 2025-05 | 2 250 000 | \$ 860     | **\$ 0.38**    |

---

## 5 · Reporting & Accountability Process

| Step                               | Owner          | Tool                  | Output                         |
| ---------------------------------- | -------------- | --------------------- | ------------------------------ |
| Daily cost ingest & tag enrichment | FinOps Lambda  | S3, Lambda            | Updated CUR files              |
| Weekly variance review             | FinOps Analyst | Athena, Cost Explorer | Jira tickets for >5 % variance |
| Monthly chargeback export          | Finance        | Athena → CSV          | BU-level invoice               |
| Quarterly KPI review               | Exec Sponsor   | QuickSight            | Value-realization slide deck   |

---

## 6 · Sample SQL View (`vw_ttm_cost`)

```sql
CREATE OR REPLACE VIEW finops.vw_ttm_cost AS
SELECT
    date_trunc('month', usage_start_date) AS month,
    linked_account_id                AS account,
    COALESCE(resource_tags.project, 'UnTagged')      AS project,
    SUM(line_item_net_cost)          AS total_cost
FROM cur_database.cur_table
WHERE usage_start_date >= add_months(trunc(current_date, 'month'), -12)
GROUP BY 1,2,3;
```

---

## 7 · Deliverables

* **QuickSight dashboards** (URL + template import file)
* **Athena view DDLs** (`vw_daily_cost`, `vw_ttm_cost`)
* **Tag-enforcement Lambda code** (optional appendix)
* **Monthly chargeback runbook** (embedded in Planning & Forecasting section)

---

*Last updated: 30 Jun 2025*


