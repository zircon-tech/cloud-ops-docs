# QuickSight Dashboard Samples

## A. Executive Summary

* Current month run-rate: **$4 100**  
* Forecast month-end: **$4 250** (±5 %)

## B. Unit-Economics View (Cost / 1 k API Calls)

| Date       | API Calls | AWS Cost | $ / 1 k Calls |
|------------|-----------|----------|---------------|
| 2025-06-01 | 2 100 000 | $820     | **$0.39**     |
| 2025-07-01 | 2 250 000 | $860     | **$0.38**     |

## C. Rolling 12-Month Spend

Use QuickSight “Line + Area Chart” with:

* X-axis: `month`
* Y1: `total_cost`
* Y2 (YoY Δ): `percent_change`
