# AWS Account Ownership-Transfer Methodology

## 1 · Purpose
Provide a controlled, auditable process for transferring ownership of one or more AWS accounts and establishing new agreements when a customer undergoes a merger, acquisition, or divestiture.

---

## 2 · Triggering Events
* Corporate **merger** or **acquisition** (target accounts move under new parent)
* **Divestiture** or spin-off (subset of accounts leave the existing Organization)
* Changes to **contractual obligations** (e.g., Enterprise Discount Program, Marketplace agreements)

---

## 3 · Stakeholder Roles

| Role                       | Responsibility                                                            | Typical Person / Team            |
|----------------------------|----------------------------------------------------------------------------|----------------------------------|
| **Executive Sponsor**      | Final business approval; signs amended agreements                         | CIO / CFO                        |
| **Legal / Procurement**    | Review & execute Enterprise Agreement, EDP, Marketplace Private Offers    | Legal Counsel / Procurement Lead |
| **Finance / Billing Owner**| Update payer account, tax settings, invoices                              | Finance Manager                  |
| **Security Lead**          | Validate guardrails, SCPs, logging before & after transfer                | SecOps Engineer                  |
| **AWS Root Account Owner** | Change root email, MFA, and contact info                                  | IT Director or Delegate          |
| **AWS Partner (ZirconTech)**| Coordinate timeline, automate Control Tower/OUs, liaise with AWS Support | Engagement Manager               |
| **AWS Account Manager**    | Process new agreements, Enterprise Support mapping                        | AWS CAM                           |

---

## 4 · High-Level Workflow

1. **Discovery & Scoping**  
   * Inventory accounts, OUs, IAM Identity Center assignments, support plans, Marketplace subscriptions.  
   * Identify compliance dependencies (HIPAA, PCI) that may restrict region moves.

2. **Legal & Contract Updates**  
   * Draft amendment to Enterprise Agreement or create new EDP ID.  
   * Obtain legal signatures; AWS Account Manager attaches updated agreement to payer.

3. **Pre-Transfer Validation**  
   * Ensure CloudTrail → LogArchive, Config recorders running.  
   * Security Lead signs off that all mandatory guardrails are in *Enabled* state.

4. **Technical Transfer Steps**  
   1. **Detach** account(s) from current Organization (`RemoveAccountFromOrganization`).  
   2. **Update root** email, phone, MFA, and billing contacts via **AWS Organizations console** or CLI.  
   3. **Accept invitation** into new Organization / Control Tower landing zone (`InviteAccountToOrganization`).  
   4. Move account into target OU; mandatory SCPs and controls auto-attach.

5. **Billing Cut-Over**  
   * Finance Owner verifies account now linked to new payer ID and correct tax profile.  
   * Close out invoices on the old payer.

6. **Post-Transfer Audit**  
   * Run automation script `account-baseline-check` (Control Tower + AWS Config) to validate:  
     - Guardrails enabled  
     - S3 log buckets mapped  
     - Cost-allocation tags inherited  
   * Security & Finance produce sign-off report.

7. **Runbook Completion**  
   * ZirconTech archives ticket, attaches audit report, and schedules 30-day health check.

---

## 5 · Automation & Tooling

| Task                           | Tool / Script                                 |
|--------------------------------|-----------------------------------------------|
| Inventory export               | `aws organizations list-accounts` + CSV       |
| Root-credential rotation       | AWS CLI + IAM Identity Center                  |
| OU move & guardrail attach     | Control Tower **Lifecycle** API                |
| Baseline compliance check      | ZirconTech script `account-baseline-check`     |
| Billing re-mapping             | AWS Billing Console API / Cost Explorer        |

---

## 6 · Deliverables

1. **Ownership-Transfer Plan** (timeline, roles, communication channels)  
2. **Signed agreement** or amendment (EDP / EA)  
3. **Technical runbook** with CLI commands and rollback steps  
4. **Post-transfer audit report** (PDF)  
5. **30-day health-check notes**

---

_Last updated: 30 Jun 2025_
