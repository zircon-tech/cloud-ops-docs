# Third-Party Tooling & Procurement Process

## 1 · Purpose  
Define how ZirconTech helps customers (a) identify the right third-party tools, (b) procure and deploy them through approved channels such as AWS Marketplace, and (c) manage licenses throughout their lifecycle.

---

## 2 · Methodology for Identifying Third-Party Tools

| Step | Activity | Output |
|------|----------|--------|
| 1. Requirement Capture | Workshops with stakeholders (security, finance, DevOps, data) | Tool capability requirements |
| 2. Shortlisting | Score vendors on security posture, AWS native integration, pricing model, support quality | Ranked vendor list |
| 3. Proof of Value | Sandbox testing in non-production account using available trials | Evaluation report |
| 4. Risk & Compliance Review | Vendor security assessment, compliance certification review, data residency evaluation | Approved vendor list |
| 5. Procurement Path Decision | Determine optimal procurement channel based on cost and terms | Procurement strategy |
| 6. Final Selection | Steering committee approval process | Purchase authorization |

*A vendor evaluation matrix should be maintained and updated after every tool evaluation.*

---

## 3 · Procurement & Deployment Approaches

| Approach | Details |
|----------|---------|
| **AWS Marketplace Private Offers** | Negotiate custom terms (price, payment schedule, licensing) through AWS Marketplace with consolidated billing integration |
| **Consulting Partner Private Offers** | Partner-facilitated procurement bundling software with implementation services |
| **AWS Service Catalog** | Publish approved tools as Service Catalog products with IAM-controlled access permissions |
| **Infrastructure as Code** | Deploy tools using version-controlled templates with automated testing |
| **Container Management** | Use private container registries for supply-chain security and availability |
| **License Management** | Store license credentials in AWS Secrets Manager with automated rotation |

---

## 4 · Integration with Customer Procurement Systems

| Integration Type | Implementation Pattern | Workflow Integration |
|------------------|------------------------|---------------------|
| **API Integration** | Connect existing procurement systems through RESTful APIs | Automated purchase order creation and tracking |
| **Event-Driven Workflows** | Use enterprise service buses to trigger procurement processes | Real-time approval and deployment workflows |
| **Approval Systems** | Link AWS Marketplace purchases to existing approval workflows | Consistent governance and compliance |

---

## 5 · License Management & Compliance

* **AWS License Manager** tracks bring-your-own-license (BYOL) entitlements for Windows, SQL Server, and commercial AMIs
* **Resource Tagging** ensures resources inherit license tracking tags; AWS Config rules flag untagged instances
* **Cost Monitoring** uses vendor-based tags to generate alerts for unusual spending patterns
* **Renewal Management** provides automated renewal reminders with approval workflow integration
* **End-of-Life Processing** includes structured decommissioning with license tracking updates

---

## 6 · Example Procurement Workflow

1. **Requirements Analysis**: Document specific tool capabilities needed for security monitoring
2. **Vendor Evaluation**: Score multiple vendors using evaluation matrix criteria
3. **Proof of Concept**: Deploy selected tool in sandbox environment for testing
4. **Business Case**: Document expected benefits and cost justification
5. **Procurement**: Execute purchase through AWS Marketplace Private Offer
6. **Deployment**: Use infrastructure as code for automated, governed deployment
7. **License Management**: Configure automated license tracking and renewal reminders

---

## 7 · Deliverables

* Vendor evaluation framework and scoring templates
* Procurement workflow documentation
* AWS Service Catalog portfolios for approved tools
* License management procedures and dashboards
* Renewal tracking and approval workflows

_This methodology provides a structured approach to third-party tool procurement while leveraging AWS-native services for governance and management._
