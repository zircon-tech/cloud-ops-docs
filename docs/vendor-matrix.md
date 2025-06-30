# Vendor Capability Matrix

| Vendor / Product | Category | Native AWS Integration | Compliance Certifications | Pricing Model | Notes |
|------------------|----------|------------------------|---------------------------|---------------|-------|
| **Wiz** | Cloud Security Posture Management (CSPM) | Ingests CloudTrail via AWS Lambdas; supports AWS Organizations auto-enrollment | SOC 2 Type II, ISO 27001, FedRAMP in process | SaaS, per-resource tier | Agentless 5-minute onboarding; strong attack-path graph |
| **Lacework** | CSPM + CWPP | CloudFormation and Terraform modules; supports Control Tower auto-tagging | SOC 2 Type II, ISO 27001 | SaaS, annual commit | Includes container vulnerability scanning |
| **Datadog** | Monitoring & Observability | AWS integrations via CloudFormation; AWS Marketplace SaaS | SOC 2 Type II, HIPAA, GDPR | SaaS, per-host + per-GB | Marketplace Private Offer eligible |
| **HashiCorp Terraform Cloud** | IaC Automation | Runs remote plans against AWS; S3 back-end supported | SOC 2 Type II | SaaS, user-based + run-based | SAML/SSO supported |
| **Pulumi Cloud** | IaC Automation | Native AWS provider; stacks map to AWS accounts | SOC 2 Type II | SaaS, per-user + per-stack | Works well for TypeScript/Golang teams |
| **Trend Micro Cloud One** | Workload Security | CloudFormation Quick Start; integrates with AWS Inspector findings | ISO 27001, PCI DSS, SOC 2 | SaaS, per-instance | Combines CSPM and AV |

*Last reviewed: 30 Jun 2025*

