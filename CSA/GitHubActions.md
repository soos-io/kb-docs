# How to Integrate SOOS CSA with your Github Action
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/github-action.png" alt="Github Action" width="128" height="128">
</div>
Set up a GitHub Workflow and scan it with SOOS Container Security Analysis (CSA).

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register) with Container scanning enabled.
- You need to have a GitHub repository.
- The action must run the task on a Linux build agent.

## Steps

### **Workflow Setup**

Create a `.github/workflows` directory in your repository on GitHub if this directory does not already exist.

In the .github/workflows directory, create a file named main.yml.

### **Get the Example**

* Navigate to the [GitHub Action DAST integration page on the SOOS App](https://app.soos.io/integrate/containers?id=github-actions), copy the example, and modify it.

### **Run It**

* Execute the workflow

## **Configure GitHub Code Scan Output**

If you are using GitHub Enterprise or your repository is public, you can configure the SOOS Action to display any issues in GitHub Code Scanning Alerts.

### **Example Workflow Setup for SARIF Upload**

``` yaml
on: [push]
 
jobs:
  soos_csa_analysis_example:
    name: SOOS Container Security Analysis (CSA) Example
    runs-on: ubuntu-latest
    steps:
      - name: Run SOOS CSA Analysis with SARIF output
        uses: soos-io/soos-csa-github-action@v2 # GET Latest Version from https://github.com/marketplace/actions/soos-csa
        with:
          client_id: ${{ secrets.SOOS_CLIENT_ID }}
          api_key: ${{ secrets.SOOS_API_KEY }}
          project_name: "<project_name>"
          target_image: "<image:tag>"
          output_format: "sarif"
      - name: Upload SOOS CSA Report # 3rd party action to upload SARIF results to your GitHub repository
        uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: results.sarif
```

**NOTE:** If you don't have a checkout step, you might encounter an error in the logs for the Upload-Sarif action. This can be ignored (it's a non-issue) but if you want to keep your log clean, just add a checkout step in your workflow before the scan step.

## Scanning a private image

If you need to run a scan against an image from a private repository, we suggest downloading the image on the agent, performing the authentication using your provider's official action, and then indicating the path to the image as the `target_image` value.

Example workflow with Amazon ECR:

``` yaml
on: [push]

jobs:
  csa-scan:
    name: SOOS CSA Scan with ECR Registry
    runs-on: ubuntu-latest
    steps:
      - name: Configure AWS credentials
        id: login-ecr
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: <aws_region>

      - name: Login to Amazon ECR
        uses: aws-actions/amazon-ecr-login@v2
        with:
          registries: <registry_id>

      - name: Pull Docker image from ECR
        run: docker pull <full_image_url>
      - name: Run SOOS CSA analysis testing
        uses: soos-io/soos-csa-github-action@v1 # GET Latest Version from https://github.com/marketplace/actions/soos-csa
        with:
          client_id: ${{ secrets.SOOS_CLIENT_ID }}
          api_key: ${{ secrets.SOOS_API_KEY }}
          project_name: "<project_name>"
          target_image: <full_path_to_image (matches docker pull path)>
```

---

## Reference
* To see the full list of available parameters go to the [CSA repository parameters description](https://github.com/soos-io/soos-csa#parameters).