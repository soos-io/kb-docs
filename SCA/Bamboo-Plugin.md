# How to Integrate SOOS SCA Plugin with your Bamboo CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/bamboo.png" alt="bamboo" width="128" height="128">
</div>
This document will take you step-by-step through the tasks required to set up a Bamboo project, for scan it with the SOOS SCA Product using our Plugin for Bamboo.

## Prerequisites
- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a Bamboo project.
- Installed the latest version of the [SOOS SCA Plugin](https://marketplace.atlassian.com/apps/1227220/soos-sca)
- JDK 11 configured on your Bamboo agent.
- Node 20 LTS or higher - [Download Node.js](https://nodejs.org/en/download) configured on your Bamboo agent.

## Steps

### Setup Environment Variables
* Within Bamboo click on the settings wheel and navigate into `Global Variables` 
* Once in `Global Variables` add the `SOOS_API_KEY` and `SOOS_CLIENT_ID` variables with the values collected from the [Bamboo Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=bamboo)

### Build Setup
* Navigate to your project’s plan configuration and press the "Add task" button. 
**Note**: Make sure this task has a predecessor task to checkout your repo’s source code.
* Type "SOOS" into the search box and choose the SOOS SCA task option.
* Fill in the SOOS SCA configuration fields, in here just make sure to set the Project Name (which groups scans together) and the Build and Branch parameters.

## Run It
To run the SOOS SCA Plugin against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.

## Technical details
For more technical details you can visit our [public GitHub repository](https://github.com/soos-io/soos-sca-bamboo-plugin)
