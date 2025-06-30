# ZirconTech Methodology – AWS Account Lifecycle

## 1. Purpose  
Define a repeatable process to create, operate, suspend, and delete AWS accounts with automation, guardrails, and clear approval gates.

## 2. Lifecycle Stages

| Stage          | Goal                                         | Typical Status Flags | Owner                |
|----------------|----------------------------------------------|----------------------|----------------------|
| **Request**    | Business or project requests a new account   | Ticket opened        | Project sponsor      |
| **Create**     | Account provisioned with baselines           | CT `CreateManagedAccount` event | Landing-zone engineer |
| **Operate**    | Workloads run, controls enforced             | Normal ops           | Workload team        |
| **Suspend**    | Temporarily restrict activity                | CT `SuspendManagedAccount` | Security / FinOps    |
| **Delete**     | Close account, retain evidence               | CT `CloseAccount`    | Governance board     |

## 3. Account Creation Workflow

1. **Inputs**  
   * Business justification  
   * Desired OU (e.g., `Workloads/Prod`)  
   * Cost-allocation tag key-value  
2. **Automation**  
   * Service: **Account Factory** (native) or **Account Factory for Terraform (AFT)**  
   * Optional extra steps (Lambda or AFT hooks):  
     ```bash
     # Pseudocode – runs right after account is provisioned
     create-iam-role --role-name "ZirconTechOps" \
                     --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
     enable-security-hub --standards "CIS-1.5"
     tag-resource --resource-id <accountId> --tags "CostCenter=1234"
     ```
3. **Outputs**  
   * Account ID, email, OU path  
   * IAM role `ZirconTechOps` with least privilege for support  
   * Entry in Cost Explorer with tags active

## 4. Account Suspension Workflow

| Step | Action | Tool |
|------|--------|------|
| 1    | Trigger suspension ticket | Jira / ServiceNow |
| 2    | **Move account** to `Suspended` OU (SCP denies writes) | Organizations API |
| 3    | Disable IAM Identity Center assignments | IAM Identity Center |
| 4    | Freeze budgets and alert bill-ops | AWS Budgets |
| 5    | Log suspension event | CloudWatch + centralized audit account |

## 5. Account Deletion / Closure Workflow

1. Verify account is **Suspended** for at least 60 days.  
2. Export CloudTrail, Config, and S3 objects to the LogArchive account.  
3. Close account via `CloseAccount` (Organizations).  
4. Retain logs for 7 years in Glacier Deep Archive (compliance default).  
5. Update CMDB and tag account as `Status=Closed`.

## 6. Additional IAM Roles

| Role name          | Purpose                     | Created at |
|--------------------|-----------------------------|------------|
| `ZirconTechOps`    | Break-glass admin support   | Account create |
| `SecurityAudit`    | Read-only security reviews  | Account create |
| `FinOpsReadOnly`   | Cost Explorer & Budgets API | Account create |

Roles are deployed by AFT hook or Control Tower lifecycle Lambda.

## 7. Drift Detection & Review Cadence

* **Monthly** – Control Tower detects guardrail drift; report sent to project owner.  
* **Quarterly** – Review suspended accounts for deletion approval.  
* **Annually** – Confirm IAM roles, tags, and OU placement still correct.

## 8. Deliverables to Customer

* Account lifecycle runbooks (PDF + Markdown)  
* AFT or pipeline source code  
* IAM role JSON templates  
* Compliance evidence: drift reports, closure proof

## 9. Change Management

Any update to lifecycle steps, retention periods, or automation hooks is version-controlled in Git and requires a pull-request review by the Governance Board.
