# How to Integrate SOOS DAST with your AWS Codebuild CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/codebuild.png" alt="GitLab" width="128" height="128">
</div>
Set up an AWS CodeBuild project and scan an endpoint with SOOS DAST.

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register) with DAST scanning enabled.

## Steps

### **Get the Example**

* Navigate to the [AWS CodeBuild DAST integration page on the SOOS App](https://app.soos.io/integrate/dast?id=aws-codebuild), copy the example, and modify it.

### **Configure CodeBuild**

* Navigate to your project and select "Environment" from the "Edit" menu.

#### **Setup environment variables**

* In the main section complete the following inputs:
  - Environment Image: Custom Image
  - Environment Type: Linux
  - Image registry: Amazon ECR
  - Amazon ECR repository URI: `public.ecr.aws/soosio/soos-dast:latest`
  - Amazon ECR image tag: latest
  - Role Name: Use existing or create new role name

#### **Set Build Commands**

* Return to the "Edit" menu and select "BuildSpec" > "Insert build commands".  Add the script provided in the AWS integration page in the SOOS App. Edit the individual arguments (e.g. the scan mode and the target URL) as needed, and then click the "Update BuildSpec" button.

### **Run It**

* Execute the pipeline

---

## Reference
* To see the full list of available parameters go to [SOOS DAST Scan Parameters](https://github.com/soos-io/soos-dast#parameters)