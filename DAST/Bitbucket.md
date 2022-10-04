# How to Integrate SOOS DAST with Bitbucket
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/bitbucket.png" alt="Bitbucket" width="128" height="128">
</div>
In this article we will make the necessary modifications to a Bitbucket pipeline to scan a site using SOOS DAST.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a https://bitbucket.org account and repository setup.

## Steps

### **Repo/Pipeline Setup**
* Edit (or create a new) `bitbucket-pipelines.yml` file in the root of the repository (`<repo_root>/bitbucket-pipelines.yml`).
* Using an example below script below, add the SOOS DAST script to your repository's `bitbucket-pipelines.yml` file.
* Copy & paste the API key and Client ID values from the [Bitbucket Integration page of the SOOS App](https://app.soos.io/integrate/dast?id=bitbucket). While editing the pipeline file, select "Add Variables" and add two Secured variables, "SOOS_CLIENT_ID" and "SOOS_API_KEY".
* Set a `TARGET_URL` value (the site to scan), by providing a default value in the pipeline file, or by running the pipeline and providing a value in the variables section.
* Configure any additional parameters - see [SOOS DAST Docs](https://github.com/soos-io/soos-dast) for more info.

Based on your preferences you can add the content for the file added on step 1 choosing between three scan modes:

* Baseline Scan
    * It performs a passive scan of HTTP messages (requests and responses) sent to the web application being tested. 
        * Passive scanning does not change the requests nor the responses in any way and is therefore safe to use.
    * The `TARGET_URL` parameter requires a website to test (starting with http(s)://). This value can be set when the pipeline is run.
    * We recommend defining apiKey and clientId as Environment Variables (see Build Setup section) to protect your credentials.

```

image: atlassian/default-image:3

pipelines:
  custom:
    soos-dast:
    - variables:
      - name: SCAN_MODE
        default: "baseline"
        allowed-values:
        - "baseline"
        - "fullscan"
        - "apiscan"
      - name: TARGET_URL
        default: ""
    - parallel:
      - step:
          name: 'SOOS DAST'
          services:
            - docker
          script:
            - docker run --rm soosio/dast:latest --clientId=$SOOS_CLIENT_ID --apiKey=$SOOS_API_KEY --projectName=$BITBUCKET_REPO_SLUG --scanMode=$SCAN_MODE --commitHash=$BITBUCKET_COMMIT --branchName=$BITBUCKET_BRANCH --buildURI="http://bitbucket.org/$BITBUCKET_REPO_FULL_NAME" --buildVersion=$BITBUCKET_BUILD_NUMBER --integrationName="Bitbucket" $TARGET_URL
```

* Full Scan 
    * This scan mode is used to perform a full analysis of a web-app. 
    * It performs an active scan, attempting to find potential vulnerabilities by using known attacks against the selected targets. 
        * It should be noted that active scanning can only find certain types of vulnerabilities. Logical vulnerabilities, such as broken access control, will not be found by any active or automated vulnerability scanning.
    * The `TARGET_URL` parameter requires a website to test (starting with http(s)://). This value can be set when the pipeline is run.
    * We recommend defining apiKey and clientId as Environment Variables (see Build Setup section) to protect your credentials.

```
image: atlassian/default-image:3

pipelines:
  custom:
    soos-dast:
    - variables:
      - name: SCAN_MODE
        default: "fullscan"
        allowed-values:
        - "baseline"
        - "fullscan"
        - "apiscan"
      - name: TARGET_URL
        default: ""
    - parallel:
      - step:
          name: 'SOOS DAST'
          services:
            - docker
          script:
            - docker run --rm soosio/dast:latest --clientId=$SOOS_CLIENT_ID --apiKey=$SOOS_API_KEY --projectName=$BITBUCKET_REPO_SLUG --scanMode=$SCAN_MODE --commitHash=$BITBUCKET_COMMIT --branchName=$BITBUCKET_BRANCH --buildURI="http://bitbucket.org/$BITBUCKET_REPO_FULL_NAME" --buildVersion=$BITBUCKET_BUILD_NUMBER --integrationName="Bitbucket" $TARGET_URL
```

* API Scan 
    * The api scan performs an analysis of APIs.
    * The apiScanFormat must be defined as one of: openapi, soap, or graphql
    * The `TARGET_URL` parameter in this scenario requires a publicly available URL where the spec file is hosted.
    * We recommend defining apiKey and clientId as Environment Variables (see Build Setup section) to protect your credentials.

```
image: atlassian/default-image:3

pipelines:
  custom:
    soos-dast:
    - variables:
      - name: SCAN_MODE
        default: "apiscan"
        allowed-values:
        - "baseline"
        - "fullscan"
        - "apiscan"
      - name: API_FORMAT
        default: "openapi"
        allowed-values:
        - "openapi"
        - "soap"
        - "graphql"
      - name: TARGET_URL
        default: ""
    - parallel:
      - step:
          name: 'SOOS DAST'
          services:
            - docker
          script:
            - docker run --rm soosio/dast:latest --clientId=$SOOS_CLIENT_ID --apiKey=$SOOS_API_KEY --projectName=$BITBUCKET_REPO_SLUG --scanMode=$SCAN_MODE --apiScanFormat=$API_FORMAT --commitHash=$BITBUCKET_COMMIT --branchName=$BITBUCKET_BRANCH --buildURI="http://bitbucket.org/$BITBUCKET_REPO_FULL_NAME" --buildVersion=$BITBUCKET_BUILD_NUMBER --integrationName="Bitbucket" $TARGET_URL
```

### **Build Setup**
**Setup Environment Variables**

Copy & paste the API key and Client ID values from the [Bitbucket Integration page of the SOOS App](https://app.soos.io/integrate/dast?id=bitbucket). While editing the pipeline file, select "Add Variables" and add two *Secured* variables, "SOOS_CLIENT_ID" and "SOOS_API_KEY".

**Run It**

To run the SOOS DAST Analysis against your webapp or API, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.
