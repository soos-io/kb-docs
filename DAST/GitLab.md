# How to Integrate SOOS DAST with your GitLab CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/gitlab.png" alt="GitLab" width="128" height="128">
</div>
This document will take you step-by-step through the tasks required to set up a GitLab repo, for scan it with the SOOS DAST Product.
## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a GitLab repo.

## Steps

<summary class='section-title'>Create or update your <code>.gitlab-ci.yml</code> file</summary>

Once the `SOOS_CLIENT_ID` and `SOOS_API_KEY` have been configured, the next step is create or update the `.gitlab-ci.yml` file.

**Note:** You need to create a `.gitlab-ci.yml` file in your repository root directory if you don't have it.

1. Copy the content below in your `.gitlab-ci.yml` file to run the `SOOS DAST Analysis` according to what type of DAST Analysis you want to do.

### **Baseline Scan**
* It performs a passive scan of HTTP messages (requests and responses) sent to the web application being tested. 
    * Passive scanning does not change the requests nor the responses in any way and is therefore safe to use.
* The parameters clientId, apiKey, and projectName are required
* We recommend defining apiKey and clientId as Environment Variables (see Build Setup section) to protect your credentials

```
image:
  name: "soosio/dast"
  entrypoint: [""]

variables:
  SOOS_PROJECT_NAME: "SOOS-DAST-GitLab"
  SOOS_SCAN_MODE: 'baseline'
  SOOS_API_BASE_URL: "https://api.soos.io/api/"
  SOOS_TARGET_URL: "https://example.com"

soos-dast-analysis:
  before_script:
    - export SOOS_PARAMS="--clientId=${SOOS_CLIENT_ID} --apiKey=${SOOS_API_KEY} --projectName=\"${SOOS_PROJECT_NAME}\" --scanMode=${SOOS_SCAN_MODE} --apiURL=${SOOS_API_BASE_URL} --integrationName=GitLab"
  script:
    - cd /zap/
    - python3 main.py ${SOOS_PARAMS} ${SOOS_TARGET_URL}
```

### **Full Scan**
* This scan mode is used to perform a full analysis of a web-app. 
* It performs an active scan, attempting to find potential vulnerabilities by using known attacks against the selected targets. 
    * It should be noted that active scanning can only find certain types of vulnerabilities. Logical vulnerabilities, such as broken access control, will not be found by any active or automated vulnerability scanning.
* The parameters clientId, apiKey, and projectName are required
* We recommend defining apiKey and clientId as Environment Variables (see Build Setup section) to protect your credentials

```
image:
  name: "soosio/dast"
  entrypoint: [""]

variables:
  SOOS_PROJECT_NAME: "SOOS-DAST-GitLab"
  SOOS_SCAN_MODE: 'fullscan'
  SOOS_API_BASE_URL: "https://api.soos.io/api/"
  SOOS_TARGET_URL: "https://example.com"

soos-dast-analysis:
  before_script:
    - export SOOS_PARAMS="--clientId=${SOOS_CLIENT_ID} --apiKey=${SOOS_API_KEY} --projectName=\"${SOOS_PROJECT_NAME}\" --scanMode=${SOOS_SCAN_MODE} --apiURL=${SOOS_API_BASE_URL} --integrationName=GitLab"
  script:
    - cd /zap/
    - python3 main.py ${SOOS_PARAMS} ${SOOS_TARGET_URL}
```

### **API Scan**
* The api scan performs an analysis of APIs.
* In addition to clientId, apiKey, and projectName, the apiscanFormat must be defined. 
    * It must be a specific value: openapi, soap, or graphql
* We recommend defining apiKey and clientId as Environment Variables (see Build Setup section) to protect your credentials

```
image:
  name: "soosio/dast"
  entrypoint: [""]

variables:
  SOOS_PROJECT_NAME: "SOOS-DAST-GitLab"
  SOOS_SCAN_MODE: “apiscan”
  SOOS_API_FORMAT: “openapi” # soap or graphql
  SOOS_API_BASE_URL: "https://api.soos.io/api/"
  SOOS_TARGET_URL: "<Swagger JSON Definition URL>"

Dast-analysis-job:
  before_script:
export PARAMS="--clientId=${SOOS_CLIENT_ID} --apiKey=${SOOS_API_KEY} --projectName=\"${SOOS_PROJECT_NAME}\" --scanMode=${SOOS_SCAN_MODE} --apiScanFormat=${SOOS_API_FORMAT} --apiURL=${SOOS_API_BASE_URL} --integrationName=GitLab" 
  script:
cd /zap/
python3 main.py ${SOOS_PARAMS} ${SOOS_TARGET_URL}
```


##  Run It
To run the SOOS DAST against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.

