# How to Integrate SOOS CSA with your Azure DevOps CI Pipeline
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/azure.png" alt="Azure" width="128" height="128">
</div>
This document will take you step-by-step through the tasks required to set up an Azure project and scan it with SOOS Container Security Analysis (CSA).

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have an Azure Project configured.

## Steps

### **Installing the Task**

The SOOS Task can be found under the shared extension listing in your organization settings. Click the option to install the Task. The installation has been completed successfully when it is visible under the Installed listing.

<img src="../assets/img/azure-install.png">

### **Setup Variables**

Once installed, search for SOOS under tasks and proceed with the configuration.

<img src="../assets/img/azure-task.png">

- Select Container Security Analysis (CSA) for the Scan Type parameter.
- Configure the SOOS variables, either directly in the YAML section or in the Task Variables section. Use the API Key and Client ID values you found on the [SOOS App integration page.](https://app.soos.io/integrate/containers)

Be sure to set the Display Name, Project Name (which groups scans together), and Target Image parameters.

<img src="../assets/img/csa-azure-variables.png">

### **Setting Up Inside Your pipeline.yml**

Here is an example YAML script for setting up the Task in your pipeline:

```yaml
- task: SOOS-Security-Analysis@0
  inputs:
    apiKey: <SOOS_API_KEY>
    clientId: <SOOS_CLIENT_ID>
    scanType: 'CSA'
    targetToScan: <image:tag> # The target to scan. Should be a docker image name or a path to a directory containing a Dockerfile
    projectName: <PROJECT_NAME> # The name of the project. Defaults to 'Build.Repository.Name'.
```

### **Run It**

To run the SOOS Azure DevOps Task against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.