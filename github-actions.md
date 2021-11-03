# Overview
In this article we will make the necessary modifications to a simple GitHub Actions project to scan a GitHub repository with SOOS.

## Introduction
You can integrate the SOOS CI Analysis in your GitHub Workflow using the `soos-io/soos-ci-analysis-github-actions`acton

The SOOS Action has properties which are passed to the action using `with`.

| Property | Default | Description |
| --- | --- | --- |
| base_uri | "https://api.soos.io/api/"  | The API BASE URI provided to you when subscribing to SOOS services. |
| project_name | [none]  | REQUIRED. A custom project name that will present itself as a collection of test results within your soos.io dashboard. |
| directories_to_exclude | ""  | List (comma separated) of directories (relative to ./) to exclude from the search for manifest files. Example - Correct: bin/start/ ... Example - Incorrect: ./bin/start/ ... Example - Incorrect: /bin/start/'|
| files_to_exclude | "" | List (comma separated) of files (relative to ./) to exclude from the search for manifest files. Example - Correct: bin/start/manifest.txt ... Example - Incorrect: ./bin/start/manifest.txt ... Example - Incorrect: /bin/start/manifest.txt' |
| analysis_result_max_wait | 300 | Maximum seconds to wait for Analysis Result before exiting with error. |
| analysis_result_polling_interval | 10 | Polling interval (in seconds) for analysis result completion (success/failure.). Min 10. |
| debug_print_variables | false | Enables printing of input/environment variables within the docker container. |
| commit_hash | [none] | The commit hash value from the SCM System |
| branch_name | [none] | The name of the branch from the SCM System |
| branch_uri | [none] | The URI to the branch from the SCM System |
| build_version | [none] | Version of application build artifacts |
| build_uri | [none] | URI to CI build info |
| operating_environment | [none] | System info regarding operating system, etc. |

The SOOS Action has environment variables which are passed to the action using `env`. These environment variables are stored as repo `secrets` and are required for the action to operate.

| Property | Description |
| --- | --- |
| SOOS_CLIENT_ID | Provided to you when subscribing to SOOS services. |
| SOOS_API_KEY | Provided to you when subscribing to SOOS services. |


## Usage
### Get the Client Id and API key
Open the PackageAware App, browse to Integrate > CI/CD/Repo > CI/CD > GitHub Actions
Note the API Key, Client ID and Script values, you will need these below.

### Setup Environment Variables
Navigate to your **Project’s Settings** > **Secrets** menu and in the **Repository secrets** section, create the `SOOS_API_KEY` and `SOOS_CLIENT_ID` secrets.

![GitHub Settings](/images/github-settings.png)

### Add the GitHub workflow to your repo

Create or Update `main.yml` in folder `<repo_root>/.github/workflows/main.yml`. An example content is:
``` yaml
name: "....."
# Events required to engage workflow (add/edit this list as needed)
on: push
jobs:
  ...
  soos-ci-analysis:
    name: SOOS CI Scan
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: Run SOOS - Scan for vulnerabilities
      uses: soos-io/soos-ci-analysis-github-actions@main
      with:
        project_name: "My Project Name"
      env:
        # Visit https://soos.io to get the required tokens to leverage SOOS scanning/analysis services
        SOOS_CLIENT_ID: ${{ secrets.SOOS_CLIENT_ID }}
        SOOS_API_KEY: ${{ secrets.SOOS_API_KEY }}
  ...
```


#### Run it
