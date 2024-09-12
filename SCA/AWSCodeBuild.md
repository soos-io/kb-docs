# How to Integrate SOOS SCA with your AWS Codebuild CI

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/codebuild.png" alt="codebuild" width="128" height="128">
</div>

Set up AWS CodeBuild to scan your manifests with SOOS SCA Core.

## Prerequisites

- You need to have a [SOOS account](https://app.soos.io/register).
- Node 20 LTS or higher enabled on the pipeline.

## Steps

### **Configure Codebuild base image**

When setting up the project make sure to use the **aws/codebuild/standard:5.0** image in order for it to work

### **Get the Example**

* Navigate to the [AWS CodeBuild SCA Core integration page on the SOOS App](https://app.soos.io/integrate/sca?id=aws-codebuild), copy the example, and modify it.

### **Set Build Commands**
* Return to the **"Edit"** menu and select **Buildspec** to insert build commands.

### **Run It**

* Execute the pipeline

---

## Reference
* To see the full list of available parameters go to [SOOS SCA Core Scan Parameters](https://github.com/soos-io/soos-sca#parameters)