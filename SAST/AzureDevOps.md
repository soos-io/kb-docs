# How to Integrate SOOS SAST with Azure DevOps

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/azure.png" alt="Azure DevOps" width="128" height="128">
</div>

Set up an Azure DevOps pipeline to scan SARIF 2.1 files with SOOS SAST.

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register) with SAST scanning enabled.
- Node 22 LTS or higher enabled on the pipeline.
- You need to have previously generated Sarif 2.1 json in file(s) that end with the `.sarif.json` extension.
- Add the [**SOOS Security Analysis**](https://marketplace.visualstudio.com/items?itemName=SOOS.SOOS-Security-Analysis) task to your organization in Azure DevOps.

## Steps

### **Get the Example**

* Navigate to the [Azure DevOps SAST integration page on the SOOS App](https://app.soos.io/integrate/sast?id=azure-devops), copy the example, and modify it.

### **Run It**

* Execute the pipeline

---

## Reference
* To see the full list of available parameters go to [SOOS Azure DevOps Scan Parameters](https://github.com/soos-io/soos-azure-devops-task#parameters).
