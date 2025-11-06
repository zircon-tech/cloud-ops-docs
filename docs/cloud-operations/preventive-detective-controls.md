# ZirconTech Preventive & Detective Controls Framework

## 1 · Purpose  
Provide a structured process and toolset to design, deploy, and manage preventive (block) and detective (monitor + alert) controls that align with industry-standard frameworks such as CIS, NIST 800-53, PCI DSS, and HIPAA.

## 2 · Methodology to Derive Controls

| Phase | Activity | Output |
|-------|----------|--------|
| 1. Compliance Mapping | Map customer requirements → control objectives (e.g., “Encrypt data at rest”) | Control matrix (objective ↔ AWS service ↔ framework ref) |
| 2. Control Type Decision | Choose preventive vs. detective (or both) based on risk impact | Draft control catalogue |
| 3. Tool Selection | Pick AWS native option first—Control Tower, SCP, Config Rule, GuardDuty | Control-tool matrix |
| 4. Design & Review | Create JSON/YAML artifacts; peer review in Git PR | Approved control spec |
| 5. Automated Deployment | Pipeline applies control to target OUs/accounts | Version-tagged release |
| 6. Continuous Improvement | Monthly drift + effectiveness review | Updated control version|

All artifacts live in **`governance/controls/`**; each control has its own folder:
```

governance/controls/
├─ CT-EnableS3PVEncryption/
│   ├─ control.yml      # metadata
│   ├─ guardrail.json   # for Control Tower
│   └─ tests/...
└─ SCP-DenyPublicEC2/
├─ control.yml
└─ scp.json

```

## 3 · Example Controls

### 3.1 Preventive – “Block Unencrypted S3 Bucket Creation”
* **Objective**: Enforce CIS 2.1 “Ensure S3 buckets require encryption at rest.”
* **Tool**: *AWS Control Tower* preventive guardrail (`ControlIds`)  
  - `AWS-GR_ENCRYPTED_BUCKET_BLOCK_UNENCRYPTED_OBJECT_UPLOADS`  
* **Deployment**:  
  ```bash
  # Part of CodePipeline step
  aws controltower enable-control \
     --control-identifier "arn:aws:controltower:us-east-1::control/AWS-GR_ENCRYPTED_BUCKET_BLOCK_UNENCRYPTED_OBJECT_UPLOADS" \
     --target-identifier  "arn:aws:organizations::123456789012:ou/o-root/ou-Prod"
```

* **Lifecycle Management**

  * Guardrail versions tracked in Git, tagged `vMajor.Minor.Patch`.
  * Monthly Terraform/CloudFormation drift check; PR required for guardrail upgrade.
  * Exceptions handled via change-advisory ticket with 7-day expiry.

### 3.2 Detective – “Detect and Auto-Remediate Public EC2 AMIs”

* **Objective**: Address NIST 800-53 SC-7 “Boundary Protection” by preventing public AMI exposure.
* **Tools**:

  * *AWS Config* custom rule + *EventBridge* + *Lambda* remediation.
  * *SCP* backup control to block `ec2:ModifyImageAttribute` sharing with `all`.
* **Deployment** (Terraform snippet):

  ```hcl
  resource "aws_config_config_rule" "ami_public" {
    name = "ami-public-detect"
    source {
      owner             = "AWS"
      source_identifier = "AMI_PUBLIC_CHECK"
    }
    input_parameters = "{\"WhitelistAccountIds\":\"123456789012\"}"
    scope {
      compliance_resource_types = ["AWS::EC2::Instance"]
    }
    depends_on = [aws_config_configuration_recorder.rec]
  }

  resource "aws_lambda_function" "ami_remediate" { ... }

  resource "aws_config_remediation_configuration" "ami_fix" {
    config_rule_name = aws_config_config_rule.ami_public.name
    target_id        = aws_lambda_function.ami_remediate.arn
    target_type      = "LAMBDA"
    automatic        = true
  }
  ```
* **Lifecycle Management**

  * Lambda code tracked in `controls/AMI_PUBLIC/` with unit tests.
  * Config rule compliance dashboard reviewed weekly; auto-remediation success rate KPI ≥ 95 %.
  * SCP updated only when new EC2 API versions introduce attributes that could bypass the rule.

## 4 · Supported Controls & Frameworks

| Control Category     | Example Controls (IDs / filenames)                                   | Frameworks Covered\*         |
| -------------------- | -------------------------------------------------------------------- | ---------------------------- |
| Identity & Access    | `SCP-RequireMFA`, `Config-IAMKeyRotation`                            | CIS 1.x, NIST IA-5           |
| Network              | `SCP-DenyOpenSecurityGroups`, `CT-EnableVPCFlowLogs`                 | CIS 4.1, PCI DSS 2.2         |
| Data Protection      | `CT-EncryptS3`, `Config-RDSEncryption`                               | CIS 2.x, HIPAA 164.306       |
| Monitoring & Logging | `CT-EnableCloudTrail`, `Config-GuardDutyEnabled`                     | NIST AU-12, ISO 27001 A.12.4 |
| Resilience           | `Config-EBSSnapshotPublicCheck`, `SCP-DenyUnencryptedSnapshotsShare` | CIS 2.7, NIST CP-9           |

*See the full mapping in [Control–Framework Cross-Reference](control-matrix.md).*


---

## 5 · Deliverables

* Control catalogue (Markdown + JSON/YAML definitions).
* CI pipeline code for deployment, rollback, and drift detection.
* Compliance mapping spreadsheet linking each control to CIS, NIST, PCI, HIPAA.
* Quarterly effectiveness report template (PDF).

---

*Last updated: 30 Jun 2025*

