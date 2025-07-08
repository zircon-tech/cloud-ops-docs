---
id: COCG-002
title: "Third party products and procurement process"
---

# Third Party Products and Procurement Process

## Overview

Identifying, procuring, and managing third-party tools requires systematic processes that balance business needs with compliance, security, and cost optimization. ZirconTech provides comprehensive methodologies that streamline vendor evaluation, automate procurement through AWS Marketplace and customer procurement systems, and establish ongoing license management.

Our approach transforms ad-hoc tool acquisition into a governed process that leverages AWS-native procurement channels while integrating seamlessly with existing customer procurement workflows and financial systems.

## Comprehensive Third-Party Tool Management Framework

**For detailed methodology and complete procurement workflows**: See [Third-Party Tooling & Procurement Process](third-party-procurement.md)

**For vendor evaluation and comparison matrix**: See [Vendor Capability Matrix](vendor-matrix.md)

### Methodology for Identifying Third-Party Tools

Our systematic 6-step approach ensures optimal tool selection aligned with business requirements and compliance standards:

1. **Requirement Capture**: Stakeholder workshops across security, finance, DevOps, and data teams to define capability needs
2. **Vendor Shortlisting**: Score vendors on security posture, AWS native integration, pricing model, and support SLAs
3. **Proof of Value**: 14-day sandbox testing using AWS Marketplace free trials or SaaS test tenants
4. **Risk & Compliance Review**: Vendor security questionnaires, SOC 2/ISO 27001 validation, data residency verification
5. **Procurement Path Decision**: Determine optimal channel (Private Offer, CPPO, EDP credits, direct PO, reseller)
6. **Final Selection**: Steering committee approval with documented business justification

### Procurement and Deployment Tactics

#### AWS Marketplace Integration

| Procurement Method | Description | Benefits | Integration |
|-------------------|-------------|----------|-------------|
| **Private Offers** | Custom terms negotiated directly with vendors | Discounted pricing, flexible payment schedules | Consolidated billing, EDP credit application |
| **CPPO (Consulting Partner Private Offer)** | ZirconTech resells with bundled services | Single invoice for software + implementation | Service delivery tracked through AWS billing |
| **Standard Marketplace** | Direct SaaS subscriptions and AMIs | Quick deployment, standard terms | Automatic billing integration |

#### Deployment Automation

| Technology | Implementation Method | Purpose |
|------------|----------------------|---------|
| **Service Catalog** | Published portfolios with IAM permissions | Governed self-service tool deployment |
| **Terraform Registry** | Version-pinned modules in private registry | Infrastructure-as-code repeatability |
| **Container Images** | Private ECR mirroring from Docker Hub | Supply chain security and availability |
| **License Management** | AWS Secrets Manager with rotation | Secure license key distribution |

### Customer Procurement System Integration

#### Enterprise System Connectors

| ERP System | Integration Pattern | Workflow Automation |
|------------|-------------------|-------------------|
| **SAP Ariba** | Slack `/procure` command → Lambda → Ariba API | PO number auto-tagged on AWS orders |
| **Coupa** | Private Offer SKU mapped to catalog items | Auto-sync orders via SNS + Lambda |
| **ServiceNow** | Catalog item "Request Third-Party Tool" | Terraform Cloud API triggered post-approval |

#### Financial Integration

- **Budget Mapping**: Cost center tags applied automatically via Control Tower lifecycle hooks
- **Invoice Reconciliation**: AWS Marketplace invoices attached to customer ERP records
- **Anomaly Detection**: Budget alerts based on vendor tags trigger FinOps review
- **Renewal Management**: 60-day SNS reminder → Jira ticket → approval workflow

### License Management and Compliance

#### Automated License Tracking

| Component | Tool | Purpose |
|-----------|------|---------|
| **BYOL Entitlements** | AWS License Manager | Track Windows, SQL Server, commercial AMI licenses |
| **Resource Tagging** | Control Tower + Config Rules | Inherit `LicenseId`, `Vendor`, `RenewalDate` tags |
| **Usage Monitoring** | Cost Explorer + Budget Alerts | Detect cost anomalies by vendor tag |
| **Compliance Scanning** | AWS Config Rules | Flag untagged instances requiring licenses |

#### Lifecycle Management Process

1. **License Deployment**: Secrets Manager stores license keys with IAM-based access control
2. **Usage Tracking**: Config rules ensure proper tagging for license attribution
3. **Renewal Workflow**: Automated 60-day reminder → steering committee approval → renewal or termination
4. **End-of-Life Cleanup**: Service Catalog de-provision + License Manager decrement + CMDB retirement

## Technology Foundation

| Component | Primary Services | Purpose |
|-----------|-----------------|---------|
| **Marketplace Integration** | AWS Marketplace, Private Offers, CPPO | Streamlined procurement with consolidated billing |
| **Deployment Automation** | AWS Service Catalog, Terraform Cloud | Governed self-service deployment |
| **License Management** | AWS License Manager, AWS Secrets Manager | Secure license storage and compliance tracking |
| **Financial Integration** | AWS Cost Explorer, Amazon EventBridge | ERP integration and cost management |
| **Governance** | AWS Control Tower, Service Control Policies | Automated compliance and tag enforcement |

## Example Procurement Workflow

### Scenario: Security CSPM Tool Procurement

1. **Vendor Evaluation**: Lacework vs. Wiz scored in vendor matrix → Wiz selected based on AWS native integration
2. **Private Offer**: Vendor drafts custom terms (12-month SaaS, prepaid, 18% discount)
3. **ERP Integration**: Slack `/procure` command creates SAP Ariba requisition PR-2025-047
4. **Approval & Purchase**: Finance approves → AWS Marketplace offer accepted with auto-applied cost center tags
5. **Deployment**: Terraform module `modules/wiz-agent` deployed via CodePipeline (non-prod → prod)
6. **License Management**: License keys retrieved via License Manager, 11-month renewal reminder scheduled

## Deliverables and Evidence Artifacts

### Vendor Evaluation Artifacts
- **Vendor Capability Matrix**: Comprehensive scoring across security, AWS integration, pricing, support
- **Proof of Value Reports**: 14-day sandbox testing results with performance metrics
- **Risk Assessment Documentation**: Security questionnaires, compliance certification verification
- **Business Case Templates**: ROI analysis and justification documentation

### Procurement Integration
- **ERP Connector Code**: Lambda functions for SAP Ariba, Coupa, ServiceNow integration
- **Service Catalog Portfolios**: Pre-configured deployment templates with IAM permissions
- **Terraform Modules**: Version-controlled infrastructure-as-code for tool deployment
- **Financial Dashboard**: QuickSight cost tracking by vendor with budget alerts

### License Management
- **License Tracking Dashboard**: AWS License Manager integration with renewal calendars
- **Compliance Reports**: Monthly license usage and tagging compliance verification
- **Renewal Runbooks**: Automated workflow templates for license renewals
- **Contract Management**: Centralized repository of vendor agreements and terms

## Success Criteria

- **90%+ Marketplace Adoption**: Leverage AWS Marketplace for eligible tool purchases
- **Automated Compliance**: Config rule compliance >95% for license tagging requirements
- **ERP Integration**: Seamless purchase requisition flow with customer procurement systems
- **Cost Optimization**: Negotiated Private Offer discounts averaging 15-25% off list price

## Getting Started

Contact ZirconTech to implement comprehensive third-party tool management. Our proven methodologies and automation frameworks ensure optimal vendor selection, streamlined procurement, and ongoing license management that scales with your organization.

---

*This document provides an overview of ZirconTech's third-party procurement capabilities. For detailed methodology and technical workflows, see our [Third-Party Tooling & Procurement Process](third-party-procurement.md). For vendor evaluation details, see our [Vendor Capability Matrix](vendor-matrix.md).*
