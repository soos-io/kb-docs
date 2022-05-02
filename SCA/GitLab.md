# How to Integrate SOOS SCA with your GitLab CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/gitlab.png" alt="GitLab" width="128" height="128">
</div>
This document will take you step-by-step through the tasks required to set up a GitLab repo, for scan it with the SOOS SCA Product.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a GitLab repo.
- Have downloaded the latest release of the `soos.py` and `requirements.txt` from [here](https://github.com/soos-io/soos-ci-analysis-python/releases/)

## Steps

### **Repo Setup**

* Create a new folder in your GitHub repository: `<repo_root>/soos/workspace/`
* Place the requirements.txt and soos.py files in the `<repo_root>/soos/workspace` folder.
* Commit these 2 new files and the new folder path to GitHub.

### **Configure GitLab**
* Navigate to Settings > CI/CD under Projects.

* Select the Expand button within the Variables section.

* Create the SOOS_API_KEY and SOOS_CLIENT_ID environment variables.

* Copy & paste the API key and Client ID values from the [GitLab Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=gitlab). These will serve as environment variables to be used by the SOOS CLI.

* Add the SOOS script from the [GitLab Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=gitlab) to your .gitlab-ci.yml file.

##  Run It
To run the SOOS CLI against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.

