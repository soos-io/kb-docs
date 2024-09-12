# How to Integrate SOOS Container Analysis with an Azure DevOps CI Pipeline
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/azure.png" alt="Azure" width="128" height="128">
</div>
Set up an Azure DevOps pipeline and scan it with SOOS Container Security Analysis (CSA).

## Prerequisites

- You need to have a [SOOS account](https://app.soos.io/register) with Container scanning enabled.
- You need to have an Azure DevOps Project with a Pipeline.
- The pipeline job must run the task on a Linux build agent.
- Add the [**SOOS Security Analysis**](https://marketplace.visualstudio.com/items?itemName=SOOS.SOOS-Security-Analysis) task to your organization in Azure DevOps.

## Steps

### **Get the Example**

* Navigate to the [Azure DevOps DAST integration page on the SOOS App](https://app.soos.io/integrate/containers?id=azure-devops), copy the example, and modify it.

### **Run It**

* Execute the pipeline

## Scan a private image from Azure Container Registry (ACR)

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
  displayName: 'Install Docker'

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

---

## Reference
* To see the full list of available parameters go to the [CSA repository parameters description](https://github.com/soos-io/soos-csa#parameters).