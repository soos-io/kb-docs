# How to Integrate SOOS SBOM with your repo using the Script
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/shell.png" alt="SOOS" width="128" height="128">
</div>
SOOS provides direct CI/CD integration to the 10 most common platforms.  So what do you do if you don't see yours listed?
SOOS provides a Script for you to customize as-needed.

## Prerequisites
- You need to have a [SOOS account.](https://app.soos.io/register)
- Node 18.12.0 or higher - https://nodejs.org/en/download

## Steps

### **Repo Setup**
* Download the corresponded Script for your environment from the [SBOM Script integration page on SOOS](https://app.soos.io/integrate/sbom?id=script).
* Create a new file in your repo root directory called `run-sbom-analysis.sh` or `run-sbom-analysis.bat` and paste the content of the script in the new file.
* Open the terminal or command prompt and go to your project directory. Run the command that corresponds to the selected operating system:
    * run-sbom-analysis.sh (Linux or MacOs)
    * run-sbom-analysis.bat (Windows)
Click the GitHub link to visit the [soos-sbom](https://github.com/soos-io/soos-sbom) repository for additional documentation.

### **Configure your environment**
Our CLI is highly configurable, as it is designed to run in a number of scenarios. The only required parameters are the Environment Variables (API Key and Client ID) and the project name.

**Environment variables**

* Create the SOOS_API_KEY an SOOS_CLIENT_ID environment variables.
* Copy & paste the API key and Client ID values from the [Script Integration page of the SOOS app](https://app.soos.io/integrate/sbom?id=script).  These will serve as environment variables to be used by the SOOS CLI.

**Project Name**

The project name is up to the user's discretion. Typically this aligns with a repository name, module name, or project/solution.

It should always point to the same codebase between scans, otherwise the difference in manifests/packages will become apparent in the application.

**Branch and Build Info**

If you are running scans against different branches, it is important to include as much branch information as possible using the built in branch/build parameters. This will ensure that you can easily track and compare scans between different branches for the same project.

**Support for Maintenance Mode**
The Script will respond to SOOS API maintenance in two ways depending on how you have the 'on failure' parameter set.

* If set to fail the build (-of="fail_the_build"), the script will exit with a non-successful exit code and fail the build during maintenance mode.
    * Otherwise a warning will be displayed and the scan will be skipped and appear to be successful in your CI/CD system.

## Reference
* To see the full list of available parameters go to [Script repository parameters description](https://github.com/soos-io/soos-sbom?tab=readme-ov-file#parameters)