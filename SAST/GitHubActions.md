# How to Integrate SOOS SAST with Your GitHub Repo

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/github-action.png" alt="Github Action" width="128" height="128">
</div>

Set up an GitHub Action Workflow to scan SARIF 2.1 files with SOOS SAST.

## Prerequisites

- You need to have a [SOOS account](https://app.soos.io/register) with SAST scanning enabled.
- Node 22 LTS or higher enabled in the workflow.
- You need to have previously generated Sarif 2.1 json in file(s) that end with the `.sarif.json` extension.

## Steps

### **Repository Setup**
* Create a `.github/workflows` directory in your repository on GitHub if it does not already exist.

### **Get the Example**

* Navigate to the [GitHub Action SAST integration page on the SOOS App](https://app.soos.io/integrate/sast?id=github-action), copy the example, and modify it.

### **Run It**

* Execute the workflow

---

## Reference
* To see the full list of available parameters go to [SOOS SAST GitHub Action Scan Parameters](https://github.com/soos-io/soos-sast-github-action)