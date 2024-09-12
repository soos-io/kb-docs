# How to Integrate SOOS SCA Plugin with your Jenkins CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/jenkins.png" alt="jenkins" width="128" height="128">
</div>
This document will take you step-by-step through the tasks required to set up a Jenkins project, for scan it with the SOOS SCA Product using our Plugin for Jenkins.

## Prerequisites
- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a Jenkins project.
- Install the latest version of the [SOOS SCA Plugin](https://plugins.jenkins.io/soos-sca/)
- JDK 11 configured on your Jenkins agent.
- Node 20 LTS or higher - [Download Node.js](https://nodejs.org/en/download) configured on your Jenkins agent.

## Steps

### Build Setup
* Add **New Item** in the Jenkins main menu.
* Enter a name for the item and select **Freestyle Project**.
* Setup the SCM for the repository where you have the manifests to perform the scan.
* In the **Build** section click the **Add a build step** button and select **SOOS SCA** and then:
    - Set up the SOOS Client ID and SOOS API Key variables with the values collected from the [Jenkins Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=jenkins)
    - Setup the Project Name (This will be the name that will be displayed on the SOOS Dashboard).

    
## Run It
To run the SOOS SCA Plugin against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.

## Technical details
For more technical details you can visit our [public GitHub repository](https://github.com/jenkinsci/soos-sca-plugin)
