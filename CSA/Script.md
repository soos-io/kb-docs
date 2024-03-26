# How to Integrate SOOS Container Security Analysis (CSA) with your OS Shell

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/shell.png" alt="OS Shell" width="128" height="128">
</div>

Currently, you have the option to integrate the SOOS Container Security Analysis (CSA) product using the command line from your operating system.

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register).
- Have [Docker installed](https://docs.docker.com/get-docker/) on the host machine.

## Steps
### **Getting the Script**
* Navigate to the [Script CSA integration page on the SOOS App](https://app.soos.io/integrate/containers?id=script) and copy the script content according to the OS you're running.

### **Environment Setup**
* In all of our example scripts, we are using environment variables. We strongly recommend that you also set up your SOOS_CLIENT_ID and SOOS_API_KEY as environment variables.

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
| `--logLevel` | INFO | Minimum level to show logs: INFO, WARN, FAIL, DEBUG, ERROR. |
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

---

## Scanning a Locally Built Docker Image

To scan a Docker image that has been built locally and is not hosted on any registry, follow these steps:

1. Give access to Docker Host Context to the `soosio/sca` image. This is done by mounting the Docker socket from your host machine into the container.

2. Run the Scan: Use the following command to scan your locally built image. The crucial part is the `-v` flag used to pass the context of the Docker host machine to the container:
   ```
   docker run -it \
   -v /var/run/docker.sock:/var/run/docker.sock \
   soosio/csa \
   --clientId=<YOUR_CLIENT_ID> \
   --apiKey=<YOUR_API_KEY> \
   --projectName="<YOUR_PROJECT_NAME>" \
   <PREVIOUSLY_BUILT_IMAGE>
   ```

Replace `<PREVIOUSLY_BUILT_IMAGE>` with the name of your locally built Docker image. This command enables the scanning tool inside the container to access and scan the specified Docker image stored on your host machine.

---

## Scanning a Docker Image Saved as a .tar File

To scan a Docker image that has been saved as a `.tar` file, follow these steps:

1. Save the Docker Image: Use the `docker save` command to save your image as a `.tar` file. For example, to save the `soosio/csa` image, use:
   ```
   docker save -o soosio_csa.tar soosio/csa
   ```

2. Run the Scan: Execute the following command, ensuring that the volume is mounted to the path where the `.tar` file is located:
   ```
   docker run -it \
   -v <YOUR_LOCAL_DIRECTORY_PATH>:/usr/src/app/results:rw \
   soosio/csa \
   --clientId=<YOUR_CLIENT_ID> \
   --apiKey=<YOUR_API_KEY> \
   --projectName="<YOUR_PROJECT_NAME>" \
   "docker-archive:results/your_image.tar"
   ```

Replace `<YOUR_LOCAL_DIRECTORY_PATH>` with the directory containing your `.tar` file. This command mounts the `.tar` file inside the container and initiates the scan.

## Scanning a Directory

To scan a directory containing Docker images or other relevant files, use the following method:

1. Specify the Directory: Ensure that the directory you want to scan is accessible on your host machine.

2. Run the Scan: Use the command below, where the last argument specifies the folder containing the files to be scanned:
   ```
   docker run -it \
   -v <YOUR_LOCAL_DIRECTORY_PATH>:/usr/src/app/results:rw \
   soosio/csa \
   --clientId=<YOUR_CLIENT_ID> \
   --apiKey=<YOUR_API_KEY> \
   --projectName="<YOUR_PROJECT_NAME>" \
   /usr/src/app/results
   ```

In this command, replace `<YOUR_LOCAL_DIRECTORY_PATH>` with the path to your local directory. The directory is mounted into the container, and the scan is executed on its contents.

---

## Reference
* To see the full list of available parameters go to the [CSA repository parameters description](https://github.com/soos-io/soos-csa#parameters).
