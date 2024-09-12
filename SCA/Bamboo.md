# How to Integrate SOOS SCA with your Bamboo CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/bamboo.png" alt="bamboo" width="128" height="128">
</div>

Set up Bamboo to scan your manifests with SOOS SCA Core.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a Bamboo project.

## Steps

### **Repository Setup**
* Create a new folder in your repository: <repo_root>/bamboo-specs
* Create a bamboo.yml file with the contents given on [Bamboo Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=bamboo) under the bamboo-specs option.
* Place the bamboo.yml under  <repo_root>/bamboo-specs/ folder that you created.
* Commit the new file and the new folder.


### **Build Setup in bamboo**
* Navigate to Administration -> Linked Repositories
* Select the repository you have set up your bamboo specs. Enable the option Scan for Bamboo Specs.
* In the specs status tab you can scan for the bamboo.yml file to be added to the steps of the build.

### **Setup Environment Variables**
* Create the SOOS_API_KEY and SOOS_CLIENT_ID environment variables, either under the Global Variables or the Plan Variables sections.
* Copy & paste the API key and Client ID values from the [Bamboo Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=bamboo).  These will serve as environment variables to be used by the SOOS CLI.

## Run It
To run the SOOS CLI against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.