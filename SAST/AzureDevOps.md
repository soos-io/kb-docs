# How to Integrate SOOS SAST with Azure DevOps

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/azure.png" alt="Azure DevOps" width="128" height="128">
</div>

In this article, we will guide you through modifying a simple Azure project to scan a GitHub or Bitbucket repository with the SOOS SAST Analysis product.

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register) with SAST scanning enabled.
- You need to have an Azure DevOps Project with a Pipeline.
- You need to have a repository, connected to that pipeline, with a [supported](https://kb.soos.io/help/sast-connector) SAST file.
- Add the [**SOOS Security Analysis**](https://marketplace.visualstudio.com/items?itemName=SOOS.SOOS-Security-Analysis) task to your organization in Azure DevOps.
- If you are running a self-hosted agent make sure to have Node 18.18.2 or higher - https://nodejs.org/en/download on the agent. Also you can look to use [Use Node Task](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/use-node-v1?view=azure-pipelines) 
- You need to have previously generated Sarif 2.1 json in file(s) that end with the `.sarif.json` extension.

## Steps
- After successfully installing the task, copy the contents of the [`azuredevops_sast.yml`](https://gist.github.com/soostech/70e79cab452a24a153585651fa1e4f8c) file from the [Azure DevOps Integrations page in the SOOS app](https://app.soos.io/integrate/SAST?id=azure-devops) into your `azure-pipelines.yml` file.
- Set the corresponding task parameters or use environment variables with the `Client ID` and `API Key` values found on the same [Azure DevOps Integrations page](https://app.soos.io/integrate/SAST?id=azure-devops).

## Optional Setup
Enable the following parameter as desired:
- `continueOnError`: Set to prevent a failed build during the maintenance window (Azure Task Parameter).

## Task Results
- In your pipeline, the task will Succeed, Succeed with Issues, or Fail.
- The task will fail during a maintenance window unless `continueOnError` is set to true.
- If your Scan/Build settings in the [SOOS app](https://app.soos.io/settings/global) are configured to fail the build upon identifying a vulnerability or policy violation, the task will wait for the scan to complete and set the result accordingly.
  - Note: Scan/Build settings can be configured globally or overridden at the project level. Configure these settings [here](https://app.soos.io/settings/global).
- Each scan outputs a console link to view the scan results.
  - Note: If a scan is incomplete when viewing the results, the information in the app will not be up-to-date.

## Run It
To run the SOOS Azure DevOps task against your repository’s code, simply execute a build or commit a change.