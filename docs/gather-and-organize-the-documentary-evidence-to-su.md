---
id: COCCA-006
title: "Gather and Organize Documentary Evidence to Support Audit Assessments"
---

# Gather and Organize Documentary Evidence to Support Audit Assessments

## Overview

The AWS Partner has methodology, process and relevant tooling experience to enable customers to adhere to major compliance controls such as FedRAMP, FIPS 140-2, FISMA, GDPR, HIPAA/HITECH, ISO27001, ITAR, NIST 800-171, PCI-DSS, and SOC1/2, with mechanisms to implement continuous monitoring to identify and remediate non-compliances.

## Evidence Documentation

### 1. Compliance Mechanisms and Audit Support

#### Automated Evidence Collection

We implement automated evidence collection using AWS Config for configuration compliance, CloudTrail for API activity logging, and AWS Security Hub for security finding aggregation. Our evidence collection methodology includes continuous monitoring, automated documentation generation, and centralized evidence repositories.

Evidence collection includes configuration snapshots, policy compliance reports, access logs, and security assessment results. We implement automated evidence validation, timestamping, and integrity verification to ensure audit trail reliability and regulatory compliance.

#### Documentation Management Framework

We deliver documentation management using AWS Systems Manager Documents for policy documentation, S3 buckets for evidence storage with versioning, and AWS CloudFormation templates for infrastructure documentation. Our framework includes automated policy generation, compliance reporting, and audit trail maintenance.

Documentation organization includes policy libraries, control implementation guides, evidence inventories, and compliance dashboards. We implement automated report generation, exception tracking, and remediation documentation to support internal and external audit requirements.

#### Continuous Monitoring and Remediation

We implement continuous monitoring using AWS Config rules for compliance validation, CloudWatch for real-time monitoring, and AWS Lambda for automated remediation. Our monitoring approach includes real-time compliance checking, automated alerting, and remediation workflow execution.

Monitoring capabilities include policy violation detection, configuration drift identification, and security finding correlation. We deliver automated remediation workflows, exception handling, and compliance reporting to maintain continuous compliance posture.

### 2. Supported Compliance Standards

#### NIST 800-171 (Controlled Unclassified Information)

We implement NIST 800-171 compliance through access control mechanisms, audit logging, configuration management, and security monitoring. Our approach includes multi-factor authentication, encryption implementation, and continuous monitoring for federal contractor requirements.

**Implementation Coverage:**
- Access Control (AC): IAM policies, MFA enforcement, least privilege access
- Audit and Accountability (AU): CloudTrail logging, log retention, security monitoring
- Configuration Management (CM): AWS Config, Systems Manager, change control
- Identification and Authentication (IA): IAM, Active Directory integration, MFA
- System and Communications Protection (SC): Encryption, network segmentation, TLS

**Exceptions:**
- Physical security controls (PE) require customer facility implementation
- Personnel security (PS) requires customer HR process implementation

#### SOC 2 Type II (Service Organization Control)

We implement SOC 2 Type II compliance through security monitoring, availability controls, processing integrity, and confidentiality measures. Our approach includes continuous monitoring, incident response, and operational effectiveness validation.

**Implementation Coverage:**
- Security: AWS Security Hub, GuardDuty, access controls, vulnerability management
- Availability: Multi-AZ deployment, auto-scaling, disaster recovery, monitoring
- Processing Integrity: Data validation, error handling, monitoring, logging
- Confidentiality: Encryption, access controls, data classification, secure transmission

**Exceptions:**
- Privacy controls require customer data handling policy implementation
- Certain operational controls require customer business process alignment

### 3. AWS Services and Custom Configuration Rules

#### AWS Config Implementation

**Core AWS Config Rules:**
- `required-tags`: Ensures mandatory tags are present on resources
- `encrypted-volumes`: Validates EBS volumes are encrypted
- `s3-bucket-public-access-prohibited`: Prevents public S3 bucket access
- `root-access-key-check`: Monitors root account access key usage
- `mfa-enabled-for-iam-console-access`: Validates MFA for console access

**Custom Configuration Rules Examples:**

```python
# Custom rule: Ensure RDS instances use approved instance types
def lambda_handler(event, context):
    config = boto3.client('config')
    rds = boto3.client('rds')
    
    # Approved instance types list
    approved_types = ['db.t3.micro', 'db.t3.small', 'db.r5.large']
    
    # Get RDS instances
    instances = rds.describe_db_instances()
    
    for instance in instances['DBInstances']:
        compliance_type = 'COMPLIANT' if instance['DBInstanceClass'] in approved_types else 'NON_COMPLIANT'
        
        config.put_evaluations(
            Evaluations=[{
                'ComplianceResourceType': 'AWS::RDS::DBInstance',
                'ComplianceResourceId': instance['DBInstanceIdentifier'],
                'ComplianceType': compliance_type,
                'OrderingTimestamp': datetime.utcnow()
            }],
            ResultToken=event['resultToken']
        )
```

```python
# Custom rule: Validate security group ingress rules
def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    config = boto3.client('config')
    
    # Get security groups
    security_groups = ec2.describe_security_groups()
    
    for sg in security_groups['SecurityGroups']:
        compliant = True
        
        for rule in sg['IpPermissions']:
            # Check for unrestricted access (0.0.0.0/0)
            for ip_range in rule.get('IpRanges', []):
                if ip_range.get('CidrIp') == '0.0.0.0/0':
                    compliant = False
                    break
        
        compliance_type = 'COMPLIANT' if compliant else 'NON_COMPLIANT'
        
        config.put_evaluations(
            Evaluations=[{
                'ComplianceResourceType': 'AWS::EC2::SecurityGroup',
                'ComplianceResourceId': sg['GroupId'],
                'ComplianceType': compliance_type,
                'OrderingTimestamp': datetime.utcnow()
            }],
            ResultToken=event['resultToken']
        )
```

#### Additional AWS Services

**AWS CloudTrail:**
- API activity logging for audit trails
- Log file validation and integrity checking
- Multi-region trail configuration
- Integration with CloudWatch Logs for real-time monitoring

**AWS Security Hub:**
- Centralized security finding aggregation
- Compliance standard integration (CIS, PCI DSS, AWS Foundational Security Standard)
- Custom insight creation for compliance reporting
- Integration with AWS Config for compliance dashboard

**AWS Systems Manager:**
- Patch compliance monitoring and reporting
- Configuration compliance validation
- Document-based policy management
- Automation for remediation workflows

#### Third-Party Solution Integration

**Splunk Enterprise Security:**
- Log aggregation and correlation for compliance reporting
- Custom dashboards for regulatory compliance monitoring
- Automated alert generation for policy violations
- Integration with AWS services for comprehensive visibility

**Rapid7 InsightVM:**
- Vulnerability assessment and management
- Compliance reporting for security standards
- Risk-based prioritization for remediation
- Integration with AWS Security Hub for finding correlation

**Qualys VMDR:**
- Continuous vulnerability management
- Compliance scanning and reporting
- Asset inventory and classification
- Integration with AWS Config for compliance validation

## Implementation Approach

Implementation begins with compliance framework selection, followed by evidence collection system deployment and continuous monitoring establishment. Our approach includes policy documentation, control implementation, and audit preparation to ensure regulatory compliance.

Service delivery includes compliance assessment, evidence collection automation, and audit support documentation. We implement continuous monitoring, automated reporting, and remediation workflows to maintain ongoing compliance posture.

## Success Metrics

Compliance effectiveness includes audit trail completeness at 100%, evidence collection automation above 95%, and compliance reporting accuracy above 98%. Audit support metrics include evidence retrieval time under 24 hours and compliance dashboard availability above 99.9%.

Operational metrics include automated remediation success rate above 90%, compliance drift detection within 15 minutes, and audit preparation time reduction by 70%. Success measurement ensures sustained compliance while supporting business objectives.

---

*This document provides evidence of our documentary evidence gathering and organization capabilities including compliance mechanisms, specific compliance standards support, and AWS services with custom configuration rules for audit assessments.*
