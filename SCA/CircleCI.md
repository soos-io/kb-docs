# How to Integrate SOOS SCA with your CircleCI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/circleci.png" alt="circleci" width="128" height="128">
</div>
## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a CircleCI project.
- Have downloaded the latest version of the `orb.yml` from [here](https://github.com/soos-io/soos-ci-analysis-circleci-orb)

## Steps

## Repo Setup
* Create a directory called `.circleci` in the root directory of your local GitHub or Bitbucket code repository.
* Create a `config.yml` file inside the `.circleci` directory directory using the contents of the `orb.yml` file downloaded from GitHub.
    * If using CircleCI server v2.x, use version 2.0 configuration.

### **Setup Environment Variables**
* Within the CircleCI application, navigate to **Project Settings > Environment Variables**.
* Select Add Variable button and create the SOOS_CLIENT_ID and SOOS_API_KEY variables.
* Copy & paste the API key and Client ID values from the [CircleCI Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=circleci). These will serve as environment variables to be used by the SOOS CLI.

## Run It
* Navigate to the Projects page in the CircleCI app, click the Add Projects button.
* Select the Set Up Project button next to your project. 
    * If you don’t see your project, make sure you have selected the associated Org. See the Org Switching section further down the page for tips.
* Click the Start Building button to trigger your first build.