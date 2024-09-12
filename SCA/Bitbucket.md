# How to Integrate SOOS SCA with Bitbucket
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/bitbucket.png" alt="Bitbucket" width="128" height="128">
</div>

Set up BitBucket to scan your manifests with SOOS SCA Core.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a https://bitbucket.org account and repository setup or a private instance of Bitbucket server

## Steps

### **Repo Setup**
* Create a new `bitbucket-pipelines.yml` file in the root of the repository (`<repo_root>/bitbucket-pipelines.yml`).
* Add the SOOS script from the [Bitbucket Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=bitbucket) to your repository's `bitbucket-pipelines.yml` file or from the Starter Script below.
* Copy & paste the API key and Client ID values from the [Bitbucket Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=bitbucket). While editing the pipeline file, select "Add Variables" and add two Secured variables, "SOOS_CLIENT_ID" and "SOOS_API_KEY".

#### Starter Script ####
```
image: node:18-slim

pipelines:
  custom:
    soos-sca:
    - variables:
      - name: SOOS_ON_FAILURE
        default: "continue_on_failure"
        allowed-values:
        - "continue_on_failure"
        - "fail_the_build"
      - name: SOOS_DIRS_TO_EXCLUDE
        default: ""
      - name: SOOS_FILES_TO_EXCLUDE
        default: ""
      - name: SOOS_LOG_LEVEL
        default: "INFO"
        allowed-values:
        - "INFO"
        - "WARN"
        - "FAIL"
        - "DEBUG"
        - "ERROR"
    - parallel:
      - step:
          name: 'SOOS SCA'
          script:
            - npm install --prefix ./soos @soos-io/soos-sca
            - node ./soos/node_modules/@soos-io/soos-sca/bin/index.js 
              --onFailure=$SOOS_ON_FAILURE
              --clientId=$SOOS_CLIENT_ID
              --apiKey=$SOOS_API_KEY
              --directoriesToExclude=$SOOS_DIRS_TO_EXCLUDE
              --filesToExclude=$SOOS_FILES_TO_EXCLUDE
              --projectName=$BITBUCKET_REPO_SLUG
              --commitHash=$BITBUCKET_COMMIT
              --branchName=$BITBUCKET_BRANCH
              --branchURI="http://bitbucket.org/$BITBUCKET_REPO_FULL_NAME"
              --buildVersion=$BITBUCKET_BUILD_NUMBER
              --logLevel=$SOOS_LOG_LEVEL
              --integrationName="BitBucket"
```

## Run It
To run the SOOS CLI against your repository’s code, just execute a build or commit a change.
