# RI / Savings Plan Coverage Plan (Sample)

```json
{
  "generated": "2025-06-30T12:00:00Z",
  "coverage": {
    "ComputeSavingsPlan": {
      "recommendedCommitUSDPerHr": 4.5,
      "expectedAnnualSavingsUSD": 18000
    },
    "EC2_StandardRI": {
      "recommendedInstances": [
        { "instanceType": "m6i.large", "count": 4, "term": "1yr", "paymentOption": "NoUpfront" }
      ],
      "expectedAnnualSavingsUSD": 6000
    }
  },
  "spotStrategy": {
    "percentageOfFleet": 20,
    "supportedWorkloads": ["batch-processing", "stateless-web"]
  }
}

```



# Value Realization Report – Q3 2025

| KPI                 | Baseline (Q3 2024) | Current | Δ        | Notes                           |
|---------------------|--------------------|---------|----------|---------------------------------|
| AWS Cost / MAU      | $0.54              | $0.39   | ↓ 28 %   | Rightsizing + Savings Plan      |
| Release Frequency   | 2 / month          | 5 / month| ↑ 150 % | CI/CD pipeline                  |
| MTTR                | 4 h                | 1.2 h   | ↓ 70 %   | Auto-rollback in CodeDeploy     |
| Uptime              | 99.2 %             | 99.9 %  | +0.7 pp | Multi-AZ refactor               |

**Conclusion:** migration delivered cost efficiency **and** agility gains that exceeded the original business case.
