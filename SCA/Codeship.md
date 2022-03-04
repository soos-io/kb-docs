# How to Integrate SOOS SCA with your Codeship CI

<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/codeship.png" alt="Codeship" width="128" height="128">

This document will take you step-by-step through the tasks required to set up a Codeship repo, for scan it with the SOOS SCA Product.
## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a Codeship repo.
- Have downloaded the latest release of the `soos.py` and `requirements.txt` from [here](https://github.com/soos-io/soos-ci-analysis-python/releases/)

## Steps

### **Repo Setup**
* Create a new folder in your repository: `<repo_root>/soos/workspace/`
* Place the requirements.txt and soos.py files in the `<repo_root>/soos/workspace` folder.
* Commit these 2 new files and the new folder path.

### **Setup environment variables in Codeship**

* Within CodeShip, navigate to **Project Settings**.
* Create the SOOS_API_KEY and SOOS_CLIENT_ID environment variables in the **Environment variables** text field under **Global Properties**.
* Copy & paste the API key and Client ID values from the [CodeShip Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=codeship). These will serve as environment variables to be used by the SOOS CLI.

### **Add Build Configuration**
* Select *Tests* in the menu, then select “I want to create my own custom commands” within the Select **your technology to pre-populate basic commands**.
* Enter “pyenv local 3.6” in the **Setup Commands** text box.
* Add a new pipeline in the **Configure Test Pipelines** section by selecting the **Add Pipeline** button.
* Enter an appropriate name for the pipeline, and press Save.
* In the **Pipeline** text box, add the script from the CodeShip Integration Page of the SOOS App.
* Select Save **Changes**.

## Run It
To run the SOOS CLI against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.