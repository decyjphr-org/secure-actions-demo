# Security Hardening of GitHub Actions Workflows 

![CI](https://github.com/decyjphr-org/actions-demo/workflows/CI/badge.svg)


Github Actions can provision infra, deploy apps, create releases, access secrets, etc. Since anyone with write access could create an Action Workflow in a repo, we want to:

- Provide teams with Workflow templates of `blessed` workflows
- Show how workflow approval could be achieved using [environments]()
- Use CODEOWNERS to prevent unauthorized changes to workflows
- Control execution-only approved workflows
- User the AuditLog to audit Workflow runs.


https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#considering-cross-repository-access


## Auditing Workflow Run Events
The Audit Log now includes events associated with GitHub Actions workflow runs. This data provides enterprise customers with a greatly expanded data set for security and compliance audits.<sup>[1](https://github.blog/changelog/2021-02-23-github-actions-workflow-run-events-are-now-included-in-the-audit-log/)</sup>

Various possible options for searching the audit log is explained here at [Reviewing the audit log for your organization
](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/reviewing-the-audit-log-for-your-organization#searching-the-audit-log)

### Viewing Workflow events in the UI
In that location above, you can find information on accessing the Audit Log at https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/reviewing-the-audit-log-for-your-organization#accessing-the-audit-log

For `Workflow`
![image](https://user-images.githubusercontent.com/57544838/112735417-2c4f8100-8f22-11eb-8f18-63de02df0c09.png)


### Exporting the Audit Log
In that location above, you can find information on exporting the  Audit Log at https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/reviewing-the-audit-log-for-your-organization#exporting-the-audit-log

### Using Audit Log API
In that location above, you can find information on using Audit Log API at https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/reviewing-the-audit-log-for-your-organization#using-the-audit-log-api
