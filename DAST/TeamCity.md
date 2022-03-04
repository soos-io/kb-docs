# How to Integrate SOOS DAST with your Teamcity CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/teamcity.png" alt="TeamCity" width="128" height="128">
</div>
Currently, you can integrate the SOOS DAST Analysis with TeamCity using the Docker build runner and setting the command and arguments.


## Prerequisites
- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a TeamCity project created

### Steps

### **Getting the script**
* Navigate to the [TeamCity DAST integration page on the SOOS App](https://app.soos.io/integrate/dast?id=teamcity) and copy the integration script content.

### **Installing Docker and configure TeamCity**
The script uses docker so it's fundamental for it to work that the agent running the script have [docker installed.](https://docs.docker.com/get-docker/).

* Visit https://www.docker.com/ and download & install Docker.
* The Docker build runner needs parameters, called “System Properties (system.)” in TeamCity, which have to be declared inside the project or the build settings, and they are required for the Docker container to run.
* You will need some required properties:
    * system.apiKey: SOOS API key
    * system.clientId: SOOS client id
    * system.apiURL: SOOS API URL. By Default: https://api.soos.io/api/
    * system.projectName: SOOS project name
    * system.scanMode: SOOS DAST scan mode. Values: baseline (Default), fullscan, apiscan, or activescan
    * system.targetURL: target URL including the protocol, eg https://www.example.com

### **Build Step**

Create a Build Step with the following settings:

* Runner type: Docker
* Step name: <insert desired name here>
* Docker command: select ‘other…’
* Command name: run
* Working directory: (leave blank)
* Additional required arguments for the command: 
`--rm soosio/dast %system.targetURL% --clientId=%system.clientId% --apiKey=%system.apiKey% --projectName="""%system.projectName%""" --scanMode=%system.scanMode% --apiURL=%system.apiURL%`

Click Save when complete


You can choose between three jobs:


**Baseline Analysis**
* Runs the ZAP spider against the specified target for (by default) 1 minute and then waits for the passive scanning to complete before reporting the results.
    * This means that the script doesn't perform any actual ‘attacks’ and will run for a relatively short period of time (a few minutes at most).
* By default, it reports all alerts as WARNINGS but you can specify a config file which can change any rules to FAIL or IGNORE.
* This mode is intended to be ideal to run in a CI/CD environment, even against production sites.

**Full Analysis**

* Runs the ZAP spider against the specified target (by default with no time limit), followed by an optional ajax spider scan and then a full active scan before reporting the results.
    * This means that the script does perform actual attacks and can potentially run for a long period of time.
* By default, it reports all alerts as WARNINGS but you can specify a config file which can change any rules to FAIL or IGNORE. The configuration works in a very similar way as the Baseline Analysis.

**API Analysis** 

* This is tuned for performing scans against APIs defined by OpenAPI, SOAP, or GraphQL via either a local file or a URL.
* It imports the definition that you specify and then runs an Active Scan against the URLs found. The Active Scan is tuned to APIs, so it doesn't bother looking for things like XSSs.
* It also includes 2 scripts that:
    * Raise alerts for any HTTP Server Error response codes.
    * Raise alerts for any URLs that return content types that are not usually associated with APIs.
