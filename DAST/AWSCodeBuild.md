# How to Integrate SOOS DAST with your AWS Codebuild CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/codebuild.png" alt="GitLab" width="128" height="128">
</div>
In this article we will make the necessary modifications to a simple AWS CodeBuild project to scan a WebApp or API with SOOS DAST.

## Prerequisites
- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a AWS Codebuild project created

## Steps

### **Configure CodeBuild**

Navigate to your project and select “Environment” from the “Edit” menu.


<img src="../assets/img/codebuild-edit.png">

### **Setup environment variables**
In the main section complete the following inputs:
- Environment Image: Custom Image
- Environment Type: Linux
- Image registry: Amazon ECR
- Amazon ECR repository URI: public.ecr.aws/y6h0x1r3/soos-dast
- Amazon ECR image tag: latest
- Role Name: Use existing or create new role name

<img src="../assets/img/codebuild-envs.png">

Open “Additional Configuration” to reveal the “Environment Variables” section. Create the **SOOS_API_KEY**  and **SOOS_CLIENT_ID** environment variables and copy/paste the values that you found from the SOOS app; these variables will be used by the SOOS CLI during the scans.

<img src="../assets/img/codebuild-additionalconfig.png">

### **Set Build Commands**
Return to the 'Edit' menu and select BuildSpec > Insert build commands.  Add the script provided in the AWS integration page in the SOOS App. Edit the individual arguments (e.g. the scan mode and the target URL) as needed, and then click the "Update BuildSpec" button.

<details open>
<summary> Example Script </summary>

```
version: 0.2

phases:
  build:
    commands:
      - cd /zap/
      - node dist/index.js --clientId=$SOOS_CLIENT_ID --apiKey=$SOOS_API_KEY --projectName=<YOUR_PROJECT_NAME> --scanMode=baseline --apiURL=https://api.soos.io/api/ <YOUR_TARGET_URL>
```

</details>

<img src="../assets/img/codebuild-buildspec.png">


### **Run It**
To run the SOOS DAST Analysis against your repository’s code, just execute a build, commit a change, or set up custom build triggers. Check your build logs for detailed scan results as well as a report URL that will allow you to view results in the SOOS app. 

## Reference
* To see the full list of available parameters go to [DAST repository parameters description](https://github.com/soos-io/soos-dast#parameters)