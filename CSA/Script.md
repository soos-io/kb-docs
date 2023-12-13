# How to Integrate SOOS Container Security Analysis (CSA) with your OS Shell


<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/shell.png" alt="SOOS" width="128" height="128">
</div>

Currently you have the option to integrate the SOOS Container Security Analysis (CSA) product using the command line from your OS.


## Prerequisites
- You need to have a [SOOS account.](https://app.soos.io/register)
- Have [docker installed.](https://docs.docker.com/get-docker/) in the Host machine.

## Steps
### **Getting the script**
* Navigate to the [Script CSA integration page on the SOOS App](https://app.soos.io/integrate/containers?id=script) and copy the script content according to the OS you're running.

### **Environment Setup**
* In all of our example scripts we are using environment variables, we strongly recommend that you also set up your SOOS_CLIENT_ID and SOOS_API_KEY as environment variables too.

### Script Parameters

| Argument | Default | Description |
| --- | --- | --- |
| `--apiKey` | None | SOOS API Key - get yours from https://app.soos.io/integrate/containers |
| `--apiURL` | https://api.soos.io/api/ | SOOS API URL - Intended for internal use only, do not modify. |
| `--appVersion` | None | App Version - Intended for internal use only. |
| `--branchName` | null | The name of the branch from the SCM System. |
| `--branchURI` | null | The URI to the branch from the SCM System. |
| `--buildURI` | null | URI to CI build info. |
| `--buildVersion` | null | Version of application build artifacts. |
| `--clientId` | None | SOOS Client ID - get yours from https://app.soos.io/integrate/containers |
| `--commitHash` | null | The commit hash value from the SCM System. |
| `--integrationName` | null | Integration Name - Intended for internal use only. |
| `--integrationType` | null | Integration Type - Intended for internal use only. |
| `--logLevel` | INFO | Minimum level to show logs: PASS, IGNORE, INFO, WARN, or FAIL. |
| `--onFailure` | continue_on_failure | Action to perform when the scan fails. Options: fail_the_build, continue_on_failure. |
| `--operatingEnvironment` | null | Set Operating environment for information purposes only. |
| `--otherOptions` | None | Other Options to pass to syft. |
| `--projectName` | None | Project Name - this is what will be displayed in the SOOS app. |
| `--scriptVersion` | null | N/A |
| `--verbose` | false | Enable verbose logging. |
| `targetToScan` | N/A | The target to scan. Should be a docker image name or a path to a directory containing a Dockerfile. |


## Scanning Private Images with Authentication
To scan an image from a private registry, follow these steps:

1. Authenticate the Host: Ensure that the Docker daemon on your host machine is authenticated with the private registry.

2. Run the Scan: Use the following command, passing the Docker socket into the container:
```
docker run -it \
  -v /var/run/docker.sock:/var/run/docker.sock \
  soosio/csa \
  --clientId=<YOUR_CLIENT_ID> \
  --apiKey=<YOUR_API_KEY> \
  --projectName="<YOUR_PROJECT_NAME>" \
  <CONTAINER_NAME>:<TAG_NAME>
```
## Retrieving the Results File for Troubleshooting
If you need to retrieve the results file for troubleshooting or any other reason, you can do so by mounting a volume. This binds the container's results directory to a directory on your host machine.
In the following example, c:/results is the local folder on the host where the results will be stored:
```
docker run -it \
  -v c:/results:/usr/src/app/results \
  soosio/csa \
  --clientId=<YOUR_CLIENT_ID> \
  --apiKey=<YOUR_API_KEY> \
  --projectName="<YOUR_PROJECT_NAME>" \
  <CONTAINER_NAME>:<TAG_NAME>
```

Note: The path c:/results is specific to Windows. If you're using Linux or macOS, adjust the path format accordingly.

## Reference
* To see the full list of available parameters go to [CSA repository parameters description](https://github.com/soos-io/soos-csa#parameters)
