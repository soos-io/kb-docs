# How to Integrate SOOS SCA with your Travis CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/travis-ci.png" alt="Travis" width="128" height="128">
</div>
In this article we will make the necessary modifications to a simple TravisCI project to scan a GitHub or Bitbucket repository with the SOOS SCA PRODUCT.

## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- You need to have a Travis repo.

## Steps

### **Configure Travis CI**
**Setup Environment Variables**

* Within TravisCI, navigate to Settings.
* Create the SOOS_API_KEY and SOOS_CLIENT_ID environment variables in the Environment Variables section
* Copy & paste the API key and Client ID values from the [Travis CI Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=travis-ci).  These will serve as environment variables to be used by the SOOS CLI.
* Add the SOOS script from the [Travis CI Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=travis-ci) to your repository's /.travis.yml file.

## Run It
To run the SOOS CLI against your repository’s code, just execute a build or commit a change. The build will use the environment variables that you created for the API Key and Client ID.


