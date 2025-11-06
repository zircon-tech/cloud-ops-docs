
# ZirconTech Resource Tagging Strategy

## 1 · Purpose
Establish a consistent tagging framework so that cost allocation, governance, automation, and security tooling can unambiguously identify every AWS resource.

## 2 · Deriving the Strategy

| Step | Activity | Output |
|------|----------|--------|
| 1. Stakeholder Discovery | Workshops with Finance, SecOps, DevOps, BU owners | Draft business-domain matrix |
| 2. Tag Dictionary Draft  | Convert matrix to canonical tag keys, value regex, ownership | `tag-dictionary.yaml` |
| 3. Pilot & Feedback      | Apply tags to non-prod accounts with IaC and Config rules | Pilot report |
| 4. Finalize & Publish    | Approve dictionary; store in Git under `governance/tagging` | v1.0 release |
| 5. Continuous Review     | Quarterly tag review meeting; propose changes via PR | Updated dictionary |

### 2.1 Tag Dictionary (excerpt)

| Tag Key          | Allowed Values / Format            | Owner           | Use Cases |
|------------------|------------------------------------|-----------------|-----------|
| `CostCenter`     | `^CC-[0-9]{4}$`                    | Finance         | Chargeback |
| `Environment`    | `Prod\|Test\|Dev\|Sandbox`         | DevOps          | Policy routing |
| `OwnerEmail`     | valid email regex                  | Resource owner  | Alerts, approvals |
| `Project`        | Free-text ≤ 32 chars (guidelines)  | PMO             | Grouping, budgets |
| `Compliance`     | `PCI\|HIPAA\|None`                 | SecOps          | Guardrails |

Full dictionary lives in **`governance/tagging/tag-dictionary.yaml`** and is referenced by validation tooling.

## 3 · Implementation Tactics

### 3.1 Infrastructure as Code
* Modules require tag inputs; Terraform `pre-commit` hook enforces presence.
* CDK stacks contain a `TaggingAspect` that applies mandatory tags automatically.

### 3.2 Validation Gates in CI
```yaml
# GitHub Actions snippet
- name: Tag Linter
  uses: bridgecrewio/checkov-action@v12
  with:
    directory: ./infra
    framework: terraform
    guidelines-file: governance/tagging/tag-dictionary.yaml
```

### 3.3 Deployment-Time Guardrails

| Tool                                | What it Blocks                          | How                                        |
| ----------------------------------- | --------------------------------------- | ------------------------------------------ |
| **AWS Tag Policies**                | Missing or misspelled tag keys          | Enforced at Org root                       |
| **Service Control Policies (SCPs)** | Critical resources without `CostCenter` | Deny `ec2:RunInstances` unless tag present |
| **AWS Config Rules**                | Non-compliant values post-deployment    | Managed rule `required-tags` + custom      |
| **Control Tower**                   | Account-level mandatory tags            | Lifecycle hook applies defaults            |

### 3.4 Post-Deployment Remediation

* **AWS Config + EventBridge + Lambda** auto-adds `OwnerEmail` when missing.
* Nightly **Steampipe** report lists non-tagged resources → Jira ticket.

### 3.5 Visibility & Reporting

* **AWS Cost Explorer** activated for `CostCenter` & `Project`.
* **Resource Groups** saved for each Environment tag, powering OpsCenter dashboards.

## 4 · Governance Process

1. **Pull-request workflow** for any tag dictionary change (2-reviewer rule).
2. **Quarterly audit**: Config compliance score must stay ≥ 95 %.
3. **FinOps tie-in**: Budgets and anomaly alerts scoped by `CostCenter`.
4. **Exception path**: Temporary tag overrides documented in ITSM ticket.

## 5 · Deliverables

* Tag Dictionary (`tag-dictionary.yaml`, Markdown table, and JSON schema).
* CI tag-linter config and sample pipeline code.
* SCP & Tag Policy JSON files (`policies/`).
* Runbook: “Remediate non-tagged resources.”
* Audit evidence: quarterly compliance report (PDF).
