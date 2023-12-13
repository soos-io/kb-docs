# How to Integrate SOOS SBOM with your GitHub Repo
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/github-action.png" alt="Github Action" width="128" height="128">
</div>
In this article we will make the necessary modifications to a GitHub Workflow using the SOOS SBOM Analysis GitHub Action to scan a GitHub repository with SOOS.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a GitHub Repo.

## Steps

### **Repo Setup**
* Create a `.github/workflows` directory in your repository on GitHub if this directory does not already exist.
* In the `.github/workflows` directory, create a file named main.yml.
For more information, see "Creating new files" in GitHub Docs.
* Paste the script given in the [GitHub Action SBOM Integration page on SOOS](https://app.soos.io/integrate/sbom?id=github-actions).


### **Build Setup**

Setup Environment Variables

Under your Repository's Settings tab, select "Secrets" > "Actions" and add two new secrets which contain the SOOS Client Id and API Key which you can find in the SOOS App under [Integrate](https://app.soos.io/integrate)

The secret names should be "SOOS_CLIENT_ID" and "SOOS_API_KEY"

<img src="../assets/img/github-action-envs.png">

### **Build Config**
Modify the `.github/workflows/main.yml` file, replacing the provided project_name variable value with one that is relevant to your project.

## Run It
To run the SOOS CLI against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.

 
### **Setup**

```yaml
name: Example workflow using SOOS
# Events required to engage workflow (add/edit this list as needed)
on: [push]

jobs:
  soos_sbom_analysis_example:
    name: SOOS SBOM Analysis Example
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      - name: Run SOOS SBOM Analysis
        uses: soos-io/soos-sbom-github-action@v1 # GET Latest Version from https://github.com/marketplace/actions/soos-sbom
        with:
          client_id: ${{ secrets.SOOS_CLIENT_ID }}
          api_key: ${{ secrets.SOOS_API_KEY }}
          project_name: "<YOUR-PROJECT-NAME>"
          sbom_path: "SBOM path relative to the repository or empty if it's on the root"
```