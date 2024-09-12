# How to Integrate SOOS SBOM with Your Repo Using the Script

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/shell.png" alt="Script" width="128" height="128">
</div>

Set up a script to scan CycloneDX or SPDX SBOMs with SOOS SBOM.

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register).
- Node 20 LTS or higher - [Download Node.js](https://nodejs.org/en/download).

## Steps

### **Repo Setup**
* Download the corresponding Script for your environment from the [SBOM Script integration page on SOOS](https://app.soos.io/integrate/sbom?id=script).
* Create a new file in your repo root directory called `run-sbom-analysis.sh` or `run-sbom-analysis.bat` and paste the content of the script in the new file.
* Open the terminal or command prompt and navigate to your project directory. Run the appropriate command:
    * `./run-sbom-analysis.sh` (Linux or macOS)
    * `run-sbom-analysis.bat` (Windows)
* Click the GitHub link to visit the [soos-sbom repository](https://github.com/soos-io/soos-sbom) for additional documentation.

### **Configure Your Environment**
Our CLI is highly configurable, designed to run in various scenarios. The only required parameters are the Environment Variables (API Key and Client ID) and the project name.

**Environment Variables**

* Create the `SOOS_API_KEY` and `SOOS_CLIENT_ID` environment variables.
* Copy and paste the API key and Client ID values from the [Script Integration page of the SOOS app](https://app.soos.io/integrate/sbom?id=script). These will be used by the SOOS CLI.

**Project Name**

The project name is at the user's discretion, typically aligning with a repository name, module name, or project/solution name. Consistency is key to ensure continuity between scans.

**Branch and Build Info**

Include comprehensive branch information when running scans against different branches, using built-in branch/build parameters. This helps track and compare scans between branches for the same project.

**Support for Maintenance Mode**

The Script responds to SOOS API maintenance based on the 'on failure' parameter setting:
* If set to fail the build (`-of="fail_the_build"`), the script will exit with a non-successful code during maintenance mode.
* Otherwise, a warning will be displayed, the scan will be skipped, and it will appear successful in your CI/CD system.

## Reference
* For the full list of available parameters, visit the [Script repository parameters description](https://github.com/soos-io/soos-sbom?tab=readme-ov-file#parameters).