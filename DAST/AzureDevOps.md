# How to Integrate SOOS DAST with your Azure DevOps CI Pipeline
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/azure.png" alt="Azure" width="128" height="128">
</div>
This document will take you step-by-step through the tasks required to set up an Azure pipeline and scan it with SOOS Dynamic Application Security Testing (DAST).

## Prerequisites

- You need to have a [SOOS account](https://app.soos.io/register) with DAST scanning enabled.
- You need to have an Azure DevOps Project with a Pipeline.
- Add the [**SOOS Security Analysis**](https://marketplace.visualstudio.com/items?itemName=SOOS.SOOS-Security-Analysis) task to your organization in Azure DevOps.
- The pipeline job must run the task on a Linux build agent.

## Steps

### **Use a Linux Build Agent**

Your pipeline stage must use a Linux build agent:
```
  pool:
    vmImage: 'ubuntu-latest'
```

### **Installing the task**

The SOOS task will be found under the shared extension listing in your organization settings. Click the option to install the task. The installation has been completed successfully when it is visible under the Installed listing.

<img src="../assets/img/azure-install.png">

### **Setup variables**

Once installed search for the SOOS under tasks and proceed with the configuration.

<img src="../assets/img/azure-task.png">

- Select Dynamic Application Security Testing (DAST) for the Scan Type parameter
- - Configure the SOOS variables, either directly in the yaml section or in the Task variables section. Use the API Key and Client ID values you collected from the SOOS App.

Make sure to also set the Display Name, Project Name (which groups scans together), Target Uri and Scan mode parameters.

<img src="../assets/img/dast-azure-variables.png">

### **Setting up inside your pipeline.yml**

Once we have defined these variables globally you can set up the task to be used inside your pipeline.yml following this example script.

```
- task: SOOS-Security-Analysis@0
  inputs:
    apiKey: {SOOS_API_KEY}
    clientId: {SOOS_CLIENT_ID}
    scanType: 'DAST'
    project: {YOUR_PROJECT_NAME_HERE}
    targetUri: 'https://example.com'
    scanMode: 'baseline'
```

### **Run It**

To run the SOOS Azure DevOps task against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.

