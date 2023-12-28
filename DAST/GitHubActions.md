# How to Integrate SOOS DAST with your Github Action
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/github-action.png" alt="Github Action" width="128" height="128">
</div>
In this article we will make the necessary modifications to a GitHub Workflow using the SOOS DAST Analysis GitHub Action to scan a GitHub repository with SOOS.

## Prerequisites
- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a GitHub repo.

## Steps

### **Repo Setup**

Create a `.github/workflows` directory in your repository on GitHub if this directory does not already exist.

In the .github/workflows directory, create a file named main.yml.

Paste the following code:

```yaml
on: [push]
 
jobs:
  soos_dast_analysis_example:
    name: SOOS DAST Analysis Example
    runs-on: ubuntu-latest
    steps:
      - name: Run SOOS DAST Analysis
        uses: soos-io/soos-dast-github-action@v2 # GET Latest Version from https://github.com/marketplace/actions/soos-dast
        with:
          client_id: ${{ secrets.SOOS_CLIENT_ID }}
          api_key: ${{ secrets.SOOS_API_KEY }}
          project_name: <Project Name>
          scan_mode: <Scan Mode> # "baseline", fullscan, or apiscan
          apiscan_format: 'openapi' # Mandatory if you select apiscan mode 
          target_url: <Target URL>
```

Note: The `target_url` **MUST** begin with `http://` or `https://`

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

It is tuned for performing scans against APIs defined by OpenAPI, SOAP, or GraphQL via a URL.

It imports the definition that you specify and then runs an Active Scan against the URLs found. The Active Scan is tuned to APIs, so it doesn't bother looking for things like XSSs.

It also includes 2 scripts that:
   - Raise alerts for any HTTP Server Error response codes
   - Raise alerts for any URLs that return content types that are not usually associated with APIs

### **Build Setup**

Setup Environment Variables

Under your Repository's Settings tab, select "Secrets" > "Actions" and add two new secrets which contain the SOOS Client Id and API Key which you can find in the SOOS App under [Integrate](https://app.soos.io/integrate)

The secret names should be "SOOS_CLIENT_ID" and "SOOS_API_KEY"

<img src="../assets/img/github-action-envs.png">


### **Run It**

To run the SOOS DAST Analysis against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.

## **Configure GitHub Code Scan Output**

If you are using GitHub Enterprise or your repository is public, you can configure the SOOS Action to display any issues in GitHub Code Scanning Alerts. There are a few additional steps to get this configured.

### **Example worfklow setup for SARIF Upload**

``` yaml
on: [push]
 
jobs:
  soos:
    permissions:
      security-events: write # for uploading code scanning alert info
    name: SOOS DAST Analysis Example
    runs-on: ubuntu-latest
    steps:
      - name: Run SOOS DAST Analysis
        uses: soos-io/soos-dast-github-action@v2 # GET Latest Version from https://github.com/marketplace/actions/soos-dast
        with:
          client_id: ${{ secrets.SOOS_CLIENT_ID }}
          api_key: ${{ secrets.SOOS_API_KEY }}
          project_name: <Project Name>
          scan_mode: <Scan Mode> # "baseline", fullscan, or apiscan
          apiscan_format: 'openapi' # Mandatory if you select apiscan mode 
          target_url: <Target URL>
          output_format: "sarif"
      - name: Upload SOOS DAST Report # 3rd party action to upload sarif results to your github repo
        uses: github/codeql-action/upload-sarif@v2
        with:
          sarif_file: results.sarif
```

**NOTE:** If you don't have a checkout step you might encounter an error in the logs for the Upload-Sarif action. This can be ignored (it's a non-issue) but if you want to keep your log clean, just add a checkout step in your workflow before the scan step.


## Authenticated scans

### Using bearer token

If you need to run a scan against url that needs authorization and the only thing needed is to set an authorization header in the form of `authorization: Bearer token-value` then this is the most straight forward workflow (note that for this method you should have the bearer token value beforehand).

example workflow:

``` yaml
on: [push]
jobs:
  synchronous-analysis-with-blocking-result:
    name: SOOS DAST Scan
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: Run SOOS DAST Baseline Analysis performing bearer token authentication
      uses: soos-io/soos-dast-github-action@v2 # GET Latest Version from https://github.com/marketplace/actions/soos-dast
      with:
        client_id: ${{ secrets.SOOS_CLIENT_ID }}
        api_key: ${{ secrets.SOOS_API_KEY }}
        project_name: "DAST-bearer-token"
        scan_mode: "baseline"
        bearer_token: "token-value"        
        target_url: "https://www.example.com/"
```

### Authenticate throughout a login form and get the auth token.

Using this option there will be an automated login form authentication performed before running the DAST scan to get the bearer token that will be then added to every request as the authorization header.

This is how a example workflow will look like:

``` yaml

on: [push]

jobs:
  synchronous-analysis-with-blocking-result:
    name: SOOS DAST Scan
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: Run SOOS DAST Baseline Analysis performing form authentication
      uses: soos-io/soos-dast-github-action@v2 # GET Latest Version from https://github.com/marketplace/actions/soos-dast
      with:
        client_id: ${{ secrets.SOOS_CLIENT_ID }}
        api_key: ${{ secrets.SOOS_API_KEY }}
        project_name: "DAST-login-form"
        scan_mode: "baseline"
        target_url: "https://example.com/"
        auth_login_url: "https://example.com/login"
        auth_username: "username-to-fill-field"
        auth_password: "password-to-fill-field"
        auth_username_field: "username-html-input-id"
        auth_password_field: "password-html-input-id"
        auth_submit_field: "submit-html-input-id"

```

### Authenticate against an OAuth token url.

In case you need to perform a DAST analysis against an OAuth application this is the workflow that you should follow. In this scenario the DAST tool will perform a request to get the `access_token` before doing any analysis.

Workflow example:

``` yaml
on: [push]

jobs:
  synchronous-analysis-with-blocking-result:
    name: SOOS DAST Scan
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: Run SOOS DAST Baseline Analysis performing OAuth
      uses: soos-io/soos-dast-github-action@v2 # GET Latest Version from https://github.com/marketplace/actions/soos-dast
      with:
        client_id: ${{ secrets.SOOS_CLIENT_ID }}
        api_key: ${{ secrets.SOOS_API_KEY }}
        project_name: "DAST-OAuth"
        scan_mode: "baseline"
        target_url: "https://example.com/"
        oauth_token_url: "https://example.com/token"
        oauth_parameters: "client_secret:value ,client_id:value , grant_type:value"
```
