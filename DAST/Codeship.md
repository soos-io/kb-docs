# How to Integrate SOOS DAST with your Codeship CI

<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/codeship.png" alt="Codeship" width="128" height="128">

This document will take you step-by-step through the tasks required to set up a Codeship repo, for scan it with the SOOS DAST Product.
## Prerequisites

- You need to have a SOOS account.
- You need to have a Codeship Pro account.

## Steps

### **Repo Setup**
- Create a new file in your repository: `codeship-services.yml` and copy this content:
```
soos-dast-analysis:
  image: docker:stable-dind
  add_docker: true
  environment:
    - SOOS_PROJECT_NAME=<Project Name>
    - SOOS_TARGET_URL=<Url to be Scanned>
    - SOOS_CLIENT_ID: <SOOS Client Id from SOOS App>
    - SOOS_API_KEY: <SOOS API Key from SOOS App>
  cached: true
```

- Decide which configuration fits your needs from the **Add The Build Configuration** section and paste the snippet inside your `codeship-steps.yml` file.
- Commit these 2 new files and the new folder path to GitHub.

### **Add The Build Configuration**
Based on your preferences you can add the content for the file `codeship-steps.yml` choosing between three scan modes:
* Baseline Scan 
    * It performs a passive scan of HTTP messages (requests and responses) sent to the web application being tested. 
        * Passive scanning does not change the requests nor the responses in any way and is therefore safe to use.
    * The parameters clientId, apiKey, and projectName are required
    * We recommend defining apiKey and clientId as Environment Variables    (see Build Setup section) to protect your credentials

```
- name: 'DAST SOOS Analysis'
  service: soos-dast-analysis
  command: /bin/sh -c 'docker run -it --rm soosio/dast --clientId=$SOOS_CLIENT_ID --apiKey=$SOOS_APIKEY --projectName=$SOOS_PROJECT_NAME $TARGET_URL'
```
* Full Scan 
    * This scan mode is used to perform a full analysis of a web-app. 
    * It performs an active scan, attempting to find potential vulnerabilities by using known attacks against the selected targets. 
        * It should be noted that active scanning can only find certain types of vulnerabilities. Logical vulnerabilities, such as broken access control, will not be found by any active or automated vulnerability scanning.
    * The parameters clientId, apiKey, and projectName are required
    * We recommend defining apiKey and clientId as Environment Variables (see Build Setup section) to protect your credentials
```
- name: 'DAST SOOS Analysis'
  service: soos-dast-analysis
  command: /bin/sh -c 'docker run -it --rm soosio/dast --clientId=$SOOS_CLIENT_ID --apiKey=$SOOS_APIKEY --projectName=$SOOS_PROJECT_NAME —-scanMode=fullscan $TARGET_URL'
```
* API Scan 
    * The api scan performs an analysis of APIs.
    * In addition to clientId, apiKey, and projectName, the apiscanFormat must be defined. 
        * It must be a specific value: openapi, soap, or graphql
    * We recommend defining apiKey and clientId as Environment Variables (see Build Setup section) to protect your credentials
```
- name: 'DAST SOOS Analysis'
  service: soos-dast-analysis
  command: /bin/sh -c 'docker run -it --rm soosio/dast --clientId=$SOOS_CLIENT_ID --apiKey=$SOOS_APIKEY --projectName=$SOOS_PROJECT_NAME —-scanMode=apiscan –apiscanFormat=openapi $TARGET_URL'
```

### Run It
To run the SOOS DAST  against your repository’s code, just execute a build or commit a change.


