# How to Integrate SOOS DAST with Bitbucket
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/bitbucket.png" alt="Bitbucket" width="128" height="128">
</div>
In this article we will make the necessary modifications to a Bitbucket pipeline to scan a site using SOOS DAST.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a https://bitbucket.org account and repository setup.

## Steps

### **Repo Setup**
* Edit (or create a new) `bitbucket-pipelines.yml` file in the root of the repository (`<repo_root>/bitbucket-pipelines.yml`).
* Add the SOOS script from the [Bitbucket Integration page of the SOOS App](https://app.soos.io/integrate/dast?id=bitbucket) to your repository's `bitbucket-pipelines.yml` file.
* Copy & paste the API key and Client ID values from the [Bitbucket Integration page of the SOOS App](https://app.soos.io/integrate/dast?id=bitbucket). While editing the pipeline file, select "Add Variables" and add two Secured variables, "SOOS_CLIENT_ID" and "SOOS_API_KEY".
* Set a `TARGET_URL` value (the site to scan), by providing a default value in the pipeline file, or by running the pipeline and providing a value in the variables section.
* Configure any additional parameters - see [SOOS DAST Docs](https://github.com/soos-io/soos-dast) for more info.

## Run It
To run SOOS DAST, just run the pipeline.
