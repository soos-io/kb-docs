# How to integrate SOOS SBOM with Azure DevOps
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/azure.png" alt="azure" width="128" height="128">
</div>

In this article we will make the necessary modifications to a simple Azure project to scan a GitHub or Bitbucket repository with the SOOS SBOM PRODUCT.

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register).
- You need to have a Azure DevOps Project with a Pipeline.
- You need to have a repository, connected to that pipeline, with a [supported](https://kb.soos.io/help/sbom-upload) SBOM file.
- You need to add the [**SOOS Security Analysis**](https://marketplace.visualstudio.com/items?itemName=SOOS.SOOS-Security-Analysis) task to your organization in Azure DevOps

## Steps
- After the task is successfully installed, copy the contents of the [`azuredevops_sbom.yml`](https://gist.github.com/soostech/983b3756ea3f6e3631d89c97604bd969) file, as see on the [Azure DevOps Integrations page in the SOOS app](https://app.soos.io/integrate/SBOM?id=azure-devops), to your `azure-pipelines.yml` file.
- Using the `Client Id` and `API Key` values found on the [Azure DevOps Integrations page in the SOOS app](https://app.soos.io/integrate/SBOM?id=azure-devops), set the corresponding task parameters or use environment variables.

## Optional Setup
Enable the following parameters as desired:
- `continueOnError` - prevents a failed build during maintenance window (Azure Task Parameter)

## Task Results
- In your pipeline, the task will either Succeed, Succeed with Issues, or Fail.
- Unless your task definition has set `continueOnError` to true, the task will fail during a maintenance window
- If you have configured your Scan/Build settings, in the [SOOS app](https://app.soos.io/settings/global), to fail the build if either a vulnerability or policy violation is identified, the task will wait for the scan to complete and set the result accordingly.
  - Note: Scan/Build settings can be set at either the global level or overridden at the project level. You can configure these settings [here](https://app.soos.io/settings/global).
- Whenever you run a scan the task will output a link to view the scan results to the console.
  - Note: If a scan has not completed before you view the results, then the information in the app will not be up-to-date.

## Run It
To run the SOOS Azure DevOps task against your repository’s code, just execute a build or commit a change.