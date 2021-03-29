# Hardening of GitHub Actions Workflows 

![CI](https://github.com/decyjphr-org/actions-demo/workflows/CI/badge.svg)

Github Actions can provision infra, deploy apps, create releases, access secrets, etc. Since anyone with write access could create an Action Workflow in a repo, we want to demonstrate examples of how to harden and secure workflows. Most of the examples listed here are based on the documentation for [security-hardening-for-github-actions](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions) 

These are the topics that are covered in this repo:

- [Hardening of GitHub Actions Workflows](#hardening-of-github-actions-workflows)
  - [Protecting workflow runs and secrets using Environments](#protecting-workflow-runs-and-secrets-using-environments)
  - [Setting Enviroment protection rules and required approvers](#setting-enviroment-protection-rules-and-required-approvers)
  - [Preventing unauthorized workflow runs](#preventing-unauthorized-workflow-runs)
  - [Organization or Enterprise level workflows](#organization-or-enterprise-level-workflows)
  - [Svanboxel Organization Workflows](#svanboxel-organization-workflows)
  - [Auditing Workflow Run Events](#auditing-workflow-run-events)
    - [Viewing Workflow events in the UI](#viewing-workflow-events-in-the-ui)
    - [Exporting the Audit Log](#exporting-the-audit-log)
    - [Using Audit Log API](#using-audit-log-api)
  - [Using a workflow template from your organization](#using-a-workflow-template-from-your-organization)

## Protecting workflow runs and secrets using Environments

This is documented [here]()

## Setting Enviroment protection rules and required approvers

This is documented [here](https://docs.github.com/en/actions/reference/environments)

In this reference, we have a branch `add-colors` and there is a workflow that has 2 jobs:
1. `build`
2. `deploy-staging`

`deploy-staging` requires `approval` before deploying to `stage`

![image](https://user-images.githubusercontent.com/57544838/112863729-24f4b880-9085-11eb-9c4a-8cce3f073985.png)

The required reviewer is notified when their approval is needed:

![image](https://user-images.githubusercontent.com/57544838/112863824-405fc380-9085-11eb-811b-5eb1ca07f14a.png)

Once they approve, the workflow runs :tada: and the approval is audited:

![image](https://user-images.githubusercontent.com/57544838/112864654-04792e00-9086-11eb-97a0-f350356235f4.png)


## Preventing unauthorized workflow runs

This is acheived using a custom GitHub App. The key things to keep in mind are:
1. There is webhook that gets triggered: [Workflow run webhook](https://docs.github.com/en/developers/webhooks-and-events/webhook-events-and-payloads#workflow_run)
2. The Webhook handler/Github App can listen to workflow run and cancel authorized runs [API to cancel a workflow run](https://octokit.github.io/rest.js/v18#actions-cancel-workflow-run)
3. The Webhook handler/Github App can listen to workflow run and disable an authorized workflow [API to disable a workflow](https://docs.github.com/en/rest/reference/actions#disable-a-workflow)

Example Probot App to cancel a workflow run and disable a workflow:

```Javascript
/**
 * This is the main entrypoint to your Probot app
 * @param {import('probot').Application} app
 */

module.exports = app => {
  // Your code here
  app.log('Yay, the app was loaded!')

  app.on(['workflow_run.requested'], async context => {
    //app.log(`Workflow Run was requested! for  ${JSON.stringify(context.payload)}`)
    let result=Math.random();
    if (result<9) {
      app.log(` deleting since result is ${result}`)
      cancelWorkflow(context);
      disableWorkflow(context)
    } else {
      app.log(`Not deleting since result is ${result}`)
    }
    return true;
  })

  async function cancelWorkflow(context) {
    try {
      await context.octokit.rest.actions.cancelWorkflowRun({
        owner: context.payload.repository.owner.login,
        repo: context.payload.repository.name,
        run_id: context.payload.workflow_run.id,
      });     
    } catch (error) {
      app.log(error)
    }
  }

  async function disableWorkflow(context) {
    try {
      await context.octokit.rest.actions.disableWorkflow({
        owner: context.payload.repository.owner.login,
        repo: context.payload.repository.name,
        workflow_id: context.payload.workflow_run.workflow_id,
      });     
    } catch (error) {
      app.log(error)
    }
  }

  // For more information on building apps:
  // https://probot.github.io/docs/

  // To get your app running against GitHub, see:
  // https://probot.github.io/docs/development/
}
```

## Organization or Enterprise level workflows

This is a feature that is in the roadmap but is targeted for a not-yet-defined future date. This would enable you to define a workflow that is run against multiple repositories in an Org or enterprise. This feature is designed to give you the ability to define workflows that must be run in your organization or enterprise. For example, you could define a workflow to scan for secrets, licenses, and more. You'll be able to define a workflow at the organization or at the enterprise level. You can choose to run the workflow in all repositories or a subset of repositories.

## Svanboxel Organization Workflows

This GitHub app allows you to run [GitHub Actions](https://github.com/features/actions) workflows across multiple repositories, which is [not yet natively supported](https://github.com/github/roadmap/issues/52). This app helps you - for example - to create a single workflow definition that is used for linting, compliance checks, and more.


https://github.com/SvanBoxel/organization-workflows


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

## Using a workflow template from your organization

This is documented [here](https://docs.github.com/en/actions/learn-github-actions/sharing-workflows-with-your-organization#using-a-workflow-template-from-your-organization)

:warning: We don't recommend this because this requires public visibility for your `.github` repository.
