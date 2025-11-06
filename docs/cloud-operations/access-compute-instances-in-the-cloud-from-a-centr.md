---
id: COCOM-006
title: "Access Compute Instances in the Cloud from a Central Location"
---

# Access Compute Instances in the Cloud from a Central Location

## Evidence Documentation

### 1. Session Management Mechanism Implementation

We can implement secure session management using AWS Systems Manager Session Manager to establish browser-based or CLI-based secure shell sessions to EC2 instances without requiring SSH keys, bastion hosts, or open inbound ports.

**Session Management Implementation:**
We can configure AWS Systems Manager Session Manager to provide secure access to EC2 instances through the AWS Management Console, AWS CLI, or AWS SDK. Session Manager establishes secure sessions using TLS 1.2 encryption and routes traffic through AWS Systems Manager infrastructure.

**Session Logging and Auditing:**
We can implement comprehensive session logging by configuring Session Manager to log all session data to Amazon CloudWatch Logs and Amazon S3 for long-term storage and audit compliance. Session logs capture all commands executed during sessions, including input and output, with timestamps and user identification.

**Access Control:**
We can implement access control using IAM policies that define which users can start sessions and which instances they can access. Access permissions can be scoped by instance tags, specific instance IDs, or resource groups to ensure least privilege access.

**Central Management:**
We can implement centralized session management through AWS Systems Manager console where administrators can view active sessions, terminate sessions if needed, and monitor session history across all instances. Session preferences can be configured centrally including session timeout, idle timeout, and shell preferences.

**Audit Trail:**
We can implement complete audit trails using AWS CloudTrail to log all Session Manager API calls, including session start and terminate events, user identity, source IP addresses, and session duration. Combined with session logs, this provides comprehensive audit capabilities for compliance requirements.

---

*This document provides evidence of our capability to implement secure, auditable session management for compute instances using AWS Systems Manager Session Manager.*
