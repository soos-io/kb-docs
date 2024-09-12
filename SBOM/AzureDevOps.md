# How to Integrate SOOS SBOM with Azure DevOps

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/azure.png" alt="Azure DevOps" width="128" height="128">
</div>

Set up an Azure DevOps pipeline to scan CycloneDX or SPDX SBOMs with SOOS SBOM Analysis.

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register) with SBOM scanning enabled.
- Node 20 LTS or higher enabled on the pipeline.
- Add the [**SOOS Security Analysis**](https://marketplace.visualstudio.com/items?itemName=SOOS.SOOS-Security-Analysis) task to your organization in Azure DevOps.

## Steps

### **Get the Example**

* Navigate to the [Azure DevOps SBOM integration page on the SOOS App](https://app.soos.io/integrate/sbom?id=azure-devops), copy the example, and modify it.

### **Run It**

* Execute the pipeline

---

## Reference
* To see the full list of available parameters go to [SOOS SBOM Scan Parameters](https://github.com/soos-io/soos-sbom#parameters)