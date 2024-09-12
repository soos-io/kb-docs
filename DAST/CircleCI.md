# How to Integrate SOOS DAST with your CircleCI CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/circleci.png" alt="CircleCI" width="128" height="128">
</div>
Set up a CircleCI pipeline project and scan an endpoint with SOOS DAST via the SOOS CircleCI DAST Orb.

## Prerequisites

- You need to have a [SOOS account](https://app.soos.io/register) with DAST scanning enabled.

## Steps

### **Repository Setup**
- Create a directory called .circleci in the root directory of your local GitHub or Bitbucket code repository.
- Create a config.yml file inside the .circleci directory with the following lines (if you are using CircleCI server v2.x, use version: 2.0 configuration):

### **Get the Example**

* Navigate to the [CirclCI DAST integration page on the SOOS App](https://app.soos.io/integrate/dast?id=circleci), copy the example, and modify it.

### Run It

* Execute the pipeline

---

## Reference
* To see the full list of available parameters go to [SOOS DAST Scan Parameters](https://github.com/soos-io/soos-dast-circleci-orb)
