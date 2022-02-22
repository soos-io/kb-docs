
# How to Integrate SOOS DAST with your Jenkins CI

<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/jenkins.png" alt="Jenkins" width="128" height="128">

Currently, you can integrate the SOOS DAST Analysis with Jenkins using the SOOS DAST Analysis Docker Image in your Jenkinsfile
## Prerequisites

- You need to have a SOOS account.
- Have docker installed in Jenkins Host machine.
## Steps

### **Setup your Repo**

Chose the configuration according to your OS and create a `Jenkinsfile` in the root of your repository.
<details open>
<summary> Windows </summary>

```
pipeline {
    agent any

    environment {
        SOOS_PROJECT_NAME = "DAST Jenkins Example"
        SOOS_SCAN_MODE = "baseline"
        SOOS_API_BASE_URL= "https://api.soos.io/api/"
        SOOS_TARGET_URL= "https://example.com/"
        SOOS_DEBUG= "false"
        SOOS_AJAX_SPIDER= "false"
       
    }

    stages {
        stage('SOOS DAST Baseline Analysis') {
            steps {
                bat '''
                    SET PARAMS=--clientId=%SOOS_CLIENT_ID% --apiKey=%SOOS_API_KEY% --projectName="%SOOS_PROJECT_NAME%" --scanMode=%SOOS_SCAN_MODE% --apiURL=%SOOS_API_BASE_URL%
                    
                    if "%SOOS_DEBUG%" == "true" ( 
                        SET PARAMS=%PARAMS% --debug
                    )
                    if "%SOOS_AJAX_SPIDER%" == "true" (
                        SET PARAMS=%PARAMS% --ajaxSpider
                    )
                    if defined %SOOS_RULES% SET PARAMS=%PARAMS% --rules=\"%SOOS_RULES%\"

                    if defined %SOOS_CONTEXT_FILE% SET PARAMS=%PARAMS% --contextFile=%SOOS_CONTEXT_FILE%

                    if defined %SOOS_CONTEXT_USER% SET PARAMS=%PARAMS% --contextUser=%SOOS_CONTEXT_USER%

                    if defined %SOOS_FULL_SCAN_MINUTES% SET PARAMS=%PARAMS% --fullScanMinutes=%SOOS_FULL_SCAN_MINUTES%

                    if defined %SOOS_API_SCAN_FORMAT% SET PARAMS=%PARAMS% --apiScanFormat=%SOOS_API_SCAN_FORMAT%

                    if defined %SOOS_LEVEL% SET PARAMS=%PARAMS% --level=%SOOS_LEVEL%
                                        
                    docker run --rm soosio/dast %SOOS_TARGET_URL% %PARAMS%
                '''
            }
        }
    }
}
```
</details>

<details open>
<summary>Linux/MacOS</summary>

```
pipeline {
    agent any

    environment {
        SOOS_PROJECT_NAME = "Jenkins SOOS DAST Analysis"
        SOOS_SCAN_MODE = "baseline"
        SOOS_API_BASE_URL= "https://api.soos.io/api/"
        SOOS_TARGET_URL= "https://example.com/"
    }

    stages {
        stage('SOOS DAST Analysis') {
            steps {
                sh '''
                    PARAMS="--clientId=${SOOS_CLIENT_ID} --apiKey=${SOOS_API_KEY} --projectName=${SOOS_PROJECT_NAME} --scanMode=${SOOS_SCAN_MODE} --apiURL=${SOOS_API_BASE_URL}"
                    
                    if ( ${SOOS_DEBUG} == "true" ) then
                        PARAMS="${PARAMS} --debug=true"
                    fi
                    if ( ${SOOS_AJAX_SPIDER} == "true" ) then
                        PARAMS="${PARAMS} --ajaxSpider=true"
                    fi
                    if [ ! -z ${SOOS_RULES} ]; then
                        PARAMS="${PARAMS} --rules=\"$SOOS_RULES\""
                    fi
                    if [ ! -z ${SOOS_CONTEXT_FILE} ]; then
                        PARAMS="${PARAMS} --contextFile=${SOOS_CONTEXT_FILE}"
                    fi
                    if [ ! -z ${SOOS_CONTEXT_USER} ]; then
                        PARAMS="${PARAMS} --contextUser=${SOOS_CONTEXT_USER}"
                    fi
                    if [ ! -z ${SOOS_FULL_SCAN_MINUTES} ]; then
                        PARAMS="${PARAMS} --fullScanMinutes=${SOOS_FULL_SCAN_MINUTES}"
                    fi
                    if [ ! -z ${SOOS_API_SCAN_FORMAT} ]; then
                        PARAMS="${PARAMS} --apiScanFormat=${SOOS_API_SCAN_FORMAT}"
                    fi
                    if [ ! -z ${SOOS_LEVEL} ]; then
                        PARAMS="${PARAMS} --level=${SOOS_LEVEL}"
                    fi
   
                    docker run --rm soosio/dast ${SOOS_TARGET_URL} $PARAMS 
                '''
            }
        }
    }
}
```
</details>

### **Enviroment Setup**

Navigate to Jenkins -> Manage Jenkins -> Configure System

<img src="../assets/img/jenkins-setup.png">

Under "Global Properties", select the Environment variables checkbox and add SOOS_CLIENT_ID and SOOS_API_KEY to the List of Variables.  Populate the Value field for each with the appropriate information obtained from the SOOS app. These will be used by the SOOS CLI. 

<img src="../assets/img/jenkins-envs.png">

### **Setting up Jenkins Project**

Add "New Item" in the Jenkins main menu.

<img src="../assets/img/jenkins-new-item.png">

Enter a name for the item and select Pipeline

<img src="../assets/img/jenkins-project-config.png">


On the next screen:
1. Type in a description for your item.
2. Select the Pipeline Tab.
3. Choose 'Pipeline script from SCM' in the Definition field.
4. In the SCM field select 'Git'.
5. Enter the link to your GitHub repo in the Repository URL field and enter the corresponding credentials when prompted.
6. Write the Jenkinsfile path in the Script Path field and select Apply and Save.

### **Run It**
To run SOOS DAST against your webapp, just execute a build. The build will use the environment variables that you created for the API Key and Client ID.
