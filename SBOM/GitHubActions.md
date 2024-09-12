# How to Integrate SOOS SBOM with Your GitHub Repo

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/github-action.png" alt="Github Action" width="128" height="128">
</div>


Set up a GitHub Action Workflow to scan CycloneDX or SPDX SBOMs with SOOS SBOM.

## Prerequisites

- You need to have a [SOOS account](https://app.soos.io/register).
- You need to have a GitHub repository.

## Steps

### **Repository Setup**
* Create a `.github/workflows` directory in your repository on GitHub if it does not already exist.
* In the `.github/workflows` directory, create a file named `main.yml`. For more information on creating new files, see [Creating new files in GitHub Docs](https://docs.github.com/en/github/managing-files-in-a-repository/creating-new-files).
* Paste the script from the [GitHub Action SBOM Integration page on SOOS](https://app.soos.io/integrate/sbom?id=github-actions).

### **Build Setup**

**Set Up Environment Variables**

Under your Repository's Settings tab, select "Secrets" > "Actions" and add two new secrets containing the SOOS Client Id and API Key, which you can find in the SOOS App under [Integrate](https://app.soos.io/integrate).

The secret names should be `SOOS_CLIENT_ID` and `SOOS_API_KEY`.

<img src="../assets/img/github-action-envs.png">

### **Build Config**
Modify the `.github/workflows/main.yml` file, replacing the `project_name` variable value with one relevant to your project.

## Run It
To run the SOOS CLI against your repository’s code, execute a build or commit a change. The build will use the environment variables created for the API Key and Client ID.

### **Setup**

```yaml
name: Example workflow using SOOS
# Events required to engage workflow (edit this list as needed)
on: [push]

jobs:
  soos_sbom_analysis_example:
    name: SOOS SBOM Analysis Example
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      - name: Run SOOS SBOM Analysis
        uses: soos-io/soos-sbom-github-action@v1 # Use latest version from https://github.com/marketplace/actions/soos-sbom
        with:
          client_id: ${{ secrets.SOOS_CLIENT_ID }}
          api_key: ${{ secrets.SOOS_API_KEY }}
          project_name: "<YOUR-PROJECT-NAME>"
          sbom_path: "SBOM path relative to the repository or leave empty if it's at the root"
```