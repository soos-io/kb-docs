# How to Integrate SOOS DAST with your Azure DevOps CI Pipeline
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/azure.png" alt="Azure" width="128" height="128">
</div>
Set up an Azure DevOps pipeline project and scan an endpoint with SOOS DAST.

## Prerequisites

- You need to have a [SOOS account](https://app.soos.io/register) with DAST scanning enabled.
- Add the [**SOOS Security Analysis**](https://marketplace.visualstudio.com/items?itemName=SOOS.SOOS-Security-Analysis) task to your organization in Azure DevOps.
- The pipeline job must run the task on a Linux build agent.

## Steps

### **Use a Linux Build Agent**

Your pipeline stage must use a Linux build agent:
```
  pool:
    vmImage: 'ubuntu-latest'
```

### **Get the Example**

* Navigate to the [Azure DevOps DAST integration page on the SOOS App](https://app.soos.io/integrate/dast?id=azure-devops), copy the example, and modify it.

### **Run It**

* Execute the pipeline

---

## Reference
* To see the full list of available parameters go to [SOOS DAST Scan Parameters](https://github.com/soos-io/soos-dast#parameters)