# How to Integrate SOOS SCA with your GitHub Repo
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/github-action.png" alt="Github Action" width="128" height="128">
</div>
In this article we will make the necessary modifications to a GitHub Workflow using the SOOS CI Analysis GitHub Action to scan a GitHub repository with SOOS.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a GitHub Repo.

## Steps

### **Repo Setup**
* Create a `.github/workflows` directory in your repository on GitHub if this directory does not already exist.
* In the `.github/workflows` directory, create a file named main.yml.
For more information, see "Creating new files" in GitHub Docs.
* Paste the script given in the [GitHub Action SCA Integration page on SOOS](https://app.soos.io/integrate/sca?id=github-actions).


### **Setup Environment Variables in GitHub repo**
Navigate to your **Project’s Settings > Secrets** menu and in the “Repository secrets” section, create the SOOS_API_KEY and SOOS_CLIENT_ID secrets. These will serve as environment variables to be used by the SOOS CLI.(The values for the Client ID & API Key can be found on the [GitHub Action SCA Integration page on SOOS](https://app.soos.io/integrate/sca?id=github-actions).)

### **Build Config**
Modify the `.github/workflows/main.yml` file, replacing the provided project_name variable value with one that is relevant to your project.

## Run It
To run the SOOS CLI against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.

 
