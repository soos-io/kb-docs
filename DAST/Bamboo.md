# How to Integrate SOOS DAST with your Bamboo CI

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/bamboo.png" alt="bamboo" width="128" height="128">
</div>
Set up a Bamboo pipeline project and scan an endpoint with SOOS DAST.

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register) with DAST scanning enabled.
- Docker needs to be installed on the agent machine.

## Steps

### **Get the Example**

* Navigate to the [Bamboo DAST integration page on the SOOS App](https://app.soos.io/integrate/dast?id=bamboo), copy the example, and modify it.

### **Running on Windows**

* Create a new script task in your bamboo project

### **Running on Linux or Mac OS**

**Repository Setup**

* Create a new folder in your git repository: `<repo_root>/bamboo-specs`
* Place the bamboo.yml under  `<repo_root>/bamboo-specs/` folder that you created in step # 1 above.
* Commit the new file and the new folder path to your repository.

**Configure Bamboo Build**

* Navigate to Administration > Linked repositories 
* Select your desired repository. In the Bamboo Specs tab enable Scan for Bamboo Specs. 
* In the Specs status tab you can scan for the bamboo.yml file.

### Run It

* Execute the pipeline

---

## Reference
* To see the full list of available parameters go to [SOOS DAST Scan Parameters](https://github.com/soos-io/soos-dast#parameters)