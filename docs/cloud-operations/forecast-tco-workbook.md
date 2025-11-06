# Forecast & TCO Workbook (Sample)

## 1 Inventory Snapshot

| ServerID   | Env  | vCPU | Memory (GB) | OS    | On-Prem Cost / Yr |
|------------|------|------|-------------|-------|-------------------|
| srv-app-01 | Prod | 4    | 16          | Linux | $1 200            |
| srv-db-01  | Prod | 8    | 32          | Linux | $2 400            |

## 2 Model Assumptions

| Parameter                | Value |
|--------------------------|-------|
| SavingsPlan discount     | 30 %  |
| Reserved Instance discount| 40 % |
| Spot Instance discount   | 70 %  |
| Annual growth            | 20 %  |

## 3 12-Month Forecast (Lift & Shift)

| Month    | Total AWS Cost |
|----------|----------------|
| 2025-01  | $3 500 |
| 2025-02  | $3 600 |
| …        | … |
| 2025-12  | $4 600 |

## 4 TCO Summary (3-Year)

| Scenario     | 3-Yr Total | NPV |
|--------------|------------|-----|
| On-Prem      | $300 000   | $270 000 |
| Lift & Shift | $250 000   | $225 000 |
| Optimized    | $180 000   | $162 000 |
