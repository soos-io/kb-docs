# How to Integrate SOOS DAST with your GitLab CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/gitlab.png" alt="GitLab" width="128" height="128">
</div>

Set up a GitLab repository project and scan an endpoint with SOOS DAST

## Prerequisites
- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a GitLab repository.

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
* The `clientId`, `apiKey`, and `projectName` parameters are required.

```
soos-dast-analysis:
  stage: deploy
  image:
    name: "soosio/dast"
    entrypoint: [""]
  variables:
    TARGET_URL: "https://example.com"
    SCAN_MODE: "baseline"
  script:
    - cd /zap/
    - node index.js
      --clientId="${SOOS_CLIENT_ID}"
      --apiKey="${SOOS_API_KEY}"
      --scanMode="${SCAN_MODE}"
      --integrationName="GitLab"
      --projectName="${CI_PROJECT_TITLE}"
      --branchName="${CI_COMMIT_BRANCH}"
      --commitHash="${CI_COMMIT_SHA}"
      --buildURI="${CI_JOB_URL}"
      ${TARGET_URL}
```

### **Full Scan**
* This scan mode is used to perform a full analysis of a web-app. 
* It performs an active scan, attempting to find potential vulnerabilities by using known attacks against the selected targets. 
    * It should be noted that active scanning can only find certain types of vulnerabilities. Logical vulnerabilities, such as broken access control, will not be found by any active or automated vulnerability scanning.
* The `clientId`, `apiKey`, and `projectName` parameters are required.

```
soos-dast-analysis:
  stage: deploy
  image:
    name: "soosio/dast"
    entrypoint: [""]
  variables:
    TARGET_URL: "https://example.com"
    SCAN_MODE: "fullscan"
  script:
    - cd /zap/
    - node index.js
      --clientId="${SOOS_CLIENT_ID}"
      --apiKey="${SOOS_API_KEY}"
      --scanMode="${SCAN_MODE}"
      --integrationName="GitLab"
      --projectName="${CI_PROJECT_TITLE}"
      --branchName="${CI_COMMIT_BRANCH}"
      --commitHash="${CI_COMMIT_SHA}"
      --buildURI="${CI_JOB_URL}"
      ${TARGET_URL}
```

### **API Scan**
* This scan mode performs an analysis of an API, the TARGET_URL should be the spec file URL where its publicly available.
* In addition to `clientId`, `apiKey`, and `projectName`, the `apiscanFormat` parameter is also required.
* **NOTE:** Make sure that in case of specifying a file you are pointing to the full path on the repository.

```
soos-dast-analysis:
  stage: deploy
  image:
    name: "soosio/dast"
    entrypoint: [""]
  variables:
    TARGET_URL: "<URL OR FULL PATH TO THE SPEC THE FILE IN THE REPOSITORY>"
    SCAN_MODE: "apiscan"
    API_FORMAT: "openapi" # "soap" or "graphql"
  script:
    - cd /zap/
    - node dist/index.js
      --clientId="${SOOS_CLIENT_ID}"
      --apiKey="${SOOS_API_KEY}"
      --scanMode="${SCAN_MODE}"
      --apiScanFormat="${API_FORMAT}"
      --integrationName="GitLab"
      --projectName="${CI_PROJECT_TITLE}"
      --branchName="${CI_COMMIT_BRANCH}"
      --commitHash="${CI_COMMIT_SHA}"
      --buildURI="${CI_JOB_URL}"
      ${TARGET_URL}
```

## Reference
* To see the full list of available parameters go to [DAST repository parameters description](https://github.com/soos-io/soos-dast#parameters)
