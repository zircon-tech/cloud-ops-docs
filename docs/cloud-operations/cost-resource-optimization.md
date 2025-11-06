# Resource-Level Cloud-Cost Optimization

## 1 · Purpose  
Provide customers an automated framework to right-size compute & database
instances, modernize storage, eliminate idle/orphaned assets, and tame
network/data-transfer spend—continuously and at scale.

---

## 2 · Framework Overview

```

CloudWatch Metrics   Trusted Advisor   Cost Explorer
│                    │                │
├────────► AWS Compute Optimizer ◄────┤
│                                      │
▼                                      ▼
Optimization Hub (Athena views + QuickSight)  ◄─‒‒ Tag-driven schedules (Lambda)
│
▼
Automation Engine (SSM Automation, EventBridge, Lambda)

```

---

## 3 · Compute & Database Right-Sizing

| Feature | Implementation |
|---------|----------------|
| **Utilization metrics** | Import CPU, network, disk I/O, memory (CloudWatch agent) into Compute Optimizer |
| **Cross-family recommendations** | Enable *EC2 enhanced metrics* to get suggestions for Graviton/M7i/T4g families |
| **RDS right-sizing** | RDS Performance Insights + Trusted Advisor => “modify-db-instance” recommendations |
| **Newer generations** | Daily script filters Optimizer output for “can be modernized” flag |
| **Approval workflow** | Jira ticket auto-created; change applies via SSM Automation after CAB sign-off |

**Sample CLI extract**

```bash
aws compute-optimizer get-ec2-instance-recommendations \
  --filter name=Finding,values=Underprovisioned \
  --query "instanceRecommendations[].[instanceArn,recommendedInstanceType]"
```

---

## 4 · Elasticity & Scheduling

| Use Case                    | Tactic                                                                                        |
| --------------------------- | --------------------------------------------------------------------------------------------- |
| Office-hour dev/test stacks | **Instance Scheduler** solution – tag `Schedule=weekday9to6`                                  |
| Predictable nightly batch   | **Auto Scaling scheduled actions** – scale to 0 at 02:00 UTC                                  |
| Ad-hoc workloads            | Lambda checks past seven-day CloudWatch metrics; stops idle instances after 2 h below 5 % CPU |

---

## 5 · Storage Optimization

### 5.1 Metrics & Modernization

* CloudWatch `VolumeIdleTime`, `VolumeThroughputPercentage` drive GP2 → GP3 list.
* SSM Automation document **`AWSSupport-ModifyEBStoGP3`** executes change.

### 5.2 Snapshot Governance

| Policy                               | Tool                                            |
| ------------------------------------ | ----------------------------------------------- |
| Tag-based retention (`Retention=30`) | Lambda + EventBridge (“EBS-Snapshot-Cleaner”)   |
| Un-tagged snapshots older > 90 d     | Step Functions workflow requests tag or deletes |

### 5.3 Lifecycle & Tiering

| Storage                          | Automation                                                               |
| -------------------------------- | ------------------------------------------------------------------------ |
| **S3**                           | Intelligent-Tiering, Lifecycle rules to Glacier Deep Archive after 365 d |
| **EFS**                          | Infrequent-Access on after 30 d idle                                     |
| **Incomplete multipart uploads** | Lifecycle rule: abort after 7 d                                          |

---

## 6 · Idle / Orphaned Resource Cleanup

| Resource            | Detection Query                                                                        | Remediation                      |
| ------------------- | -------------------------------------------------------------------------------------- | -------------------------------- |
| Elastic IPs         | `DescribeAddresses` where `AssociationId` NULL                                         | Lambda releases after 3 d        |
| Unattached EBS vols | Athena CUR query `UsageType='EBS:VolumeUsage' AND ResourceId NOT IN (ec2 vols in use)` | SSM runbook snapshots & deletes  |
| Idle RDS            | CloudWatch `CPUUtilization < 3% AND ConnCount < 1` for 7 d                             | Stop DB; notify owner tag        |
| Idle Redshift       | Trusted Advisor + cluster CPU metrics                                                  | Pause cluster; review after 14 d |
| Unused VPCs         | No ENIs + no routes                                                                    | Terraform PR deletes             |
| Idle ALBs           | `RequestCount < 1` 7 d                                                                 | Lambda deletes                   |

---

## 7 · Networking & Data-Transfer Cost Controls

* **Athena‡CUR view** highlights `DataTransfer-Out-Bytes` spikes.
* Enable **Amazon CloudFront** + Regional Edge Cache for egress heavy apps.
* **S3 → EC2 same-AZ** pattern enforced by Config rule; cross-AZ flag triggers Jira.
* **VPC interface endpoints** replace NAT GW where feasible; cost impact tracked monthly.

---

## 8 · Continuous Optimization Process

| Frequency | Task                                                 | Owner          | Tool |
| --------- | ---------------------------------------------------- | -------------- | ---- |
| Daily     | Pull Optimizer + Trusted Advisor findings → DynamoDB | Lambda         |      |
| Weekly    | Generate QuickSight “Top 20 Savings Ops” report      | FinOps Analyst |      |
| Monthly   | Execute approved Automation runbooks                 | DevOps         |      |
| Quarterly | Re-evaluate RI/SP mix based on right-sizing results  | FinOps + SA    |      |

---

## 9 · Sample QuickSight “Top Savings Opportunities”

| Resource        | Rec Type                  | Old → New  | Annual Savings |
| --------------- | ------------------------- | ---------- | -------------- |
| `i-0ab1c`       | EC2 t3.large → t4g.medium | **\$ 410** |                |
| `mysql-prod-db` | RDS r5.large → r7g.large  | **\$ 720** |                |
| 15 GP2 vols     | Convert to GP3            | **\$ 530** |                |

---

## 10 · Deliverables

* **Right-Sizing Report** – CSV + dashboard link
* **GP2→GP3 Migration Plan** – SSM Automation doc + schedule
* **Snapshot & Orphan Cleaner** – Lambda code (embedded appendix)
* **Monthly Optimization Summary** – PDF auto-emailed to stakeholders

---

## Appendix – Lambda Skeleton: EBS Snapshot Cleaner

```python
import boto3, os, datetime
ec2 = boto3.client('ec2')
retention = int(os.environ['RETENTION_DAYS'])

def lambda_handler(event, context):
    today = datetime.datetime.utcnow()
    snaps = ec2.describe_snapshots(OwnerIds=['self'])['Snapshots']
    for s in snaps:
        if 'Tags' not in s:
            age = (today - s['StartTime'].replace(tzinfo=None)).days
            if age > retention:
                ec2.delete_snapshot(SnapshotId=s['SnapshotId'])
```

---

*Last updated: 30 Jun 2025*
