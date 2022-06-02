# How to Integrate SOOS SCA with Bitbucket
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/bitbucket.png" alt="Bitbucket" width="128" height="128">
</div>
In this article we will make the necessary modifications to a Bitbucket repository to scan the repository with SOOS SCA.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a https://bitbucket.org account and repository setup.
- Download the latest release of the `soos.py` and `requirements.txt` from [here](https://github.com/soos-io/soos-ci-analysis-python/releases/)

## Steps

### **Repo Setup**
* Create a new folder in your repository: `<repo_root>/soos/workspace/`
* Place the requirements.txt and soos.py files in the `<repo_root>/soos/workspace` folder.
* Commit these 2 new files and the new folder path.
* Create a new `bitbucket-pipelines.yml` file in the root of the repository (`<repo_root>/bitbucket-pipelines.yml`).
* Add the SOOS script from the [Bitbucket Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=bitbucket) to your repository's `bitbucket-pipelines.yml` file.
* Copy & paste the API key and Client ID values from the [Bitbucket Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=bitbucket). While editing the pipeline file, select "Add Variables" and add two Secured variables, "SOOS_CLIENT_ID" and "SOOS_API_KEY".

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
            - echo $WORKING_DIR
            - mkdir -p "$WORKING_DIR"
            - cd "$WORKING_DIR"
            - python -m venv ./
            - source bin/activate
            - pip3 install -r $WORKING_DIR/requirements.txt
            - python "$WORKING_DIR/soos.py"
              -m="async_init"
              -of=$SOOS_ON_FAILURE
              -cid=$SOOS_CLIENT_ID
              -akey=$SOOS_API_KEY
              -dte=$SOOS_DIRS_TO_EXCLUDE
              -fte=$SOOS_FILES_TO_EXCLUDE
              -wd=$BITBUCKET_CLONE_DIR
              -armw=$SOOS_ANALYSIS_RESULT_MAX_WAIT
              -arpi=$SOOS_ANALYSIS_RESULT_POLLING_INTERVAL
              -scp=$BITBUCKET_CLONE_DIR
              -pn=$BITBUCKET_REPO_SLUG
              -ch=$BITBUCKET_COMMIT
              -bn=$BITBUCKET_BRANCH
              -bruri="http://bitbucket.org/$BITBUCKET_REPO_FULL_NAME"
              -bldver=$BITBUCKET_BUILD_NUMBER
          after-script:
            - WORKING_DIR=$BITBUCKET_CLONE_DIR/soos/workspace/
            - echo $WORKING_DIR
            - mkdir -p "$WORKING_DIR"
            - cd "$WORKING_DIR"
            - python -m venv ./
            - source bin/activate
            - pip3 install -r $WORKING_DIR/requirements.txt
            - python "$WORKING_DIR/soos.py"
              -m="async_result"
              -of=$SOOS_ON_FAILURE
              -cid=$SOOS_CLIENT_ID
              -akey=$SOOS_API_KEY
              -wd=$BITBUCKET_CLONE_DIR
              -pn=$BITBUCKET_REPO_SLUG
              -armw=$SOOS_ANALYSIS_RESULT_MAX_WAIT
              -arpi=$SOOS_ANALYSIS_RESULT_POLLING_INTERVAL
              -scp=$BITBUCKET_CLONE_DIR
```
