# How to Integrate SOOS SCA with your Jenkins CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/jenkins.png" alt="Jenkins" width="128" height="128">
</div>
This document will take you step-by-step through the tasks required to set up a Jenkins project, for scan it with the SOOS SCA Product.
## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a Jenkins project.

## Steps

## Repo Setup
* Download the Jenkinsfile Integration content from the [Jenkinsfile Integration page of the SOOS app](https://app.soos.io/integrate/sca?id=jenkins).
* Create a `Jenkinsfile` file in your root repo and paste the contents downloaded in the step above.

### **Setup Environment Variables**
* Within Jenkins navigate to **Manage Jenkins > System Configuration > Configure System.**
* Under **Global Properties**, add environment variables for your SOOS_API_KEY and SOOS_CLIENT_ID
* Copy & paste the values from the [Jenkinsfile Integration page of the SOOS app](https://app.soos.io/integrate/sca?id=jenkins).  These will serve as environment variables to be used by the SOOS CLI.
* Save your configuration.

### **Install Docker and add the Plugins**
* Visit https://www.docker.com/ and download Docker.
* In Jenkins navigate to **Manage Jenkins > Manage Plugins**.
* Select the **Available** tab, and search for Docker plugins.
* Install the **Docker** and **Docker Pipeline** plugins in order to get the Docker agent available to run the Jenkinsfile.

### **Build Setup**
* Add **New Item** in the Jenkins main menu
* Enter a name for the item and select **Pipeline**.
* On the next screen:
    * Type in a description for your item.
    * Select the Pipeline Tab.
    * Choose 'Pipeline script from SCM' in the Definition field.
    * In the SCM field select 'Git'.
    * Enter the link to your GitHub repo in the Repository URL field and enter the corresponding credentials when prompted.
    * Write the Jenkinsfile path in the Script Path field and select Apply and Save.

## Run It
To run the SOOS CLI against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.

 
## Error Troubleshooting
If errors such as the following are received ensure that the Docker plugins have been installed.  Refer to 'Install Docker and add the Plugins' section above for guidance.
* "Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?"
* "Invalid agent type "docker" specified. Must be one of [any, label, none] @ line 3, column 9. docker"