# How to Integrate SOOS SAST with Your Repo Using the Script

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/shell.png" alt="Script" width="128" height="128">
</div>
SOOS provides direct CI/CD integration to the 10 most common platforms. So what do you do if you don't see yours listed? SOOS provides a Script for you to customize as needed.

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register) with SAST scanning enabled.
- Node 20 LTS or higher - [Download Node.js](https://nodejs.org/en/download).
- You need to have previously generated Sarif 2.1 json in file(s) that end with the `.sarif.json` extension.

## Steps

### **Repo Setup**
* Download the corresponding Script for your environment from the [SAST Script integration page on SOOS](https://app.soos.io/integrate/sast?id=script).
* Create a new file in your repository root directory called `run-sast-analysis.sh` or `run-sast-analysis.bat` and paste the content of the script in the new file.
* Open the terminal or command prompt and navigate to your project directory. Run the appropriate command:
    * `./run-sast-analysis.sh` (Linux or macOS)
    * `run-sast-analysis.bat` (Windows)
* Click the GitHub link to visit the [soos-sast repository](https://github.com/soos-io/soos-sast) for additional documentation.

### **Configure Your Environment**
Our CLI is highly configurable, designed to run in various scenarios. The only required parameters are the Environment Variables (API Key and Client ID) and the project name.

**Environment Variables**

* Create the `SOOS_API_KEY` and `SOOS_CLIENT_ID` environment variables.
* Copy and paste the API key and Client ID values from the [Script Integration page of the SOOS app](https://app.soos.io/integrate/sast?id=script). These will serve as environment variables for the SOOS CLI.

**Project Name**

The project name is at the user's discretion, typically aligning with a repository name, module name, or project/solution name. It should consistently point to the same codebase between scans to ensure continuity.

**Branch and Build Info**

Include as much branch information as possible when running scans against different branches, using built-in branch/build parameters. This helps track and compare scans between branches for the same project.

**Support for Maintenance Mode**

The Script responds to SOOS API maintenance based on the 'on failure' parameter setting:
* If set to fail the build (`-of="fail_the_build"`), the script will exit with a non-successful code during maintenance mode.
* Otherwise, a warning will be displayed, the scan will be skipped, and it will appear successful in your CI/CD system.

## Reference
* For the full list of available parameters, visit the [Script repository parameters description](https://github.com/soos-io/soos-sast?tab=readme-ov-file#parameters).