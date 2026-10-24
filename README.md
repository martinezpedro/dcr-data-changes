# data-changes (change repository template)

Holds **only** generated change folders: `changes/<domain>/<DCR-id>/r<revision>/{manifest.json,data.csv,preview.sql,evidence/}`.
No executable code lives here; the executor and policy come from the platform repo (pinned ref).

**NOT TESTED end to end**. The POC mocks GitHub; the app's `SimulatedGitHubActions` (app/dcr/ci.py)
performs the same steps in-process. Required repository settings:

* Branch protection on `main`: require PR, 1 approval from CODEOWNERS, **dismiss stale approvals**,
  **require approval of the most recent push**, required status checks `dcr/policy-lint` and
  `dcr/approval-gate`, no admin bypass, no force pushes.
* The DCR GitHub App may create branches/PRs and merge; it must not be a CODEOWNER.
* Environment `production` (deployment branch = `main`) with secrets: `SERVICENOW_INSTANCE_URL`,
  `SERVICENOW_USERNAME`, `SERVICENOW_PASSWORD`, `DATABRICKS_HOST`, `DCR_DEPLOY_SP_CLIENT_ID`
  (GitHub OIDC workload identity federation, so no Databricks secret is stored), `DCR_EXECUTOR_JOB_ID`.
* Variables: `DCR_APPROVER_GROUPS` (email → groups, until IdP group lookup is decided),
  `DCR_GITHUB_LOGIN_MAP` (GitHub login → corporate email, until EMU/SAML mapping is decided).
