# How to integrate SOOS SCA with Azure DevOps
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/azure.png" alt="azure" width="128" height="128">
</div>

Set up an Azure Devops pipeline to scan your manifests with SOOS SCA Core.

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register).
- Node 22 LTS or higher enabled on the pipeline.
- Add the [**SOOS Security Analysis**](https://marketplace.visualstudio.com/items?itemName=SOOS.SOOS-Security-Analysis) task to your organization in Azure DevOps.

## Steps

### **Get the Example**

* Navigate to the [Azure DevOps SCA Core integration page on the SOOS App](https://app.soos.io/integrate/sca?id=azure-devops), copy the example, and modify it.

### **Run It**

* Execute the pipeline

---

## Reference
* To see the full list of available parameters go to [SOOS Azure DevOps Scan Parameters](https://github.com/soos-io/soos-azure-devops-task#)
