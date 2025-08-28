# How to Integrate SOOS SCA with your CircleCI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/circleci.png" alt="circleci" width="128" height="128">
</div>

Set up CircleCI to scan your manifests with SOOS SCA Core Orb.

## Prerequisites

- You need to have a [SOOS account](https://app.soos.io/register).
- Node 22 LTS or higher enabled on the pipeline.

## Steps

## Repo Setup
* Create a directory called `.circleci` in the root directory of your local GitHub or Bitbucket code repository.
* Create a `config.yml` file inside the `.circleci` directory directory.

### **Get the Example**

* Navigate to the [CircleCI SCA Core integration page on the SOOS App](https://app.soos.io/integrate/sca?id=circleci), copy the example, and modify it.

### **Run It**

* Execute the pipeline

---

## Reference
* To see the full list of available parameters go to [SOOS SCA Core Scan Parameters](https://github.com/soos-io/soos-sca#parameters)