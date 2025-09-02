# How to Integrate SOOS DAST with your Github Action
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/github-action.png" alt="Github Action" width="128" height="128">
</div>
Set up a GitHub Workflow and scan an endpoint with the SOOS DAST GitHub Action

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register) with DAST scanning enabled.
- The action must run the task on a Linux build agent.

## Steps

### **Repository Setup**

- Create a `.github/workflows` directory in your repository on GitHub if this directory does not already exist.
- In the .github/workflows directory, create a file named main.yml.

### **Get the Example**

* Navigate to the [GitHub Action DAST integration page on the SOOS App](https://app.soos.io/integrate/dast?id=github-actions), copy the example, and modify it.

### **Run It**

* Execute the workflow

## **Configure Sarif Output for CodeQL**

If you are using GitHub Enterprise or your repository is public, you can configure the SOOS Action to display any issues in GitHub Code Scanning Alerts. There are a few additional steps to get this configured.

### **Example workflow setup for SARIF Upload**

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
        uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: results.sarif
```

**NOTE:** If you don't have a checkout step you might encounter an error in the logs for the Upload-Sarif action. This can be ignored (it's a non-issue) but if you want to keep your log clean, just add a checkout step in your workflow before the scan step.

---

## Reference
* To see the full list of available parameters go to [SOOS DAST GitHub Action Scan Parameters](https://github.com/soos-io/soos-dast-github-action)