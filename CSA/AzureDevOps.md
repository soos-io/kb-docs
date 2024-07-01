# How to Integrate SOOS CSA with your Azure DevOps CI Pipeline
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/azure.png" alt="Azure" width="128" height="128">
</div>
This document will take you step-by-step through the tasks required to set up an Azure project and scan it with SOOS Container Security Analysis (CSA).

## Prerequisites

- You need to have a [SOOS account](https://app.soos.io/register) with Container scanning enabled.
- You need to have a Azure DevOps YML pipeline.
- The pipeline job must run the task on a Linux build agent.

## Steps

### **Use a Linux Build Agent**

Your pipeline stage must use a Linux build agent:
```
  pool:
    vmImage: 'ubuntu-latest'
```

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

## Scanning a private image from Azure Container Registry (ACR)

The following steps outline how to perform a CSA scan targeting private images in ACR.

1. Setup Docker Environment: Ensure Docker is installed on your build agent. The pipeline script uses the DockerInstaller task to install a specific version of Docker.

2. Pull the Private Image: Use the Docker task to authenticate and pull your private image from Azure Container Registry. You will need to define a [service connection](https://learn.microsoft.com/en-us/azure/devops/pipelines/ecosystems/containers/acr-template?view=azure-devops) in Azure DevOps that has access to your ACR instance.

3. Save the Image Locally: After pulling the image, use the Docker 'save' command to save the image as a tarball file.

4. Run the SOOS Security Analysis: Fill all the required fields as normal and on the `targetToScan` set the path to the saved image tarball.

Example workflow: 

``` yaml
pool:
  name: Azure Pipelines
steps:
- task: DockerInstaller@0
  displayName: 'Install Docker 17.09.0-ce'

- task: Docker@2
  displayName: pull
  inputs:
    containerRegistry: <REGISTRY_SERVICE_CONNECTION_NAME>
    command: pull
    arguments: '<PRIVATE_IMAGE_FULL_URL>'

- task: Docker@2
  displayName: save
  inputs:
    command: save
    arguments: '-o <FILE_NAME>.tar <PRIVATE_IMAGE_FULL_URL>'

- task: SOOS.SOOS-Security-Analysis.scan-task.SOOS-Security-Analysis@0
  displayName: 'SOOS Security Analysis'
  inputs:
    apiKey: <YOUR_API_KEY>
    clientId: <YOUR_CLIENT_ID>
    scanType: CSA
    project: <YOUR_PROJECT_NAME>
    targetToScan: 'docker-archive:results/<FILE_NAME>.tar'
    verbose: false
```
