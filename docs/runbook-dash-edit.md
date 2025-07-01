# Runbook — Safe Edit & Deployment of Grafana / QuickSight Dashboards

_Last updated: 30 Jun 2025_

---

## 1 · Purpose  
Ensure dashboard changes are version-controlled, peer-reviewed, and deployed without affecting production visibility.

---

## 2 · Preconditions

* You have **Developer** or **Admin** rights in the staging Grafana workspace (or QuickSight dev namespace).  
* `git`, `jq`, and AWS CLI configured with *developer* IAM role.  
* API key **“Provisioner”** (read-only in prod; write in staging).

---

## 3 · Procedure

### Step 1 – Create a Feature Branch
```bash
git checkout -b feat/dashboard-latency-drilldown
````

### Step 2 – Export Current Dashboard JSON

```bash
# For Grafana
curl -s -H "Authorization: Bearer $GRAFANA_DEV_API_KEY" \
  https://grafana-dev.example.com/api/dashboards/uid/exec-score | \
  jq '.dashboard' > dashboards/exec-scorecard.json
```

### Step 3 – Edit Locally

* Use VS Code with **Grafana JSON** plugin or QuickSight visual editor.
* Follow colour conventions (green < warning < red).
* Add panel description and datasource UID.

### Step 4 – Validate in Staging

1. Import JSON to staging workspace.
2. Load live data; verify no datasource errors.
3. Execute alert test-fire (Grafana → *Alerting > Test rule*).

### Step 5 – Commit & Push

```bash
git add dashboards/exec-scorecard.json
git commit -m "Add latency drill-down panel"
git push --set-upstream origin feat/dashboard-latency-drilldown
```

### Step 6 – Pull Request & Review

* Request **2 reviewers** (FinOps + SRE).
* Address any comments; re-push if needed.
* Upon approval, merge to `main`.

### Step 7 – CI/CD Deployment

* Merge triggers CodeBuild job **`deploy-grafana-dash`**:

  1. Reads JSON from `main`.
  2. Calls Grafana Prod API to **upsert** dashboard.
  3. Posts Slack #dashboards “Deployed exec-scorecard.json v`$GIT_SHA`”.

### Step 8 – Post-Deploy Verification

* Refresh prod dashboard; confirm new panel appears.
* Validate alert rule fired test OK.

---

## 4 · Rollback

```bash
git revert <merge_commit_sha>
git push origin main      # triggers pipeline, restores prior JSON
```

---

## 5 · Audit & Logging

* **CodeBuild logs** – stored 30 days in CloudWatch.
* **Grafana API calls** – captured by CloudTrail and archived in S3 Glacier.
* PR history – retained indefinitely in GitHub.

---

## 6 · Contact (TBD)

| Role        | Slack        | Email                                           |
| ----------- | ------------ | ----------------------------------------------- |
| FinOps Lead | @finops-lead | [finops@example.com](mailto:finops@example.com) |
| SRE On-call | #on-call     | [sre@example.com](mailto:sre@example.com)       |

