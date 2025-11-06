# Scope of Work  
**GitOps Infrastructure Deployment on AWS**

## 1. Objectives
* Implement a Git-centric workflow to define, test, and deploy all AWS infrastructure.  
* Enable customer teams to manage identities and permissions through code.

## 2. In-Scope Activities

| Phase | Tasks | Deliverables |
|-------|-------|--------------|
| Discovery | Requirements workshop; networking design worksheet | Approved requirements matrix |
| Foundation | Create repo, set up branching model, baseline modules | Git repo bootstrapped |
| CI Setup | Linting, unit tests, policy checks, artifact storage | CI pipeline operational |
| CD Setup | Multi-stage CodePipeline or GitHub Actions | Dev, Test, Prod pipelines |
| IAM as Code | Define roles, permission boundaries, SSO assignments | `infra/iam/*` modules |
| Knowledge Transfer | Pair-programming, runbooks, KT session | Docs + session recording |

## 3. Out-of-Scope
* Application code deployment  
* On-prem identity provider changes

## 4. Acceptance Criteria
* All infrastructure changes flow only through pull requests.  
* CI pipeline blocks merges failing lint, test, or policy checks.  
* Rollback can be executed from the pipeline UI without manual CLI steps.

## 5. Pricing & Timeline
Time-and-materials; target completion in six weeks. Milestone billing bi-weekly.

## 6. Change Control
Scope modifications require a written change order approved by both parties.
