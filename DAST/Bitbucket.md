# How to Integrate SOOS DAST with Bitbucket
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/bitbucket.png" alt="Bitbucket" width="128" height="128">
</div>
Set up a Bitbucket pipeline project and scan an endpoint with SOOS DAST.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a https://bitbucket.org account and repository setup.

## Steps

### **Repo/Pipeline Setup**
* Edit (or create a new) `bitbucket-pipelines.yml` file in the root of the repository (`<repo_root>/bitbucket-pipelines.yml`).
* Using an example below script below, add the SOOS DAST script to your repository's `bitbucket-pipelines.yml` file.
* Copy & paste the API key and Client ID values from the [Bitbucket Integration page of the SOOS App](https://app.soos.io/integrate/dast?id=bitbucket). While editing the pipeline file, select "Add Variables" and add two Secured variables, "SOOS_CLIENT_ID" and "SOOS_API_KEY".
* Set a `<target_url>` value (the site to scan), by providing a default value in the pipeline file, or by running the pipeline and providing a value in the variables section.
* Configure any additional parameters - see [SOOS DAST Docs](https://github.com/soos-io/soos-dast) for more info.

Based on your preferences you can add the content for the file added on step 1 choosing between three scan modes:

* Baseline Scan
    * It performs a passive scan of HTTP messages (requests and responses) sent to the web application being tested. 
        * Passive scanning does not change the requests nor the responses in any way and is therefore safe to use.
    * The `<target_url>` parameter requires a website to test (starting with http(s)://). This value can be set when the pipeline is run.
    * We recommend defining apiKey and clientId as Environment Variables (see Build Setup section) to protect your credentials.

```
image: atlassian/default-image:3

pipelines:
  custom:
    soos-dast:
    - parallel:
      - step:
          name: 'SOOS DAST'
          services:
            - docker
          script:
            - docker run --rm soosio/dast --clientId=<client_id> --apiKey=<api_key> --projectName="<project_name>" --integrationName="BitBucket" <target_url>
```

* Full Scan 
    * This scan mode is used to perform a full analysis of a web-app. 
    * It performs an active scan, attempting to find potential vulnerabilities by using known attacks against the selected targets. 
        * It should be noted that active scanning can only find certain types of vulnerabilities. Logical vulnerabilities, such as broken access control, will not be found by any active or automated vulnerability scanning.
    * The `<target_url>` parameter requires a website to test (starting with http(s)://). This value can be set when the pipeline is run.
    * We recommend defining apiKey and clientId as Environment Variables (see Build Setup section) to protect your credentials.

```
image: atlassian/default-image:3

pipelines:
  custom:
    soos-dast:
    - parallel:
      - step:
          name: 'SOOS DAST'
          services:
            - docker
          script:
            - docker run --rm soosio/dast --clientId=<client_id> --apiKey=<api_key> --projectName="<project_name>" --scanMode=fullscan --integrationName="BitBucket" <target_url>
```

* API Scan 
    * The api scan performs an analysis of APIs.
    * The apiScanFormat must be defined as one of: openapi, soap, or graphql
    * The `<target_url>` parameter in this scenario requires a publicly available URL where the spec file is hosted.
    * We recommend defining apiKey and clientId as Environment Variables (see Build Setup section) to protect your credentials.

```
image: atlassian/default-image:3

pipelines:
  custom:
    soos-dast:
    - parallel:
      - step:
          name: 'SOOS DAST'
          services:
            - docker
          script:
            - docker run --rm soosio/dast --clientId=<client_id> --apiKey=<api_key> --projectName="<project_name>" --scanMode=apiscan --apiScanFormat=openapi --integrationName="BitBucket" <target_url>
```

**Run It**

To run the SOOS DAST Analysis against your webapp or API, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.

## Reference
* To see the full list of available parameters go to [DAST repository parameters description](https://github.com/soos-io/soos-dast#parameters)