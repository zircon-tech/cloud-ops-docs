# Third-Party Tooling & Procurement Process

## 1 · Purpose  
Define how ZirconTech helps customers (a) identify the right third-party tools, (b) procure and deploy them through approved channels such as AWS Marketplace Private Offers, and (c) manage licenses throughout their lifecycle.

---

## 2 · Methodology for Identifying Third-Party Tools

| Step | Activity | Output |
|------|----------|--------|
| 1. Requirement Capture | Workshops with stakeholders (security, finance, DevOps, data) | Tool capability matrix |
| 2. Shortlisting | Score vendors on security posture, AWS native integration, pricing model, support SLAs | Ranked shortlist |
| 3. Proof of Value | 14-day sandbox in non-prod account using AWS Marketplace free trials or SaaS test tenant | POV report |
| 4. Risk & Compliance Review | Vendor security questionnaire, SOC 2 / ISO 27001 review, data residency check | Approved / rejected vendor list |
| 5. Procurement Path Decision | Determine: Private Offer, CPPO, EDP credits, direct PO, or reseller | Procurement plan |
| 6. Final Selection | Steering committee signs off | Purchase request in customer ERP |

*A living **Vendor Capability Matrix** is maintained in [vendor-matrix.md](vendor-matrix.md) and updated after every tool evaluation.*


---

## 3 · Procurement & Deployment Tactics

| Tactic | Details |
|--------|---------|
| **AWS Marketplace Private Offer** | Negotiate custom terms (price, payment schedule, EULA) and buy via the customer’s AWS ID; integrates with consolidated billing and applies to Enterprise Discount Program spend tiers. |
| **Marketplace CPPO** | When vendor is a Consulting Partner Private Offer participant, ZirconTech can resell and bundle implementation services on the same bill. |
| **Service Catalog Portfolios** | Once purchased, AMIs or CloudFormation templates are published as Service Catalog products; IAM permissions limit who can launch. |
| **Terraform Registry / Helm Repos** | For open-source or BYOL software, modules are vendored into `infra/modules/`; version pinned via `~>` constraints. |
| **Container Images** | Pulled from AWS ECR Public or Docker Hub, then mirrored to private ECR for supply-chain security. |
| **License Files / BYOL Keys** | Stored in AWS Secrets Manager with rotation policies; accessed by automation at deploy time. |

---

## 4 · Integration with Customer Procurement Systems

| System | Integration Pattern | Artifact Flow |
|--------|--------------------|---------------|
| **SAP Ariba** | Purchase Requisition triggered by Slack `/procure` slash-command → Lambda posts to Ariba API | PO number returned, tagged on AWS Marketplace order |
| **Coupa** | Private Offer SKU mapped to Coupa catalog item; once approved, order auto-syncs to AWS Marketplace via SNS + Lambda | Invoice attached to Coupa record |
| **ServiceNow** | Catalog item “Request Third-Party Tool”; Terraform Cloud API called after approval to deploy | CI/CD comments PO & CMDB number on PR |

---

## 5 · License Management & Compliance

* **AWS License Manager** tracks BYOL entitlements (Windows, SQL Server, commercial AMIs).  
* **Tagging** – resources inherit `LicenseId`, `Vendor`, `RenewalDate`; Config rule flags untagged instances.  
* **Budget Alerts** – cost anomalies against `Vendor` tag generate FinOps alerts.  
* **Renewal Workflow** – 60-day SNS reminder → Jira ticket → steering committee approval → Private Offer renewal or termination.  
* **End-of-Life Cleanup** – Service Catalog de-provision + License Manager decrement; CMDB status set to “Retired”.

---

## 6 · Example Control Flow: Procuring a Security SaaS via Private Offer

1. **Shortlist**: Lacework vs. Wiz scored in vendor matrix → Wiz selected.  
2. **Private Offer** drafted by vendor; terms: 12-mo SaaS, prepaid, 18 % discount.  
3. **Slack slash-command** posts PR-2025-047 to SAP Ariba; finance approves.  
4. **Offer accepted** in AWS Console; cost center tag auto-applied by Control Tower lifecycle function.  
5. **Terraform module** `modules/wiz-agent` deployed via CodePipeline to non-prod → prod after 1-week test.  
6. **License keys** retrieved via License Manager; renewal reminder set 11 months ahead.

---

## 7 · Deliverables

* Vendor capability matrix (`vendor-matrix.csv`)  
* Procurement workflow diagrams (draw.io)  
* Service Catalog portfolios & Terraform modules  
* License Manager dashboard link + monthly license-usage PDF  
* Renewal runbook and Jira template

_Last updated: 30 Jun 2025_
