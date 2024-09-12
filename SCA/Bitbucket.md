# How to Integrate SOOS SCA with Bitbucket
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/bitbucket.png" alt="Bitbucket" width="128" height="128">
</div>

Set up BitBucket to scan your manifests with SOOS SCA Core.

## Prerequisites

- You need to have a [SOOS account](https://app.soos.io/register).
- Node 20 LTS or higher enabled on the pipeline.

## Steps

### **Repository Setup**
* Create a new `bitbucket-pipelines.yml` file in the root of the repository (`<repo_root>/bitbucket-pipelines.yml`).

### **Get the Example**

* Navigate to the [BitBucket SCA Core integration page on the SOOS App](https://app.soos.io/integrate/sca?id=bitbucket), copy the example, and modify it.

### **Run It**

* Execute the pipeline

---

## Reference
* To see the full list of available parameters go to [SOOS SCA Core Scan Parameters](https://github.com/soos-io/soos-sca#parameters)