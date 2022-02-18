---
title: Integrating SOOS DAST with CircleCI 
gistURL: https://gist.github.com/soostech/46c705c2c9f9914337f7365ea90b52b9f2ef555f
---

# How to Integrate SOOS DAST with your CircleCI CI

<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/circleci.png" alt="CircleCI" width="128" height="128">

In this article we will make the necessary modifications to a simple CircleCI project to scan a GitHub or Bitbucket repository with the SOOS CircleCI Orb.

## Prerequisites

- You need to have a SOOS account.
- You need to have a CircleCI project.

## Steps

### **Repo Setup**
- Create a directory called .circleci in the root directory of your local GitHub or Bitbucket code repository.
- Create a config.yml file inside the .circleci directory with the following lines (if you are using CircleCI server v2.x, use version: 2.0 configuration):

```
    version: 2.1
    orbs:
        soos: soos-io/sca@1.0.0

    workflows:
        main:
            jobs:

                # NOTE: YOUR OTHER JOBS GO HERE

                - soos/analysis_run_and_wait:
                    client_id: "<<SOOS Client Id>>"
                    api_key: "<<SOOS API Key>>"
                    project_name: "<<Project Name>>"

                # NOTE: YOUR OTHER JOBS GO HERE
```


- Commit and push the changes.
- Go to the Projects page in the CircleCI app, click the Add Projects button, then click the Set Up Project button next to your project. If you don’t see your project, make sure you have selected the associated Org. See the Org Switching section below for tips.
- Click the Start Building button to trigger your first build. (Previous this step, you must to setup the environment variables)

### **Build Setup**
- In the CircleCI application, go to your project’s settings by clicking the gear icon on the Pipelines page, or the three dots on other pages in the application.
<img src="../assets/img/circleci-settings.png">
- Click on Environment Variables.
- Add the SOOS_CLIENT_ID and SOOS_API_KEY variables by clicking the Add Variable button and enter the name and the value provided by the SOOS App. (See screenshot from Integration Steps section) 




<script src="https://gist.github.com/soostech/652c66b03cd8c7a856c74467bffc4086.js"></script>

