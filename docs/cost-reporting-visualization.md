# Cost Reporting & Visualization

## Purpose  
ZirconTech delivers a self-service reporting stack that measures, monitors, and drives accountability for cloud spend across single AWS accounts up to consolidated, multi-account organizations. Our methodology transforms cost data into actionable business intelligence through automated tagging, comprehensive allocation models, and real-time visualization.

## Architecture Overview

Our cost reporting architecture centers on AWS Cost and Usage Reports (CUR) delivered hourly to S3 with complete resource-level detail. Glue crawlers automatically discover and catalog this data, creating Athena external tables that provide SQL access across all linked accounts in an organization. 

A tag enrichment Lambda function ensures tag purity by automatically adding missing CostCenter and Project tags, enabling accurate showback reporting. QuickSight dashboards provide executive visibility into spend patterns, forecasts, and anomalies while supporting drill-down analysis to individual resources.

```
CUR (S3, hourly, resource-level)
│
▼
Glue Crawler ─► Athena External Table ─►  Views (vw_daily, vw_ttm)
│                                      │
▼                                      ▼
Tag Enrichment Lambda (ensures tag purity)   QuickSight Dashboards
```

## Methodology and Capabilities

**Comprehensive Spend Measurement and Monitoring**: Athena views including `vw_daily_cost` and `vw_ttm_cost` provide granular cost analysis, while QuickSight dashboards offer organization-wide spend visibility with filtering by Account, Organizational Unit, and Region. This approach scales seamlessly from single account deployments to complex multi-account organizations using AWS Organizations payer account consolidation.

**Cost Allocation Models Aligned with Account Structures**: Cost Categories define allocation rules for shared infrastructure, security services, and cross-account resources. Monthly showback CSV exports provide business units with detailed cost attribution. The `LinkedAccountId` column in CUR data enables both single and multi-account cost views within the same reporting framework.

**Business-Relevant Tagging Schema with Automation**: Mandatory tags including `CostCenter`, `Project`, and `Environment` are enforced through Tag Policies and Service Control Policies. A tag-enforcement Lambda function automatically corrects missing tags at resource creation, while nightly Config rules audit tag compliance across the organization.

**Unit Economics and Value Attribution**: CUR data joins with business KPIs to create meaningful cost-per-unit metrics such as cost per 1,000 API calls, cost per customer, or cost per transaction. This enables product teams to understand the economic impact of their architectural decisions and optimize accordingly.

## Sample Dashboard Implementation

### Executive Summary Dashboard
Current month-to-date spend of $41,250 represents a 3.2% increase month-over-month, with month-end forecast of $43,800 within a 4% confidence interval. Rolling 12-month trend analysis identifies seasonal patterns and growth trajectories for strategic planning.

### Cost Center Chargeback Analysis
```sql
SELECT cost_center,
       SUM(line_item_net_cost) AS total_cost
FROM vw_daily_cost
WHERE usage_date BETWEEN date_trunc('month', current_date) AND current_date
GROUP BY cost_center
ORDER BY total_cost DESC;
```

This query generates monthly chargeback reports by extracting cost center attribution from enforced resource tags, enabling accurate business unit cost allocation.

### Unit Economics Tracking
April 2025 data shows 2.1 million API calls costing $820 total, resulting in $0.39 per 1,000 calls. May optimization efforts reduced unit cost to $0.38 per 1,000 calls despite increased volume to 2.25 million calls, demonstrating effective cost optimization.

## Reporting Process and Accountability

Daily cost ingestion occurs automatically through the FinOps Lambda function processing S3 CUR files and applying tag enrichment. Weekly variance reviews by FinOps analysts using Athena and Cost Explorer identify spending anomalies exceeding 5% variance, triggering Jira tickets for investigation.

Monthly chargeback exports generated through Athena CSV output provide Finance teams with business unit-level invoicing data. Quarterly KPI reviews conducted by executive sponsors using QuickSight dashboards produce value-realization presentations demonstrating cloud optimization ROI.

## Technical Implementation

### Trailing Twelve Months Cost View
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

This view enables trend analysis and year-over-year comparisons while highlighting untagged resources requiring remediation.

## Deliverables

Complete implementation includes QuickSight dashboard templates with import files, Athena view DDL statements for `vw_daily_cost` and `vw_ttm_cost`, tag-enforcement Lambda function code, and monthly chargeback runbook integration with planning and forecasting processes.

For detailed cost allocation methodology including shared service distribution rules and tag enforcement policies, see [Cost Allocation, Measurement & Accountability](cost-allocation.md).

---

*Last updated: 30 Jun 2025*


