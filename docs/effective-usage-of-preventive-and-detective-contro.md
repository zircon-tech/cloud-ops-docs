---
id: COCG-001
title: "Effective usage of preventive and detective controls"
---

# Effective Usage of Preventive and Detective Controls

## Overview

Implementing effective preventive and detective controls is essential for maintaining security, compliance, and operational excellence across AWS environments. ZirconTech provides comprehensive methodologies that establish automated, scalable control frameworks aligned with industry standards including CIS, NIST, PCI DSS, and HIPAA.

Our approach implements policy-as-code practices that treat security controls with the same rigor as application development, enabling consistent enforcement while maintaining developer productivity and business agility.

## Comprehensive Controls Framework

**For detailed methodology, control implementations, and compliance mappings**: See [ZirconTech Preventive & Detective Controls Framework](preventive-detective-controls.md)

**For complete control catalog and framework mappings**: See [Control–Framework Cross Reference](control-matrix.md)

### Methodology for Control Derivation

Our systematic approach ensures controls align with business requirements and compliance mandates:

- **Compliance Mapping**: Map customer requirements to specific control objectives with framework references
- **Control Type Decision**: Determine preventive vs. detective approach based on risk impact and feasibility
- **Tool Selection**: Prioritize AWS-native solutions (Control Tower, SCPs, Config Rules, GuardDuty)
- **Design and Review**: Create JSON/YAML artifacts with peer review through Git workflows
- **Automated Deployment**: Pipeline-driven deployment to target OUs and accounts
- **Continuous Improvement**: Monthly effectiveness reviews and drift detection

### Control Implementation Examples

#### Example 1: S3 Bucket Encryption (Preventive)
**Objective**: Enforce CIS 2.1 "Ensure S3 buckets require encryption at rest"  
**Implementation**: AWS Control Tower preventive guardrail  

```bash
# Deploy via Control Tower API
aws controltower enable-control \
   --control-identifier "arn:aws:controltower:us-east-1::control/AWS-GR_ENCRYPTED_BUCKET_BLOCK_UNENCRYPTED_OBJECT_UPLOADS" \
   --target-identifier "arn:aws:organizations::123456789012:ou/o-root/ou-Prod"
```

**Lifecycle Management**:
- Version-controlled guardrail configurations in Git with semantic versioning
- Monthly Terraform/CloudFormation drift detection with automated remediation
- Exception handling via change advisory process with time-bound approvals

#### Example 2: Public AMI Detection (Detective)
**Objective**: Address NIST 800-53 SC-7 "Boundary Protection"  
**Implementation**: AWS Config Rule + EventBridge + Lambda remediation

```hcl
resource "aws_config_config_rule" "ami_public_check" {
  name = "ami-public-detect"
  source {
    owner             = "AWS"
    source_identifier = "AMI_PUBLIC_CHECK"
  }
  input_parameters = jsonencode({
    WhitelistAccountIds = "123456789012"
  })
}

resource "aws_config_remediation_configuration" "ami_auto_fix" {
  config_rule_name = aws_config_config_rule.ami_public_check.name
  target_id        = aws_lambda_function.ami_remediate.arn
  target_type      = "LAMBDA"
  automatic        = true
}
```

**Lifecycle Management**:
- Lambda remediation code with comprehensive unit testing
- Weekly compliance dashboard review with 95%+ auto-remediation success rate KPI
- Service Control Policy backup enforcement for API-level protection

### Technology Foundation

| Component | Primary Services | Purpose |
|-----------|-----------------|---------|
| **Preventive Controls** | AWS Control Tower, Service Control Policies | Block non-compliant actions before execution |
| **Detective Controls** | AWS Config, AWS Config Rules | Monitor and detect compliance violations |
| **Compliance Standards** | AWS Security Hub, Conformance Packs | CIS, NIST, PCI DSS, HIPAA framework support |
| **Automation** | AWS Lambda, Amazon EventBridge | Automated remediation and notifications |
| **Monitoring** | Amazon CloudWatch, AWS Systems Manager | Continuous compliance monitoring |

## Controls Catalog and Compliance Support

### Preventive Controls Library

| Control ID | Description | Implementation | Compliance Frameworks |
|------------|-------------|----------------|---------------------|
| **CT-EncryptS3** | All S3 buckets encrypted at rest | Control Tower guardrail | CIS 2.1, NIST SC-28, PCI 3.4 |
| **SCP-DenyPublicS3** | Block public S3 bucket creation | Service Control Policy | CIS 2.1, NIST SC-7 |
| **SCP-RequireMFA** | Require MFA for privileged access | Service Control Policy | CIS 1.6, NIST IA-2 |
| **SCP-DenyOpenSG** | Block security groups with 0.0.0.0/0 | Service Control Policy | CIS 4.1, PCI 1.2 |

### Detective Controls Library

| Control ID | Description | Implementation | Compliance Frameworks |
|------------|-------------|----------------|---------------------|
| **Config-IAMKeyRotation** | Verify IAM keys rotated within 90 days | AWS Config managed rule | CIS 1.4, NIST IA-5 |
| **Config-RootMFAEnabled** | Alert if root account lacks MFA | AWS Config managed rule | CIS 1.6, NIST IA-2 |
| **Config-EBSEncrypted** | Ensure EBS snapshots encrypted | AWS Config managed rule | CIS 2.7, NIST SC-13 |
| **Config-SGOpenSSH** | Detect SGs open to 0.0.0.0/0 on port 22 | AWS Config custom rule | CIS 4.1, NIST SC-7 |

### Supported Compliance Frameworks

- **CIS Controls**: AWS Foundations Benchmark v1.4 with 28+ control mappings
- **NIST Cybersecurity Framework**: Core functions across all categories  
- **PCI DSS**: Level 1 merchant compliance requirements
- **SOC 2 Type II**: Trust services criteria implementation
- **ISO 27001**: Information security management controls
- **HIPAA**: Healthcare information protection safeguards

## Implementation Approach

### Discovery and Assessment
- Current state evaluation of existing controls and compliance gaps
- Stakeholder workshops to understand business requirements and risk tolerance
- Compliance framework mapping to technical control implementations
- Risk-based prioritization of high-impact controls

### Control Deployment
- AWS Control Tower guardrail enablement across organizational units
- Service Control Policy development and testing in non-production environments
- AWS Config Rules deployment via CloudFormation StackSets
- Security Hub standards subscription for automated compliance monitoring

### Governance and Operations
- Policy-as-code implementation with version control and peer review
- Automated compliance reporting and drift detection
- Exception management processes with time-bound approvals
- Continuous improvement through monthly effectiveness reviews

## Deliverables and Evidence Artifacts

### Control Framework Artifacts
- **Control Catalog**: Comprehensive library with compliance framework mappings
- **Policy Templates**: JSON/YAML artifacts for SCPs, Config Rules, and Control Tower guardrails
- **Compliance Matrix**: Detailed mapping of controls to CIS, NIST, PCI, HIPAA requirements
- **Implementation Guides**: Step-by-step deployment procedures for each control type

### Automation and Integration
- **CI/CD Pipeline**: Automated control deployment with testing and validation
- **Monitoring Dashboards**: Security Hub and CloudWatch dashboards for compliance visibility
- **Remediation Functions**: Lambda-based automatic remediation for detective controls
- **Compliance Reports**: Automated reporting with control effectiveness metrics

### Process Documentation
- **Control Derivation Methodology**: Framework for mapping requirements to technical controls
- **Exception Management Process**: Documented procedures for temporary control overrides
- **Lifecycle Management**: Version control, deployment, and maintenance procedures
- **Audit Evidence**: Quarterly compliance assessment reports with remediation tracking

## Success Criteria

- **95%+ Compliance**: Continuous Config rule compliance across all monitored controls
- **Automated Enforcement**: Zero-touch policy enforcement through preventive controls
- **Complete Coverage**: All critical compliance requirements mapped to technical controls
- **Operational Excellence**: Monthly control effectiveness reviews with continuous improvement

## Getting Started

Contact ZirconTech to implement comprehensive preventive and detective controls. Our proven methodology and automation frameworks ensure consistent, compliant security posture that scales with your organization while maintaining operational efficiency.

---

*This document provides an overview of ZirconTech's preventive and detective controls capabilities. For detailed methodology and technical implementations, see our [ZirconTech Preventive & Detective Controls Framework](preventive-detective-controls.md). For complete control mappings, see our [Control–Framework Cross Reference](control-matrix.md).*
