# How to Integrate SOOS SCA with your Jenkins CI using the Python Script
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/jenkins.png" alt="Jenkins" width="128" height="128">
</div>
This document will take you step-by-step through the tasks required to set up a Jenkins project, for scan it with the SOOS SCA Product.

## Prerequisites
- You need to have a [SOOS account.](https://app.soos.io/register)
- Have downloaded the latest release of the `soos.py`,`requirements.txt` and `VERSION.txt` from [here](https://github.com/soos-io/soos-ci-analysis-python/releases/)

## Steps

### **Repo Setup**
* Create a new folder in your repository: `<repo_root>/soos/workspace/`
* Place the `requirements.txt`,`soos.py` and `VERSION.txt` files in the `<repo_root>/soos/` folder.
* Commit these 2 new files and the new folder path.

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
    - Add the SOOS script from the [Jenkins Integration - Python Script Integration page of the SOOS app](https://app.soos.io/integrate/sca?id=jenkins) into the Execute Shell > Command  text box.
    - Set the Exit code to set build unstable value to "1".
    - Make sure to set the Project Name (which groups scans together) and the Build & Branch parameters.  Providing the build/branch parameters allows us to tie together scans and issues, and provide more meaningful insights and actionability to you.

## Run It
To run the SOOS CLI against your repository's code, execute a build or commit a change.  The build will use the environment variables that you created for the API Key and Client ID.

## Error Troubleshooting
If errors such as the following are received:

* ['pip3' is not recognized as an internal or external command]

* ['python' is not recognized as an internal or external command]

Open Jenkins > Manage Jenkins > Configure System > Environment.
Add a "path" variable with values pointing to the Python install and Python scripts directory. 
E.g.: [C:\Users\USER\AppData\Local\Programs\Python\Python39\;C:\Users\USER\AppData\Local\Programs\Python\
