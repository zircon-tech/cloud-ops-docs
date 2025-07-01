# Runbook — Updating Forecast Drivers


## 1 · Purpose
Explain how to refresh demand-driver data (e.g., MAU projections, storage growth) so that Athena views, Amazon Forecast models, and QuickSight dashboards stay accurate.

---

## 2 · Prerequisites

* Access to **AWS Glue/Athena** workspace `forecast`  
* IAM permissions: `athena:StartQueryExecution`, `s3:PutObject`, `stepfunctions:StartExecution`  
* Product-roadmap CSV or XLSX with updated demand drivers

---

## 3 · Procedure

### Step 1 – Collect Inputs
1. **Product roadmap** → export growth assumptions to `drivers_YYYY-MM-DD.csv`.  
2. **Capacity planning sheet** → confirm CPU-hour and storage forecasts.

### Step 2 – Upload to S3
```bash
aws s3 cp drivers_2025-07-01.csv s3://finops-data/forecast/drivers/
````

### Step 3 – Update Athena Driver Table

```sql
-- File: sql/insert_drivers.sql
INSERT INTO forecast.drivers
SELECT *
FROM EXTERNAL_TABLE('s3://finops-data/forecast/drivers/drivers_2025-07-01.csv');
```

Run in Athena (or via `aws athena start-query-execution`).

### Step 4 – Recalculate Forecast

Trigger the Step Functions state machine:

```bash
aws stepfunctions start-execution \
  --state-machine-arn arn:aws:states:us-east-1:123456789012:stateMachine:RecalcForecast \
  --name "Reforecast_$(date +%Y%m%d_%H%M%S)"
```

The workflow:

1. Queries Athena view `vw_unit_cost`
2. Calls Amazon Forecast to produce a 12-month projection
3. Publishes results to QuickSight dataset `unit_cost_forecast`

### Step 5 – Verify Dashboard

* Open QuickSight dashboard **“Unit-Economics”** → tab **“Forecast”**.
* Check that the “Last refresh” timestamp is current.
* Sanity-check key metrics (e.g., cost per 1 k API calls).

### Step 6 – Notify Stakeholders

Post update in **#finops** Slack channel:

```
Forecast drivers updated (2025-07-01). Dashboard refreshed. Variance within ±3 %.
```

---

## 4 · Rollback (if needed)

1. Revert `forecast.drivers` table to previous partition:

```sql
DELETE FROM forecast.drivers WHERE load_dt = '2025-07-01';
```

2. Re-run Step Functions `RecalcForecast` with prior date parameter.

---

## 5 · References

* **Athena database**: `forecast`
* **Step Functions**: `RecalcForecast` (version v1.3)
* **QuickSight dataset**: `unit_cost_forecast`
* **Tagging**: all driver records tagged `Environment=FinOps`


