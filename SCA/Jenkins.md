# How to Integrate SOOS SCA with your Jenkins CI using the Jenkinsfile
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/jenkins.png" alt="Jenkins" width="128" height="128">
</div>

Set up Jenkins to scan your manifests with SOOS SCA Core.

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register).
- Node 20 LTS or higher enabled on the pipeline.

## Steps

### Repo Setup
* Create a `Jenkinsfile` file in your root repository.

### **Get the Example**

* Navigate to the [Jenkins SBOM integration page on the SOOS App](https://app.soos.io/integrate/sca?id=jenkins), copy the example, and modify it.

### **Run It**

* Execute the pipeline

---

## Reference
* To see the full list of available parameters go to [SOOS SCA Core Scan Parameters](https://github.com/soos-io/soos-sca#parameters)