# Overview
In this article we will make the necessary modifications to a simple GitHub Actions project to scan a GitHub repository with SOOS.

## Integration Steps
### Get the Client Id and API key
Open the PackageAware App, browse to Integrate > CI/CD/Repo > CI/CD > GitHub Actions
Note the API Key, Client ID and Script values, you will need these below.

### Setup
#### Setup Environment Variables
Navigate to your **Project’s Settings** > **Secrets** menu and in the **Repository secrets** section, create the SOOS_API_KEY and SOOS_CLIENT_ID secrets. These will serve as environment variables to be used by the SOOS CLI.


Create a new file called `main.yml` in 
<repo_root>/.github/workflows/main.yml

Create a new folder in your GitHub repository: <repo_root>/soos/workspace/
Place the requirements.txt and soos.py files <repo_root>/soos/ folders that you created above.
Commit these new files and the new folder path to GitHub.

### Build Setup



#### Build Config
Modify the <repo_root>/.github/workflows/main.yml file, replacing the provided “project_name” variable value with on that is relevant to your project:

#### Run it
