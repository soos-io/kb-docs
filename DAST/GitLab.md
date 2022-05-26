# How to Integrate SOOS DAST with your GitLab CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/gitlab.png" alt="GitLab" width="128" height="128">
</div>

This document will walk you through, step-by-step, how to set up a GitLab repository and scan it with the SOOS DAST Product.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a GitLab repo.

## Steps

* When viewing your project, navigate to `Settings`, then to `CI/CD`, and expand the `Variables` section.
* Using the `Client Id` and `API Key` values found on the [GitLab Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=gitlab), create environment variables named `SOOS_CLIENT_ID` and `SOOS_API_KEY` for those values, respectively. These variables will be used by the SOOS CLI.
* Copy the contents of the [`gitlab_dast_baseline.yml`](https://gist.github.com/soostech/7b74eb66cc1bde6cc4506eb67538fc14) file, as seen on the [GitLab Integration page of the SOOS App](https://app.soos.io/integrate/dast?id=gitlab), to your `.gitlab-ci.yml` file.

## Run It
To run the SOOS CLI against your repository, just execute a build or commit a change.

## Scan Modes

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
