# How to Integrate SOOS SCA with Bitbucket
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/bitbucket.png" alt="Bitbucket" width="128" height="128">
</div>
In this article we will make the necessary modifications to a Bitbucket repository to scan the repository with SOOS SCA.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a https://bitbucket.org account and repository setup.
- 
## Steps

### **Repo Setup**
* Create a new `bitbucket-pipelines.yml` file in the root of the repository (`<repo_root>/bitbucket-pipelines.yml`).
* Add the SOOS script from the [Bitbucket Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=bitbucket) to your repository's `bitbucket-pipelines.yml` file or from the Starter Script below.
* Copy & paste the API key and Client ID values from the [Bitbucket Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=bitbucket). While editing the pipeline file, select "Add Variables" and add two Secured variables, "SOOS_CLIENT_ID" and "SOOS_API_KEY".

#### Starter Script ####
```
image: python:3.8

pipelines:
  custom:
    soos-sca:
    - variables:
      - name: SOOS_ON_FAILURE
        default: "continue_on_failure"
        allowed-values:
        - "continue_on_failure"
        - "fail_the_build"
      - name: SOOS_DIRS_TO_EXCLUDE
        default: "soos"
      - name: SOOS_FILES_TO_EXCLUDE
        default: ""
      - name: SOOS_ANALYSIS_RESULT_MAX_WAIT
        default: "300"
      - name: SOOS_ANALYSIS_RESULT_POLLING_INTERVAL
        default: "10"
      - name: SOOS_LOGGING_VERBOSITY
        default: "INFO"
        allowed-values:
        - "INFO"
        - "DEBUG"
      - name: SOOS_SCAN_MODE
        default: "run_and_wait"
        allowed-values:
        - "run_and_wait"
        - "async_init"
        - "async_result"
    - parallel:
      - step:
          name: 'SOOS SCA'
          script:
            - WORKING_DIR=$BITBUCKET_CLONE_DIR/soos/workspace/
            - mkdir -v -p "$WORKING_DIR"
            - python -m venv ./
            - source bin/activate
            - python -m pip install soos-sca --trusted-host pypi.python.org
            - soos-sca
              -m=$SOOS_SCAN_MODE
              -of=$SOOS_ON_FAILURE
              -cid=$SOOS_CLIENT_ID
              -akey=$SOOS_API_KEY
              -dte=$SOOS_DIRS_TO_EXCLUDE
              -fte=$SOOS_FILES_TO_EXCLUDE
              -wd=$WORKING_DIR
              -armw=$SOOS_ANALYSIS_RESULT_MAX_WAIT
              -arpi=$SOOS_ANALYSIS_RESULT_POLLING_INTERVAL
              -scp=$BITBUCKET_CLONE_DIR
              -pn=$BITBUCKET_REPO_SLUG
              -ch=$BITBUCKET_COMMIT
              -bn=$BITBUCKET_BRANCH
              -bruri="http://bitbucket.org/$BITBUCKET_REPO_FULL_NAME"
              -bldver=$BITBUCKET_BUILD_NUMBER
              -v=$SOOS_LOGGING_VERBOSITY
              -intn="BitBucket"
```

## Run It
To run the SOOS CLI against your repository’s code, just execute a build or commit a change.

## Async Mode
To run SOOS SCA in async mode, use the snippet below. Other build steps can then run in parallel.

```
image: python:3.8

pipelines:
  custom:
    soos-sca:
    - variables:
      - name: SOOS_ON_FAILURE
        default: "continue_on_failure"
        allowed-values:
        - "continue_on_failure"
        - "fail_the_build"
      - name: SOOS_DIRS_TO_EXCLUDE
        default: "soos"
      - name: SOOS_FILES_TO_EXCLUDE
        default: ""
      - name: SOOS_ANALYSIS_RESULT_MAX_WAIT
        default: "300"
      - name: SOOS_ANALYSIS_RESULT_POLLING_INTERVAL
        default: "10"
    - parallel:
      - step:
          name: 'SOOS SCA'
          script:
            - WORKING_DIR=$BITBUCKET_CLONE_DIR/soos/workspace/
            - mkdir -v -p "$WORKING_DIR"
            - python -m venv ./
            - source bin/activate
            - python -m pip install soos-sca --trusted-host pypi.python.org
            - soos-sca
              -m="async_init"
              -of=$SOOS_ON_FAILURE
              -cid=$SOOS_CLIENT_ID
              -akey=$SOOS_API_KEY
              -pn=$BITBUCKET_REPO_SLUG
              -dte=$SOOS_DIRS_TO_EXCLUDE
              -fte=$SOOS_FILES_TO_EXCLUDE
              -wd=$BITBUCKET_CLONE_DIR
              -scp=$BITBUCKET_CLONE_DIR
              -armw=$SOOS_ANALYSIS_RESULT_MAX_WAIT
              -arpi=$SOOS_ANALYSIS_RESULT_POLLING_INTERVAL
              -ch=$BITBUCKET_COMMIT
              -bn=$BITBUCKET_BRANCH
              -bldver=$BITBUCKET_BUILD_NUMBER
              -bruri="https://bitbucket.org/$BITBUCKET_REPO_FULL_NAME"
              -v=$SOOS_LOGGING_VERBOSITY
              -intn="BitBucket"
          after-script:
            - WORKING_DIR=$BITBUCKET_CLONE_DIR/soos/workspace/
            - mkdir -v -p "$WORKING_DIR"
            - python -m venv ./
            - source bin/activate
            - python -m pip install soos-sca --trusted-host pypi.python.org
            - soos-sca
              -m="async_result"
              -of=$SOOS_ON_FAILURE
              -cid=$SOOS_CLIENT_ID
              -akey=$SOOS_API_KEY
              -pn=$BITBUCKET_REPO_SLUG
              -wd=$BITBUCKET_CLONE_DIR
              -scp=$BITBUCKET_CLONE_DIR
              -armw=$SOOS_ANALYSIS_RESULT_MAX_WAIT
              -arpi=$SOOS_ANALYSIS_RESULT_POLLING_INTERVAL
              -v=$SOOS_LOGGING_VERBOSITY
```
