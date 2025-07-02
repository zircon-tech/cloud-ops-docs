---
id: COBL-004
title: "Implement and enforce tagging strategy to support business needs"
---

# Implement and enforce tagging strategy to support business needs

## Purpose

Establishing a robust implement and enforce tagging strategy to support business needs is fundamental to enterprise AWS success. Our methodology provides a structured approach that transforms ad-hoc cloud usage into a governed, scalable environment that supports innovation while maintaining security and compliance standards.

## Methodology & Process

### Discovery Phase

Our discovery process begins with collaborative workshops that bring together key stakeholders—architects, security teams, and business leaders—to understand your current state and future objectives.

**Current State Assessment**: We catalog existing AWS resources, identify compliance requirements, and map organizational structures that influence technical decisions. This assessment reveals patterns in resource usage and highlights opportunities for consolidation or enhanced security.

**Requirements Gathering**: Understanding your specific needs for scalability, compliance, and operational processes helps us design solutions that align with business objectives rather than imposing generic patterns.

### Design and Planning

**Architecture Design**: Based on discovery findings, we create detailed architectural patterns that balance security, operational efficiency, and long-term maintainability. These designs prioritize practical implementation over theoretical perfection.

**Implementation Roadmap**: We develop phased implementation plans that minimize risk while delivering incremental value. Each phase includes clear success criteria and rollback procedures.

### Implementation and Validation

**Automated Deployment**: All configurations are deployed through infrastructure-as-code pipelines that ensure consistency and enable reliable rollback. Manual configuration steps are eliminated to prevent drift and human error.

**Continuous Monitoring**: Implementation includes comprehensive monitoring and alerting that provide early warning of configuration drift or compliance violations.

### Evidence Artifacts Included

**Discovery Worksheets**: Multi-account strategy assessment templates and completed examples
**Architecture Diagrams**: Reference landing zone designs with OU structures and account hierarchies  
**Automation Scripts**: Account Factory deployment code and Control Tower setup automation
**Process Runbooks**: Step-by-step procedures for account provisioning and governance

## Technology Stack

| Layer | AWS Services | Alternative Options |
|-------|--------------|--------------------|
| **Core** | AWS Organizations, AWS Control Tower, AWS IAM Identity Center, AWS CloudFormation | |
| **Security** | AWS Config, AWS CloudTrail, AWS GuardDuty, Amazon VPC | |
| **Automation** | AWS CodePipeline, AWS CodeCommit, AWS Lambda, Amazon EventBridge | |
| **Third-Party** | — | Terraform (Infrastructure as Code), GitLab CI (Pipeline automation), Azure AD (Identity federation) |



## Implementation Phases

| Phase | Duration | Key Activities | Deliverables |
|-------|----------|----------------|--------------|
| 1. Discovery | 1-2 weeks | Requirements gathering, current state assessment | Discovery document, requirements matrix |
| 2. Design | 2-3 weeks | Architecture design, tool selection, process definition | Design document, implementation plan |
| 3. Implementation | 3-6 weeks | Deployment, configuration, testing, validation | Working solution, documentation |
| 4. Knowledge Transfer | 1 week | Training, handover, ongoing support planning | Training materials, runbooks |

## Deliverables

1. **Implement and enforce tagging strategy to support business needs Methodology Document** (this document)
2. **Implementation Runbook** (see Implementation Artifacts section)
3. **Infrastructure as Code** templates (see Implementation Artifacts section)
4. **Configuration Standards** and baseline policies (see Implementation Artifacts section)
5. **Multi-Account Landing Zone** deployment artifacts
6. **Account vending pipeline** code and documentation
7. **Organizational Unit (OU) structure** diagram and rationale
8. **Knowledge Transfer Session** recording and materials

## Implementation Artifacts


## Implementation Runbook

### Step 1: AWS Organizations Setup

```bash
# Enable AWS Organizations
aws organizations create-organization --feature-set ALL

# Create Organizational Units
aws organizations create-organizational-unit \
    --parent-id r-exampleid \
    --name "Production"

aws organizations create-organizational-unit \
    --parent-id r-exampleid \
    --name "Non-Production"
```

### Step 2: Control Tower Deployment

```bash
# Deploy Control Tower Landing Zone
aws controltower create-landing-zone \
    --version 3.0 \
    --manifest file://landing-zone-manifest.json
```

### Step 3: Account Factory Configuration

```yaml
# account-factory-template.yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Account Factory Template for Standard AWS Account'

Parameters:
  AccountName:
    Type: String
    Description: Name of the AWS Account
  AccountEmail:
    Type: String
    Description: Email address for the AWS Account

Resources:
  ManagedAccount:
    Type: AWS::ControlTower::ManagedAccount
    Properties:
      AccountName: !Ref AccountName
      AccountEmail: !Ref AccountEmail
      OrganizationalUnitName: Production
```


## Service Control Policy Templates

### Preventive Control: Deny Public S3 Buckets

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyPublicS3Buckets",
      "Effect": "Deny",
      "Action": [
        "s3:PutBucketAcl",
        "s3:PutBucketPolicy",
        "s3:PutBucketPublicAccessBlock"
      ],
      "Resource": "*",
      "Condition": {
        "Bool": {
          "s3:PublicReadAccess": "true"
        }
      }
    }
  ]
}
```

### Required MFA Policy

```json
{
  "Version": "2012-10-17", 
  "Statement": [
    {
      "Sid": "DenyAllExceptListedIfNoMFA",
      "Effect": "Deny",
      "NotAction": [
        "iam:CreateVirtualMFADevice",
        "iam:EnableMFADevice",
        "iam:GetUser",
        "iam:ListMFADevices",
        "iam:ListVirtualMFADevices",
        "iam:ResyncMFADevice",
        "sts:GetSessionToken"
      ],
      "Resource": "*",
      "Condition": {
        "BoolIfExists": {
          "aws:MultiFactorAuthPresent": "false"
        }
      }
    }
  ]
}
```

## References

- [AWS Multi-Account Strategy Best Practices](https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/organizing-your-aws-environment.html)
- [AWS Control Tower User Guide](https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html)

---

*Last updated: 02 Jul 2025*
