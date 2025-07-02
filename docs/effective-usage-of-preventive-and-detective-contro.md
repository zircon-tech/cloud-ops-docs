---
id: COCG-001
title: "Effective usage of preventive and detective controls"
---

# Effective usage of preventive and detective controls

## Purpose

Effective effective usage of preventive and detective controls requires systematic processes and automated controls that scale with your organization. Our approach implements preventive and detective controls that enforce compliance while enabling developer productivity.

## Methodology & Process

### Policy Development Methodology

**Compliance Mapping**: We begin by mapping your specific compliance requirements to AWS controls, creating a comprehensive matrix that links regulatory requirements to technical implementations.

**Control Design**: Each control is designed with both preventive and detective capabilities, using AWS-native services like Service Control Policies, AWS Config, and AWS Control Tower guardrails.

**Risk-Based Prioritization**: Controls are prioritized based on risk impact and implementation complexity, ensuring high-value protections are deployed first.

### Automated Enforcement

**Policy as Code**: All governance policies are defined in version-controlled code, enabling rigorous testing and change management processes that treat security controls with the same discipline as application code.

**Continuous Compliance**: Automated monitoring detects policy violations in real-time, with automatic remediation for low-risk violations and immediate alerting for high-risk scenarios.

### Evidence Artifacts Included

**Control Catalog**: Comprehensive library of preventive and detective controls with compliance mappings
**Policy Templates**: Service Control Policies (SCPs) and Config rules in JSON/YAML format
**Compliance Reports**: Sample control effectiveness reports and drift detection outputs
**Implementation Guides**: Step-by-step deployment procedures for each control type

## Technology Stack

| Layer | AWS Services | Alternative Options |
|-------|--------------|--------------------|
| **Core** | AWS Control Tower, AWS Organizations, AWS Config, Service Control Policies | |
| **Compliance** | AWS Security Hub, AWS Audit Manager, AWS CloudFormation Guard, AWS Systems Manager | |
| **Monitoring** | Amazon CloudWatch, AWS CloudTrail Lake, Amazon GuardDuty, AWS Well-Architected Tool | |
| **Third-Party** | — | Terraform Sentinel (Policy as Code), Prisma Cloud (Compliance monitoring), Lacework (Cloud security) |


## 1. Effective usage of preventive and detective controls Methodology and Process

### Discovery Phase

**Stakeholder Engagement**: Collaborative workshops with technical teams, business stakeholders, and decision-makers to understand current state, requirements, and success criteria.

**Current State Assessment**: Comprehensive evaluation of existing governance capabilities, identifying gaps, opportunities, and constraints.

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


## 2. Control Implementation Examples

### Example 1: S3 Bucket Encryption Control

**Control Type**: Preventive
**Implementation**: Service Control Policy (SCP)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyUnencryptedS3Objects",
      "Effect": "Deny",
      "Action": "s3:PutObject",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "s3:x-amz-server-side-encryption": ["AES256", "aws:kms"]
        }
      }
    }
  ]
}
```

**Deployment**: Applied via AWS Organizations to all accounts
**Lifecycle Management**: Version-controlled in Git, deployed via CI/CD pipeline

### Example 2: CloudTrail Monitoring Control

**Control Type**: Detective
**Implementation**: AWS Config Rule

```yaml
CloudTrailMonitoringRule:
  Type: AWS::Config::ConfigRule
  Properties:
    ConfigRuleName: cloudtrail-enabled-check
    Source:
      Owner: AWS
      SourceIdentifier: CLOUD_TRAIL_ENABLED
    Scope:
      ComplianceResourceTypes:
        - AWS::CloudTrail::Trail
```

**Deployment**: Deployed via CloudFormation StackSets
**Lifecycle Management**: Automated remediation via Lambda when non-compliant


## 3. Controls Catalog and Compliance Framework Support

### Preventive Controls

| Control ID | Description | Implementation | Compliance Framework |
|------------|-------------|----------------|---------------------|
| **PC-001** | Prevent public S3 bucket access | Service Control Policy | CIS, NIST, SOC 2 |
| **PC-002** | Enforce MFA for privileged access | IAM policies, Conditional access | CIS, PCI DSS, ISO 27001 |
| **PC-003** | Restrict instance types | Service Control Policy | CIS, NIST |
| **PC-004** | Enforce encryption at rest | KMS policies, S3 bucket policies | PCI DSS, HIPAA, SOX |
| **PC-005** | Network access restrictions | Security Groups, NACLs | CIS, NIST, ISO 27001 |

### Detective Controls

| Control ID | Description | Implementation | Compliance Framework |
|------------|-------------|----------------|---------------------|
| **DC-001** | Monitor root account usage | CloudTrail, CloudWatch alarms | CIS, NIST, SOC 2 |
| **DC-002** | Detect privilege escalation | AWS Config rules, GuardDuty | CIS, NIST, ISO 27001 |
| **DC-003** | Monitor data access patterns | CloudTrail, VPC Flow Logs | PCI DSS, HIPAA, SOX |
| **DC-004** | Configuration drift detection | AWS Config, Systems Manager | CIS, NIST, SOC 2 |
| **DC-005** | Security finding aggregation | Security Hub, Inspector | CIS, NIST, ISO 27001 |

### Supported Compliance Frameworks

- **CIS Controls**: AWS Foundations Benchmark v1.4
- **NIST Cybersecurity Framework**: Core functions implementation
- **PCI DSS**: Level 1 merchant compliance
- **SOC 2 Type II**: Trust services criteria
- **ISO 27001**: Information security management
- **HIPAA**: Healthcare information protection
- **SOX**: Financial reporting controls



## Implementation Phases

| Phase | Duration | Key Activities | Deliverables |
|-------|----------|----------------|--------------|
| 1. Discovery | 1-2 weeks | Requirements gathering, current state assessment | Discovery document, requirements matrix |
| 2. Design | 2-3 weeks | Architecture design, tool selection, process definition | Design document, implementation plan |
| 3. Implementation | 3-6 weeks | Deployment, configuration, testing, validation | Working solution, documentation |
| 4. Knowledge Transfer | 1 week | Training, handover, ongoing support planning | Training materials, runbooks |

## Deliverables

1. **Effective usage of preventive and detective controls Methodology Document** (this document)
2. **Implementation Runbook** (see Implementation Artifacts section)
3. **Infrastructure as Code** templates (see Implementation Artifacts section)
4. **Configuration Standards** and baseline policies (see Implementation Artifacts section)
5. **Control catalog** with compliance framework mapping
6. **Policy enforcement pipeline** code and configurations
7. **Compliance dashboard** templates and reports
8. **Knowledge Transfer Session** recording and materials

## Implementation Artifacts


## Governance Controls Implementation Runbook

### Step 1: Deploy AWS Config Rules

```bash
# Enable AWS Config
aws configservice put-configuration-recorder \
    --configuration-recorder name=default,roleARN=arn:aws:iam::ACCOUNT:role/aws-service-role/config.amazonaws.com/AWSServiceRoleForConfig

# Deploy required S3 bucket encryption rule
aws configservice put-config-rule \
    --config-rule '{
        "ConfigRuleName": "s3-bucket-ssl-requests-only",
        "Source": {
            "Owner": "AWS",
            "SourceIdentifier": "S3_BUCKET_SSL_REQUESTS_ONLY"
        }
    }'
```

### Step 2: Security Hub Controls

```yaml
# security-hub-standards.yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  SecurityHub:
    Type: AWS::SecurityHub::Hub
    Properties:
      Tags:
        - Key: Purpose
          Value: CentralSecurityFindings

  CISStandard:
    Type: AWS::SecurityHub::StandardsSubscription
    Properties:
      StandardsArn: !Sub 'arn:aws:securityhub:${AWS::Region}:${AWS::AccountId}:standard/cis-aws-foundations-benchmark/v/1.2.0'
    DependsOn: SecurityHub

  PCIStandard:
    Type: AWS::SecurityHub::StandardsSubscription  
    Properties:
      StandardsArn: !Sub 'arn:aws:securityhub:${AWS::Region}:${AWS::AccountId}:standard/pci-dss/v/3.2.1'
    DependsOn: SecurityHub
```



## References

- [AWS Security Best Practices](https://docs.aws.amazon.com/security/)
- [AWS Compliance Resources](https://aws.amazon.com/compliance/resources/)

---

*Last updated: 02 Jul 2025*
