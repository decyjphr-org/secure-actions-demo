# Security Hardening of GitHub Actions Workflows 

![CI](https://github.com/decyjphr-org/actions-demo/workflows/CI/badge.svg)


Github Actions can provision infra, deploy apps, create releases, access secrets, etc. Since anyone with write access could create an Action Workflow in a repo, we want to demonstrate examples of how to harden and secure workflows. Most of the examples listed here are based on the documentation for [security-hardening-for-github-actions](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions) 

Table of Contents

- Provide teams with Workflow templates of `blessed` workflows
- Show how workflow approval could be achieved using [environments]()
- Use CODEOWNERS to prevent unauthorized changes to workflows
- Control execution-only approved workflows
- Auditing Workflow Run Events.



## Auditing Workflow Run Events
The Audit Log now includes events associated with GitHub Actions workflow runs. This data provides enterprise customers with a greatly expanded data set for security and compliance audits.<sup>[blog](https://github.blog/changelog/2021-02-23-github-actions-workflow-run-events-are-now-included-in-the-audit-log/)</sup>

Various possible options for searching the audit log is explained here at [Reviewing the audit log for your organization
](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/reviewing-the-audit-log-for-your-organization#searching-the-audit-log)

### Viewing Workflow events in the UI
In that location above, you can find information on accessing the Audit Log at https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/reviewing-the-audit-log-for-your-organization#accessing-the-audit-log

For `Workflow` events, you can use the `actions` parameter to filter. Please see the screenshot below:

![image](https://user-images.githubusercontent.com/57544838/112735417-2c4f8100-8f22-11eb-8f18-63de02df0c09.png)


### Exporting the Audit Log
In that location above, you can find information on exporting the  Audit Log at https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/reviewing-the-audit-log-for-your-organization#exporting-the-audit-log

### Using Audit Log API
In that location above, you can find information on using Audit Log API at https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/reviewing-the-audit-log-for-your-organization#using-the-audit-log-api
For `Workflow` events, you can use the `phrase`  with the `action` parameter to filter. For example, the followinf query `https://api.github.com/organizations/65230155/audit-log?phrase=action:worflows` will produce the result as shown below:

![image](https://user-images.githubusercontent.com/57544838/112736273-de3d7c00-8f27-11eb-9228-e7fff8713d78.png)
