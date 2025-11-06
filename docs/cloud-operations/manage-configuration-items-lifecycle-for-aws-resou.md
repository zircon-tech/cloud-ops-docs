---
id: COCOM-002
title: "Manage Configuration Items Lifecycle for AWS Resources"
---

# Manage Configuration Items Lifecycle for AWS Resources

## Overview

The AWS Partner has methodology, process and relevant tooling experience to enable customers to identify applicable CIs to track, plan and implement CMDB solutions, and incorporate ticketing tools to drive changes through controlled processes.

## Evidence Documentation

### 1. Service Offering for CMDB Configuration

#### CMDB Implementation Services

We implement CMDB solutions using AWS Config as the foundational service for configuration item discovery and tracking. Our methodology includes integration with existing CMDB systems like ServiceNow, BMC Remedy, or implementation of new CMDB solutions based on customer requirements.

Our approach includes automated CI discovery using AWS Systems Manager Inventory, AWS Config resource tracking, and AWS CloudFormation stack analysis. We establish data synchronization processes between AWS services and CMDB systems using AWS Lambda functions and API integrations.

#### CMDB Evolution Services

We assess existing CMDB capabilities and design integration patterns to incorporate cloud configuration items. Our methodology includes data mapping between AWS resource attributes and CMDB CI classes, establishing automated synchronization workflows, and implementing change tracking processes.

Integration approach includes RESTful API connections, event-driven updates using CloudWatch Events, and batch synchronization processes. We establish CI relationships mapping between AWS resources and existing infrastructure components in customer CMDB systems.

#### Ticketing Integration Services

We integrate AWS change processes with existing ticketing systems including ServiceNow, Jira, BMC Remedy, and other ITSM platforms. Our methodology includes automated ticket creation for infrastructure changes, approval workflows for critical resources, and automated status updates.

Integration implementation utilizes AWS Systems Manager Change Calendar for maintenance windows, automated ticket creation via API calls from CloudWatch alarms, and bidirectional synchronization between AWS automation and ticketing systems.

### 2. Resource Tracking Classification

#### Tracked Resources

**Compute Resources:**
- EC2 Instances (all states)
- Auto Scaling Groups
- ECS Clusters and Services
- EKS Clusters
- Lambda Functions
- Elastic Beanstalk Applications

**Storage Resources:**
- EBS Volumes
- S3 Buckets (with policies and configurations)
- EFS File Systems
- FSx File Systems

**Network Resources:**
- VPCs
- Subnets
- Security Groups
- Network ACLs
- Load Balancers (ALB, NLB, CLB)
- NAT Gateways

**Database Resources:**
- RDS Instances
- DynamoDB Tables
- ElastiCache Clusters
- Redshift Clusters

**Security and Identity Resources:**
- IAM Roles, Users, Policies
- KMS Keys
- Secrets Manager Secrets
- ACM Certificates

#### Non-Tracked Resources

**Transient Resources:**
- CloudWatch Logs (individual log entries)
- Temporary EC2 Spot Instances (under 1 hour lifecycle)
- Lambda execution environments
- Auto Scaling temporary instances during scaling events

**Automatically Managed Resources:**
- Default VPC components (unless customized)
- AWS service-linked roles
- Default security groups (unless modified)
- Automatic EBS snapshots created by AWS services

**Low-Value Resources:**
- Individual CloudWatch metrics
- Route 53 health check details
- CloudFront edge locations
- S3 individual object metadata (unless business critical)

### 3. CI Recommendation Methodology

#### Environment-Based CI Prioritization

| Environment | Criticality Level | Resources Tracked | Change Control | Monitoring | Relationship Mapping |
|-------------|-------------------|-------------------|----------------|------------|---------------------|
| **Production** | High | All compute, storage, network, and database resources | Mandatory ticketing integration with approval workflows | Real-time synchronization with CMDB (within 5 minutes) | Complete dependency tracking between all resources |
| **Staging** | Medium | Core infrastructure and application resources | Automated ticketing for major changes, manual for minor changes | Hourly synchronization with CMDB | Application-tier dependency tracking |
| **Development/Test** | Low | Infrastructure backbone and shared resources | Notification-only for tracking purposes | Daily synchronization with CMDB | Basic resource grouping and ownership tracking |

#### CI Selection Methodology

**Business Impact Assessment:**
- Revenue impact analysis for each resource type
- Compliance requirements evaluation (SOX, HIPAA, PCI-DSS)
- Operational dependency mapping
- Cost optimization tracking requirements

**Technical Criticality Analysis:**
- Single points of failure identification
- Cross-environment dependency analysis
- Security boundary component identification
- Performance bottleneck resource analysis

**Operational Requirements:**
- Change frequency analysis
- Incident correlation requirements
- Capacity planning data needs
- Cost allocation and chargeback requirements

#### Implementation Approach

**Discovery Phase:**
1. Automated resource discovery using AWS Config and Systems Manager
2. Business stakeholder interviews for criticality assessment
3. Existing CMDB schema analysis and mapping
4. Change management process evaluation

**Configuration Phase:**
1. CMDB CI class definition and attribute mapping
2. Automated synchronization workflow implementation
3. Change tracking and approval process integration
4. Monitoring and alerting configuration

**Validation Phase:**
1. Data accuracy verification between AWS and CMDB
2. Change process testing and validation
3. Performance impact assessment
4. User training and knowledge transfer

---

*This document provides evidence of our configuration items lifecycle management capabilities including CMDB configuration services, resource tracking classification, and CI recommendation methodology.*
