# How to Integrate SOOS SCA with your Travis CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/azure.png" alt="azure" width="128" height="128">
</div>
In this article we will make the necessary modifications to a simple Azure project to scan a GitHub or Bitbucket repository with the SOOS SCA PRODUCT.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a Azure Project.
- Have downloaded the latest release of the [**SOOS Core SCA**](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/soos1643899774087.soos_msft?tab=Overview) from the Azure Marketplace

### Steps

### **Setup the SOOS SCA in your `pipeline.yml`**
After the task is successfully installed, download the Script Integration content from the [Azure DevOps Integrations page in the SOOS app](https://app.soos.io/integrate/sca?id=azure-devops) and paste to your existing pipeline yaml file or search for SOOS under Tasks.

### **Setup Variables**
Configure the SOOS variable, either directly in the yaml section or in the Task variables section.   Use the API Key and Client ID values you collected from the SOOS App.

Make sure to also set the Project Name (which groups scans together) and the Build and Branch parameters if available.

Providing the branch/build parameters allows us to tie together scans and issues, and provide more meaningful insights and actionability to you.

### **Additional Azure Pipeline Setup Notes**
Enable the following parameters in your pipeline as desired:

* continueOnError - prevents a failed build during maintenance window (SOOS Task option)
* waitForScan - waits for the analysis scan to complete (SOOS script parameter)

These options will allow SOOS to return scan status information to Azure.  The task will either Succeed or Fail with one of the following messages:

* Scan completed with # vulnerabilities and # violations.
Scan failed.
* Will appear if there was an error on the SOOS side while performing the scan.
* Scan failed with # vulnerabilities and # violations.
    * Will show if the Scan/Build configurations are turned on, and the corresponding settings in Azure are set to fail the build in the presence of failures due to vulnerabilities/violations. **Note, Scan/Build settings can be set at both the global & project level; make sure to check both!. (You can configure these options in [here](https://app.soos.io/settings/global)**
    * The specifics of the vulnerabilities/violations will need to be viewed in SOOS, this information will not be returned to Azure.  A link to access the scan results will be provided in the message displayed in Azure.

### Run It
To run the SOOS Azure DevOps task against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.
