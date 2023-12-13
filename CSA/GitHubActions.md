# How to Integrate SOOS CSA with your Github Action
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/github-action.png" alt="Github Action" width="128" height="128">
</div>
In this article we will add the SOOS Container Security Analysis (CSA) GitHub Action to a GitHub Workflow and scan a GitHub repository.

## Prerequisites
- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a GitHub repo.

## Steps

### **Repo Setup**

Create a `.github/workflows` directory in your repository on GitHub if this directory does not already exist.

In the .github/workflows directory, create a file named main.yml.

Paste the following code:

``` yaml
on: [push]
 
jobs:
  soos_csa_analysis_example:
    name: SOOS Container Security Analysis (CSA) Example
    runs-on: ubuntu-latest
    steps:
      - name: Run SOOS CSA Analysis
        uses: soos-io/soos-csa-github-action@vX.Y.Z # GET Latest Version from https://github.com/marketplace/actions/soos-csa
        with:
          client_id: ${{ secrets.SOOS_CLIENT_ID }}
          api_key: ${{ secrets.SOOS_API_KEY }}
          project_name: "<YOUR-PROJECT-NAME>"
      	  target_image: "image:tag"
```


### **Build Setup**

Setup Environment Variables

Under your Repository's Settings tab, select "Secrets" > "Actions" and add two new secrets which contain the SOOS Client Id and API Key which you can find in the SOOS App under [Integrate](https://app.soos.io/integrate/containers)

The secret names should be "SOOS_CLIENT_ID" and "SOOS_API_KEY"

<img src="../assets/img/github-action-envs.png">


### **Run It**

To run the SOOS CSA Analysis against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.

## **Configure GitHub Code Scan Output**

If you are using GitHub Enterprise or your repository is public, you can configure the SOOS Action to display any issues in GitHub Code Scanning Alerts. There are a few additional steps to get this configured.

### **Example worfklow setup for SARIF Upload**

``` yaml
on: [push]
 
jobs:
  soos_csa_analysis_example:
    name: SOOS Container Security Analysis (CSA) Example
    runs-on: ubuntu-latest
    steps:
      - name: Run SOOS CSA Analysis
        uses: soos-io/soos-csa-github-action@vX.Y.Z # GET Latest Version from https://github.com/marketplace/actions/soos-csa
        with:
          client_id: ${{ secrets.SOOS_CLIENT_ID }}
          api_key: ${{ secrets.SOOS_API_KEY }}
          project_name: "<YOUR-PROJECT-NAME>"
      	  target_image: "image:tag"
          output_format: "sarif"
      - name: Upload SOOS CSA Report # 3rd party action to upload Sarif results to your github repository
        uses: github/codeql-action/upload-sarif@v2
        with:
          sarif_file: results.sarif
```

**NOTE:** If you don't have a checkout step you might encounter an error in the logs for the Upload-Sarif action. This can be ignored (it's a non-issue) but if you want to keep your log clean, just add a checkout step in your workflow before the scan step.


## Authenticated scans

### Using bearer token

If you need to run a scan against an image from a private repository we suggest to download the image on the agent performing the authentication using your provider official action and then indicate the path to the image as the `target_image` value.

example workflow with amazon ecr:

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
          aws-region: <YOUR-AWS-REGION>

      - name: Login to Amazon ECR
        uses: aws-actions/amazon-ecr-login@v2
        with:
          registries: <YOUR-REGISTRY-ID>

      - name: Pull Docker image from ECR
        run: docker pull <IMAGE-FULL-URL>
      - name: Run SOOS CSA analysis testing
        uses: soos-io/soos-csa-github-action@vX.Y.Z # GET Latest Version from https://github.com/marketplace/actions/soos-csa
        with:
          client_id: ${{ secrets.SOOS_CLIENT_ID }}
          api_key: ${{ secrets.SOOS_API_KEY }}
          project_name: "<YOUR-PROJECT-NAME>"
          target_image: <IMAGE-FULL-PATH (SAME AS THE ONE PROVIDED ON THE DOCKER PULL COMMAND)>
```