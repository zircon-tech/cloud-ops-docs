# Purchase-Option Cloud-Cost Optimization

## 1 · Purpose  
Maximize long-term AWS savings by recommending the optimal mix of **Savings Plans (SPs), Reserved Instances (RIs), and Spot**—across all linked accounts and services—while making trade-offs transparent to finance and engineering.

---

## 2 · Capabilities

| Checklist Item | ZirconTech Implementation |
|----------------|---------------------------|
| **1- & 3-year SP/RI recommendations** | Athena CUR analysis + AWS Compute Optimizer SP/RDS RI CSV exports; forecasts normalized usage to recommend commitments for both 1- and 3-year terms |
| **Cross-account coverage** | Pull CUR “LinkedAccountId” + Organizations tag to aggregate usage; recommendations split by payer or BU |
| **All SP/RI types** | Compute SP, EC2 Instance SP, Standard/Convertible, Zonal/Regional RIs; coverage plan JSON lists each recommendation |
| **Multi-service RIs** | Same pipeline evaluates RDS, Redshift, ElastiCache, DynamoDB standby; uses respective service recommendation APIs |
| **Spot instance guidance** | Spot Fleet Advisor + AWS Instance Selector CLI to build diversified launch templates matched to workload tolerance |

---

## 3 · Methodology

### 3.1 Data Collection  
1. Enable **Cost & Usage Report** with hourly granularity.  
2. Run **`aws compute-optimizer export-recommendation`** for EC2/RDS.  
3. Ingest to Athena table `finops.rec_input`.

### 3.2 Commitment Modeling

| Step | Tool | Output |
|------|------|--------|
| Calculate normalized hours | Athena SQL | `vw_norm_usage` |
| Run RI/SP optimizer | Python notebook w/ `boto3` + OR-Tools | JSON plan (`ri_sp_plan.json`) |
| Spot workload fit | Instance Selector + historical price API | “Spot-eligible” launch template list |

### 3.3 Decision Workshop  
* Compare options: **Compute SP vs. EC2 Instance SP**, **SP vs. RI**, visualized in QuickSight “Commitment Trade-off” dashboard.  
* Finance chooses risk appetite (commit %).  
* CAB approves purchase order or Private Offer.

### 3.4 Execution & Tracking  
* Commit via AWS Console or **`purchase-savings-plan`** API.  
* Tag commitments `CostCenter`, `Project`.  
* Monthly variance check: actual vs. covered hours.

---

## 4 · Clarify / Compare / Contrast

| Feature | **Compute SP** | **EC2 Instance SP** | **Standard RI** | **Convertible RI** |
|---------|---------------|---------------------|-----------------|--------------------|
| Flexibility | Any region, OS, size | Specific family & region | Exact family/size/AZ | Family swap allowed |
| Discount | 66 % max | 72 % max | Up to 75 % | Up to 54 % |
| Applies to | EC2, Fargate, Lambda | EC2 only | EC2 only | EC2 only |
| Convertible? | N/A | N/A | No | Yes |
| Best for | Diverse workloads | Homogeneous fleets | Steady state in one AZ | Long-lived but changing |

---

## 5 · Spot Instance Recommendations

| Workload | Recommendation | Rationale |
|----------|----------------|-----------|
| Batch / CI jobs | Spot Fleet with 6 instance pools spread across 3 AZs | 90 % savings, interruption-tolerant |
| Stateless web | EC2 Auto Scaling group: 20 % Spot, 80 % On-Demand | Balances savings and availability |
| Stateful DB | **Do not use Spot** | Data loss risk |

**Instance-type optimization**  
```bash
aws ec2-instance-selector --vcpus 4 --memory 16 --usage-class spot \
  --price-per-hour 0.05 --region us-east-1
````

Outputs cheapest compatible types considering real-time Spot price.

---

## 6 · Sample Coverage Plan (inline)

```json
{
  "generated": "2025-06-30T12:00:00Z",
  "computeSavingsPlan": {
    "termYears": 3,
    "hourlyCommitUSD": 5.2,
    "expectedAnnualSavingsUSD": 21_300
  },
  "ec2InstanceSavingsPlan": {
    "m6i.large": { "commitHours": 8, "savings": 2_800 }
  },
  "rdsStandardRI": {
    "db.r7g.large": { "term": "1yr", "paymentOption": "NoUpfront", "count": 2 }
  },
  "spotStrategy": { "fleetDiversification": 6, "targetPct": 25 }
}
```

---

## 7 · Continuous Optimization Loop

| Frequency | Task                                              | Owner               |
| --------- | ------------------------------------------------- | ------------------- |
| Weekly    | Import latest usage, refresh commitment model     | Lambda              |
| Monthly   | Governance call to review coverage & Spot savings | FinOps Analyst      |
| Qtrly     | Rebalance Convertible RI exchanges                | Solutions Architect |

---

*Last updated: 30 Jun 2025*
