# How to Integrate SOOS SBOM with Your GitHub Repo

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/github-action.png" alt="Github Action" width="128" height="128">
</div>


Set up a GitHub Action Workflow to scan CycloneDX or SPDX SBOMs with SOOS SBOM Analysis.

## Prerequisites

- You need to have a [SOOS account](https://app.soos.io/register) with SBOM scanning enabled.
- Node 22 LTS or higher enabled in the workflow.

## Steps

### **Repository Setup**
* Create a `.github/workflows` directory in your repository on GitHub if it does not already exist.

### **Get the Example**

* Navigate to the [GitHub Action SBOM integration page on the SOOS App](https://app.soos.io/integrate/sbom?id=github-actions), copy the example, and modify it.

### **Run It**

* Execute the workflow

---

## Reference
* To see the full list of available parameters go to [SOOS SBOM GitHub Action Scan Parameters](https://github.com/soos-io/soos-sbom-github-action)