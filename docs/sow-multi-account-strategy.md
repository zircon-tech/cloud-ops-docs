# Scope of Work  
**AWS Multi-Account Strategy Engagement**

*Prepared for: <Customer>*  
*Prepared by: ZirconTech*

## 1. Objectives

* Design and deploy an AWS multi-account landing zone that meets security, operations, and cost-management requirements.
* Enable automated and auditable account provisioning.

## 2. In-Scope Activities

| Phase | Activities | Outputs |
|-------|------------|---------|
| 1. Discovery | Stakeholder workshops, requirements matrix | Current-state report |
| 2. Design | OU map, guardrail set, account factory design | Architecture doc |
| 3. Build | Deploy Control Tower, SCPs, logging, AFT pipeline | Configured landing zone |
| 4. Validate | Functional and security testing, handover | Test report |
| 5. Knowledge Transfer | Runbooks, KT session | Final documentation pack |

## 3. Deliverables

* Multi-account architecture document  
* Configured AWS Control Tower landing zone  
* Terraform / AFT repository with account factory workflows  
* SCP and AWS Config rules library  
* Operational runbooks and knowledge-transfer recording

## 4. Timeline

| Milestone | Week |
|-----------|------|
| Kick-off   | 0 |
| Landing zone deployed | 2 |
| Account factory operational | 3 |
| Validation complete | 4 |

> Actual dates confirmed during project planning.

## 5. Roles and Responsibilities

| Role | ZirconTech | Customer |
|------|------------|----------|
| Project manager | ✓ |  |
| Landing-zone engineer | ✓ |  |
| Security lead | ✓ | Point of contact |
| Identity architect |  | ✓ (IdP integration) |
| Change approval board |  | ✓ |

## 6. Assumptions

* Customer grants required IAM permissions.  
* External IdP is available for IAM Identity Center integration.  
* All testing uses non-production data.

## 7. Out-of-Scope

* Application refactoring  
* On-premise network changes  
* Ongoing managed services beyond 30 days post-go-live

## 8. Acceptance Criteria

* All guardrails in “Mandatory” state in Control Tower  
* At least one workload account created via factory and passing security checks  
* Sign-off on final documentation pack

## 9. Pricing and Payment

Time-and-materials, invoiced bi-weekly. Detailed rate card provided separately.

## 10. Change Management

Material changes to scope, timeline, or cost will be handled through a mutually approved change order.
