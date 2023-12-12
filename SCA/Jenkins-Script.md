# How to Integrate SOOS SCA with your Jenkins CI using the Script
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/jenkins.png" alt="Jenkins" width="128" height="128">
</div>
This document will take you step-by-step through the tasks required to set up a Jenkins project, for scan it with the SOOS SCA Product.

## Prerequisites
- You need to have a [SOOS account.](https://app.soos.io/register)

## Steps

### **Setup Environment Variables**
* Within Jenkins navigate to **Manage Jenkins > System Configuration > Configure System.**
* Under **Global Properties**, add environment variables for your SOOS_API_KEY and SOOS_CLIENT_ID
* Copy & paste the values from the [Jenkins Integration page of the SOOS app](https://app.soos.io/integrate/sca?id=jenkins).  These will serve as environment variables to be used by the SOOS CLI.
* Save your configuration.

### **Build Setup**
* Add **New Item** in the Jenkins main menu.
* Enter a name for the item and select **Freestyle Project**.
* On the next screen:
    - Type in a description for your item.
    - Select the GitHub Project checkbox and enter your Project URL.
    - Select the Source Code Management tab and select the Git option.
    - Enter your Repository URL and all other required information within the Source Code Management section in order to integrate with your repository.
    - Add the SOOS script from the [Jenkins Integration - Script Integration page of the SOOS app](https://app.soos.io/integrate/sca?id=jenkins) into the Execute Shell > Command  text box.
    - Set the Exit code to set build unstable value to "1".
    - Make sure to set the Project Name (which groups scans together) and the Build & Branch parameters.  Providing the build/branch parameters allows us to tie together scans and issues, and provide more meaningful insights and actionability to you.

## Run It
To run the SOOS CLI against your repository's code, execute a build or commit a change.  The build will use the environment variables that you created for the API Key and Client ID.

## Error Troubleshooting
If errors such as the following are received:

* ['node' is not recognized as an internal or external command]

* ['npm' is not recognized as an internal or external command]

Open Jenkins > Manage Jenkins > Configure System > Environment.
Add a "path" variable with values pointing to the node install. 
E.g.: [C:\Program Files\nodejs\node.exe]

## Reference
* To see the full list of available parameters go to [Script repository parameters description](https://github.com/soos-io/soos-sca?tab=readme-ov-file#parameters)