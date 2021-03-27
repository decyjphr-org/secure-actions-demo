# Reference Repository for Workflow Hardening and Approvals

![CI](https://github.com/decyjphr-org/actions-demo/workflows/CI/badge.svg)


Github Actions can provision infra, deploy apps, create releases, access secrets, etc. Since anyone with write access could create an Action Workflow in a repo, we want to:

- Provide teams with Workflow templates of `blessed` workflows
- Show how workflow approval could be achieved using [environments]()
- Use CODEOWNERS to prevent unauthorized changes to workflows
- Control execution-only approved workflows
- User the AuditLog to audit Workflow runs.


https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#considering-cross-repository-access



