# How to Integrate SOOS SCA Plugin with your TeamCity CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/teamcity.png" alt="teamcity" width="128" height="128">
</div>
In this article we will make the necessary modifications to a simple TeamCity project to scan a GitHub repository with the SOOS SCA Product using our Plugin for TeamCity.

## Prerequisites
- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a TeamCity project.
- Have installed the latest version of the [SOOS SCA Plugin](https://plugins.jetbrains.com/plugin/18222-soos-sca)
- Have JDK 11 configured on your Teamcity agent.
- Have Node 18.18.2 or Higher - [Download Node.js](https://nodejs.org/en/download) configured on your Teamcity agent.

## Steps

### Configure Build Steps
* Navigate to your project’s Build Configuration and select Create build configuration.
* Within your `BUILD CONFIGURATION`, select `BUILD STEPS` within the left-hand menu, then add a new build step.
* When prompted, select `SOOS SCA` from the menu.
* Add a step name that is relevant (such as SOOS SCA) and enter the minimum required settings for the correct running of the plugin. Providing the branch/build parameters allows SOOS to tie together scans and issues, and provide more meaningful insights and actionability to you.  
* Finally just save your new step.

### Setup Environment Variables
* In the left-hand menu, select `Parameters` to review your environment variables.
* The SOOS SCA Plugin needs `System Properties (system.)` in TeamCity, which are passed as parameters by the plugin. Create `SOOS_CLIENT_ID` and `SOOS_API_KEY` as system properties and fill in the values with those collected from the [SOOS App Integrations Page](https://app.soos.io/integrate/sca?id=teamcity).
**Notice that if you have these environments values set up in your agent machine these will be overriden.**


## Run It
To run the SOOS SCA Plugin against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.

## Technical details
For more technical details you can visit our [public GitHub repository](https://github.com/soos-io/soos-sca-teamcity-plugin)
