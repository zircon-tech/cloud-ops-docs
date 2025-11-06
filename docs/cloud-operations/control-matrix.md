# Control–Framework Cross Reference

This table lists the preventive and detective controls ZirconTech deploys, the AWS tool used to enforce each control, and the compliance frameworks the control helps satisfy.

| Control ID                 | Name / Objective                                       | Type        | AWS Tooling                          | Primary Framework Mappings* |
|----------------------------|--------------------------------------------------------|-------------|--------------------------------------|-----------------------------|
| CT-EncryptS3               | All S3 buckets encrypted at rest                       | Preventive  | Control Tower guardrail              | CIS 2.1 • NIST SC-28 • PCI 3.4 |
| CT-EnableCloudTrail        | CloudTrail enabled in all regions                      | Preventive  | Control Tower guardrail              | NIST AU-12 • ISO A.12.4 • HIPAA 164.312(b) |
| CT-EnableVPCFlowLogs       | VPC Flow Logs enabled for all VPCs                     | Preventive  | Control Tower guardrail              | CIS 4.3 • NIST AC-17 • PCI 10.2 |
| CT-GuardDutyEnabled        | GuardDuty enabled and findings forwarded               | Preventive  | Control Tower guardrail              | CIS 4.4 • NIST SI-4 |
| CT-EnableConfig            | AWS Config recorder enabled organization-wide          | Preventive  | Control Tower guardrail              | CIS 2.5 • NIST CA-7 |
| SCP-DenyPublicS3           | Block creation of public S3 buckets                    | Preventive  | Service Control Policy               | CIS 2.1 • NIST SC-7 |
| SCP-DenyPublicEC2          | Block EC2 instances with public IP unless approved     | Preventive  | Service Control Policy               | CIS 4.1 • NIST SC-7 |
| SCP-DenyOpenSecurityGroups | Block security groups with 0.0.0.0/0 ingress on ports  | Preventive  | Service Control Policy               | CIS 4.1 • PCI 1.2 |
| SCP-RequireMFA             | Require MFA for root and console users                 | Preventive  | Service Control Policy               | CIS 1.6 • NIST IA-2 |
| SCP-DenyUnencryptedSnapshotsShare | Block sharing of unencrypted EBS snapshots      | Preventive  | Service Control Policy               | CIS 2.7 • NIST SC-28 |
| Config-IAMKeyRotation      | Verify IAM keys rotated within 90 days                 | Detective   | AWS Config managed rule              | CIS 1.4 • NIST IA-5 |
| Config-UnusedIamCreds      | Detect unused IAM credentials older than 90 days       | Detective   | AWS Config managed rule              | CIS 1.5 • NIST IA-4 |
| Config-RootMFAEnabled      | Alert if root account has no MFA                       | Detective   | AWS Config managed rule              | CIS 1.6 • NIST IA-2 |
| Config-EBSSnapshotEncrypted| Ensure EBS snapshots are encrypted                     | Detective   | AWS Config managed rule              | CIS 2.7 • NIST SC-13 |
| Config-RDSEncryption       | Ensure RDS instances encrypted at rest                 | Detective   | AWS Config managed rule              | CIS 2.6 • NIST SC-28 |
| Config-RDSBackupRetention  | Verify RDS backup retention ≥ 7 days                   | Detective   | AWS Config custom rule               | CIS 2.6 • NIST CP-9 |
| Config-SGOpenSSH           | Detect SGs open to 0.0.0.0/0 on port 22                | Detective   | AWS Config custom rule               | CIS 4.1 • NIST SC-7 |
| Config-S3PublicReadProhibited | Detect S3 buckets with public read ACL              | Detective   | AWS Config managed rule              | CIS 2.1 • NIST SC-7 |
| Config-S3Versioning        | Check S3 buckets have versioning enabled               | Detective   | AWS Config custom rule               | CIS 2.3 • NIST SI-12 |
| Lambda-SSMAgentInstall     | Auto-install SSM Agent on new EC2 instances            | Detective   | EventBridge + Lambda remediation     | CIS 4.2 • NIST CM-7 |

\* Column shows key framework references; full mapping lives in the individual control metadata files.

_Last updated: 30 Jun 2025_
