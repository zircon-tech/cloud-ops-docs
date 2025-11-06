---
id: COCOM-007
title: "ITSM Integration"
---

# ITSM Integration

## Evidence Documentation

### 1. Methodology and Processes for ITSM Use-Cases

#### Self-Service and ITSM Workflow-Based Provisioning

We can implement self-service provisioning through AWS Service Catalog integrated with ITSM platforms to enable users to request AWS resources through familiar ITSM workflows. Our methodology includes creating service catalog portfolios with pre-approved AWS resource templates, integrating approval workflows with existing ITSM systems, and automating resource provisioning upon ITSM approval.

The provisioning process can include ITSM ticket creation for resource requests, automated approval routing based on cost and risk thresholds, resource provisioning through AWS Service Catalog upon approval, and automatic ticket updates with resource details and access information.

#### AWS Resource Visibility, Tracking, and Control

We can implement federated visibility by integrating AWS Config and AWS Systems Manager with ITSM configuration management databases (CMDB) to maintain synchronized records of AWS resources. Our methodology includes automated discovery and registration of AWS resources in ITSM CMDBs, real-time synchronization of resource state changes, and integration of AWS resource lifecycle events with ITSM workflows.

Resource tracking can include automated CMDB updates when AWS resources are created, modified, or terminated, integration with AWS CloudTrail for change auditing, and correlation of AWS resources with business services in ITSM platforms.

#### Cloud Operational Issue Detection and Resolution

We can implement incident and change management integration by connecting AWS monitoring services with ITSM platforms to automate issue detection and response workflows. Our methodology includes automated incident creation from AWS CloudWatch alarms, integration of AWS security findings with ITSM incident management, and automated change request generation for remediation activities.

The issue resolution process can include automatic incident creation from AWS service disruptions, escalation workflows based on service impact and urgency, automated correlation of AWS metrics with business service availability, and integration of AWS automation with ITSM change management for remediation approval and execution.

### 2. ITSM Integrations Supported

#### AWS Service Management Connectors

We can implement AWS Service Management Connector for ServiceNow which provides native integration for incident management, change management, and configuration management. The connector enables automatic synchronization of AWS Config data with ServiceNow CMDB, automated incident creation from AWS CloudWatch alarms, and integration of AWS resource provisioning with ServiceNow service catalog.

We can implement AWS Service Management Connector for Jira Service Management which provides integration for incident tracking, change management, and asset management. The connector enables automated ticket creation from AWS monitoring events and integration of AWS resource requests with Jira workflows.

#### Third-Party ITSM Connectors

We can integrate with ServiceNow through native AWS connectors and custom API integrations for comprehensive ITSM workflow automation including incident, problem, change, and configuration management. We can integrate with Jira and Jira Service Management through API-based connectors for issue tracking, workflow automation, and asset management.

We can integrate with BMC Remedy through web services API integration for incident, problem, and change management workflows. We can integrate with PagerDuty through native AWS integration for incident routing, escalation, and on-call management using CloudWatch Events and webhooks.

#### Custom-Built ITSM Integrations

We can develop custom integrations using AWS Lambda functions and API Gateway to connect AWS services with ITSM platforms that don't have native connectors. These custom integrations can include REST API connections for data synchronization, webhook implementations for real-time event processing, and batch processing for bulk data updates.

Custom integration capabilities include developing Lambda-based connectors for proprietary ITSM systems, creating API Gateway endpoints for bidirectional data exchange, implementing EventBridge rules for real-time event routing, and building Step Functions workflows for complex ITSM process automation.

We can implement custom integrations with monitoring platforms by developing connectors for log aggregation and analytics systems, creating custom metrics and dashboards for ITSM platforms, and implementing automated correlation between AWS metrics and ITSM incidents.

---

*This document provides evidence of our capability to deliver comprehensive ITSM integration methodologies and support for various ITSM platforms using AWS native connectors, third-party solutions, and custom-built integrations.*
