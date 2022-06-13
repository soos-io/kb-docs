# How to Integrate SOOS SCA with your AWS Codebuild CI

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/codebuild.png" alt="codebuild" width="128" height="128">
</div>
In this article we will make the necessary modifications to a simple AWS CodeBuild project to scan a GitHub repository with SOOS.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a AWS Codebuild Project.
- Have downloaded the latest release of the `soos.py`,`requirements.txt` and `VERSION.txt` from [here](https://github.com/soos-io/soos-ci-analysis-python/releases/)

## Steps

### **Repo Setup**
* Create a new folder in your repository: `<repo_root>/soos/workspace/`
* Place the `soos.py`,`requirements.txt` and `VERSION.txt` files in the `<repo_root>/soos/` folder.
* Commit these 2 new files and the new folder path.

### **Configure Codebuild environment variables**

* Within Codebuild navigate to your Project and select **"Environment"** from the **"Edit"** menu.
Open **"Additional Configuration"** to reveal the **"Environment Variables"** section. In the **"Environment Variables"** section, create the SOOS_API_KEY and SOOS_CLIENT_ID environment variables. These will serve as environment variables to be used by the Package Aware CLI. Use the API Key and Client ID from the [AWS Codebuild integration page on the SOOS AP](https://app.soos.io/integrate/sca?id=aws-codebuild).

### **Set Build Commands**
Return to the **"Edit"** menu and select **Buildspec** to insert build commands.  Add the script provided in the [AWS Integration page in the SOOS App](https://app.soos.io/integrate/sca?id=aws-codebuild) and select **"Update Buildspec"**.

## Run It
To run the SOOS CLI against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.
