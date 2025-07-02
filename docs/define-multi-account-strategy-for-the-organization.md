---
id: COBL-001
title: "Define multi account strategy for the organization"
---

# Define multi account strategy for the organization

## Purpose

Establishing a robust define multi account strategy for the organization is fundamental to enterprise AWS success. Our methodology provides a structured approach that transforms ad-hoc cloud usage into a governed, scalable environment that supports innovation while maintaining security and compliance standards.

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


## 1. Define multi account strategy for the organization Methodology and Process

### Discovery Phase

**Stakeholder Engagement**: Collaborative workshops with technical teams, business stakeholders, and decision-makers to understand current state, requirements, and success criteria.

**Current State Assessment**: Comprehensive evaluation of existing baseline capabilities, identifying gaps, opportunities, and constraints.

**Requirements Analysis**: Documentation of functional and non-functional requirements aligned with business objectives and compliance needs.

### Design Phase

**Solution Architecture**: Design of target state architecture incorporating AWS best practices, security requirements, and scalability considerations.

**Implementation Planning**: Detailed project plan with phases, milestones, dependencies, and resource allocation.

**Risk Assessment**: Identification and mitigation strategies for technical, operational, and business risks.

### Implementation Phase

**Iterative Deployment**: Phased implementation approach with regular checkpoints and validation gates.

**Testing and Validation**: Comprehensive testing including functional, performance, security, and user acceptance testing.

**Documentation and Training**: Knowledge transfer through documentation, training sessions, and hands-on workshops.

### Operations Phase

**Monitoring and Support**: Ongoing monitoring, incident response, and continuous improvement processes.

**Optimization**: Regular reviews and optimization recommendations based on usage patterns and performance metrics.


## 2. Statement of Work: Define multi account strategy for the organization Implementation

### Project Overview

**Objective**: Implement comprehensive define multi account strategy for the organization solution using AWS native services and industry best practices.

**Scope**: End-to-end baseline implementation including design, deployment, testing, and knowledge transfer.

**Duration**: 8-12 weeks depending on complexity and requirements

### Deliverables

#### Phase 1: Discovery and Design (2-3 weeks)
- Current state assessment and gap analysis
- Target state architecture design
- Implementation roadmap and project plan
- Risk assessment and mitigation strategies

#### Phase 2: Implementation (4-6 weeks)
- Infrastructure deployment and configuration
- Service integration and testing
- Security implementation and validation
- Performance optimization and tuning

#### Phase 3: Testing and Validation (1-2 weeks)
- Functional and integration testing
- Performance and security testing
- User acceptance testing
- Documentation review and finalization

#### Phase 4: Knowledge Transfer (1 week)
- Technical documentation handover
- Training sessions for operations teams
- Runbook development and review
- Go-live support and transition

### Success Criteria

- **Functionality**: All requirements implemented and validated
- **Performance**: Meets or exceeds performance benchmarks
- **Security**: Passes security review and compliance audit
- **Operations**: Teams trained and ready for production support

### Assumptions and Dependencies

- Customer provides necessary access and resources
- Existing infrastructure meets minimum requirements
- Stakeholders available for requirements gathering and validation
- Change management processes followed for production deployment



## Implementation Phases

| Phase | Duration | Key Activities | Deliverables |
|-------|----------|----------------|--------------|
| 1. Discovery | 1-2 weeks | Requirements gathering, current state assessment | Discovery document, requirements matrix |
| 2. Design | 2-3 weeks | Architecture design, tool selection, process definition | Design document, implementation plan |
| 3. Implementation | 3-6 weeks | Deployment, configuration, testing, validation | Working solution, documentation |
| 4. Knowledge Transfer | 1 week | Training, handover, ongoing support planning | Training materials, runbooks |

## Deliverables

1. **Define multi account strategy for the organization Methodology Document** (this document)
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

[1] [Design principles for your multi-account strategy](https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/design-principles-for-your-multi-account-strategy.html)
[2] [Organizing Your AWS Environment Using Multiple Accounts](https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/organizing-your-aws-environment.html)

---

*Last updated: 02 Jul 2025*
