# How to Integrate SOOS DAST with your Github Action
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/github-action.png" alt="Github Action" width="128" height="128">
</div>
In this article we will make the necessary modifications to a GitHub Workflow using the SOOS DAST Analysis GitHub Action to scan a GitHub repository with SOOS.
## Prerequisites

- You need to have a SOOS account.
- You need to have a Github repo.

## Steps

### **Repo Setup**

Create a `.github/workflows` directory in your repository on GitHub if this directory does not already exist.

In the .github/workflows directory, create a file named main.yml.

Paste the following code:

```
on: [push]
 
jobs:
  soos_dast_analysis_example:
    name: SOOS DAST Analysis Example
    runs-on: ubuntu-latest
    steps:
      - name: Run SOOS DAST Analysis
        uses: soos-io/soos-dast-github-action@v1
        with:
          client_id: ${{ secrets.SOOS_CLIENT_ID }}
          api_key: ${{ secrets.SOOS_API_KEY }}
          project_name: <Project Name>
          scan_mode: <Scan Mode> # "baseline", fullscan, or apiscan
          apiscan_format: 'openapi' # Mandatory if you select apiscan mode 
          target_url: <Target URL>
```

### **Scan Modes**
**Baseline Analysis**

It runs the ZAP spider against the specified target for (by default) 1 minute and then waits for the passive scanning to complete before reporting the results.

This means that the script doesn't perform any actual ‘attacks’ and will run for a relatively short period of time (a few minutes at most).

By default, it reports all alerts as WARNings but you can specify a config file which can change any rules to FAIL or IGNORE.

This mode is intended to be ideal to run in a CI/CD environment, even against production sites.

**Full Analysis**

It runs the ZAP spider against the specified target (by default with no time limit) followed by an optional ajax spider scan and then a full active scan before reporting the results.

This means that the script does perform actual ‘attacks’ and can potentially run for a long period of time.

By default, it reports all alerts as WARNings but you can specify a config file which can change any rules to FAIL or IGNORE. The configuration works in a very similar way as the Baseline Analysis

**API Analysis**

It is tuned for performing scans against APIs defined by OpenAPI, SOAP, or GraphQL via either a local file or a URL.

It imports the definition that you specify and then runs an Active Scan against the URLs found. The Active Scan is tuned to APIs, so it doesn't bother looking for things like XSSs.

It also includes 2 scripts that:
   - Raise alerts for any HTTP Server Error response codes
   - Raise alerts for any URLs that return content types that are not usually associated with APIs

### **Build Setup**

Setup Environment Variables

Navigate to your Project’s Settings > Secrets menu and in the “Repository secrets” section, create the SOOS_API_KEY and SOOS_CLIENT_ID secrets. These will serve as environment variables to be used by the SOOS CLI.

<img src="../assets/img/github-action-envs.png">


### **Run It**

To run the SOOS DAST Analysis against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.
