# How to Integrate SOOS SCA with your repo using Python

<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/python.png" alt="GitLab" width="128" height="128">

SOOS provides direct CI/CD integration to the 10 most common platforms.  So what do you do if you don't see yours listed?
SOOS provides a Python-based script for you to customize as-needed.

## Prerequisites
- You need to have a SOOS account.
- Python 3.7 or higher - https://www.python.org/downloads/  
- Pip - https://pip.pypa.io/en/stable/installation/ 

## Steps

### **Repo Setup**
* Download the corresponded Script for your environment from the [SCA Script integration page on SOOS](https://app.soos.io/integrate/sca?id=script).
* Create a new file in your repo root directory called `run-sca-analysis.sh` or `run-sca-analysis.bat` and paste the content of the script in the new file.
* Open the terminal or command prompt and go to your project directory. Run the command that corresponds to the selected operating system:
    * run-sca-analysis.sh (Linux or MacOs)
    * run-sca-analysis.bat (Windows)
Click the GitHub link to visit the [soos-ci-analysis-python](https://github.com/soos-io/soos-ci-analysis-python) repository for additional documentation.

### **Configure your environment**
Our CLI is highly configurable, as it is designed to run in a number of scenarios. The only required parameters are the Environment Variables (API Key and Client ID) and the project name.

**Environment variables**

* Create the SOOS_API_KEY an SOOS_CLIENT_ID environment variables.
* Copy & paste the API key and Client ID values from the [Script Integration page of the SOOS app](https://app.soos.io/integrate/sca?id=script).  These will serve as environment variables to be used by the SOOS CLI.

**Project Name**

The project name is up to the user's discretion.  Typically this aligns with a repository name, module name, or project/solution.

It should always point to the same codebase between scans, otherwise the difference in manifests/packages will become apparent in the application.

**Mode**

The integration allows you to select the mode of running the script. Currently the integration supports three modes:

* run_and_wait: to run the analysis synchronously. The results of the analysis will be displayed in the report url that you can copy from the script’s logs. This is the default option.
* async_init: to start async scanning, add other tasks. The script’s log will display the report status url.
* async_result: to wait for the scan to complete. The script’s log will display the report url.

**Branch and Build Info**

If you are running scans against different branches, it is important to include as much branch information as possible using the built in branch/build parameters. This will ensure that you can easily track and compare scans between different branches for the same project.

**Support for Maintenance Mode**
The Python script will respond to SOOS API maintenance in two ways depending on how you have the 'on failure' parameter set.

* If set to fail the build (-of="fail_the_build"), the script will exit with a non-successful exit code and fail the build during maintenance mode.
    * Otherwise a warning will be displayed and the scan will be skipped and appear to be successful in your CI/CD system.