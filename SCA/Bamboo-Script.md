# How to Integrate SOOS SCA with your Bamboo CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/bamboo.png" alt="bamboo" width="128" height="128">
</div>
This document will take you step-by-step through the tasks required to set up a Bamboo project, for scan it with the SOOS SCA Product.
## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a Bamboo project.

## Steps

### **Build Setup in bamboo**
* Navigate to your project’s job-task header and press the **Add task** button. **Make sure this task has a predecessor task to checkout your repo’s source code**.
* Type Script into the search box and choose the Script task option.
* Enter a task description, such as 'Run SOOS Scan'.
* Now, add the SOOS script from the [Bamboo Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=bamboo) under the Script tab.
* Make sure to set the Project Name (which groups scans together) and the Build and Branch parameters. Providing the branch/build parameters allows us to tie together  scans and issues, and provide more meaningful insights and actionability to you. 

### **Setup Environment Variables**
* Create the SOOS_API_KEY and SOOS_CLIENT_ID environment variables, either under the Global Variables or the Plan Variables sections.
* Copy & paste the API key and Client ID values from the [Bamboo Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=bamboo).  These will serve as environment variables to be used by the SOOS CLI.
* Then in the script please note that you should set the variables like this (Example for Windows script)
```
set "SOOS_CLIENT_ID=${bamboo.SOOS_CLIENT_ID}"
set "SOOS_API_KEY=${bamboo.SOOS_API_KEY}"
```

## Run It
To run the SOOS CLI against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.


## Reference
* To see the full list of available parameters go to [Script repository parameters description](https://github.com/soos-io/soos-sca?tab=readme-ov-file#parameters)
