# How to Integrate SOOS Container Security Analysis (CSA) with your OS Shell

<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/shell.png" alt="OS Shell" width="128" height="128">
</div>
Set up a command line task to run SOOS Container Security Analysis (CSA).

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register) with Container scanning enabled.
- Have [Docker installed](https://docs.docker.com/get-docker/) on the host machine.

## Steps
### **Getting the Script**
* Navigate to the [Script Containers integration page on the SOOS App](https://app.soos.io/integrate/containers?id=script), copy the example, and modify it.

### **Run It**
* Execute the command locally or as a script task within your CI/CD pipeline.

## Scanning Private Images with Authentication
To scan an image from a private registry, follow these steps:

1. Authenticate the Host: Ensure that the Docker daemon on your host machine is authenticated with the private registry.

2. Run the Scan: Use the following command, passing the Docker socket into the container:
```
docker run -it \
  -v /var/run/docker.sock:/var/run/docker.sock \
  soosio/csa \
  --clientId=<client_id> \
  --apiKey=<api_key> \
  --projectName="<project_name>" \
  <image>:<tag>
```

## Retrieving the Results File for Troubleshooting
If you need to retrieve the results file for troubleshooting or any other reason, you can do so by mounting a volume. This binds the container's results directory to a directory on your host machine.
In the following example, c:/results is the local folder on the host where the results will be stored:
```
docker run -it \
  -v c:/results:/usr/src/app/results \
  soosio/csa \
  --clientId=<client_id> \
  --apiKey=<api_key> \
  --projectName="<project_name>" \
  <image>:<tag>
```

NOTE: The path c:/results is specific to Windows. If you're using Linux or macOS, adjust the path format accordingly.

---

## Scanning a Locally Built Docker Image

To scan a Docker image that has been built locally and is not hosted on any registry, follow these steps:

1. Give access to Docker Host Context to the `soosio/sca` image. This is done by mounting the Docker socket that the host listens on from the host machine to the container.

2. Run the Scan: Use the following command to scan your locally built image. The crucial part is the `-v` flag used to pass the context of the Docker host machine to the container:
   ```
   docker run -it \
   -v /var/run/docker.sock:/var/run/docker.sock \
   soosio/csa \
   --clientId=<client_id> \
   --apiKey=<api_key> \
   --projectName="<project_name>" \
   <local_image_name>
   ```

Replace `<local_image_name>` with the name of your locally built Docker image. This command enables the scanning tool inside the container to access and scan the specified Docker image stored on your host machine.

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
   -v <local_path_to_image>:/usr/src/app/results:rw \
   soosio/csa \
   --clientId=<client_id> \
   --apiKey=<api_key> \
   --projectName="<project_name>" \
   "docker-archive:results/your_image.tar"
   ```

Replace `<local_path_to_image>` with the directory containing your `.tar` file. This command mounts the `.tar` file inside the container and initiates the scan.

## Scanning a Docker Image Definition Directory

To scan a directory containing Docker image definitions with its other relevant files, use the following method:

1. Specify the Directory: Ensure that the directory you want to scan is accessible on your host machine.

2. Run the Scan: Use the command below, where the last argument specifies the folder containing the files to be scanned:
   ```
   docker run -it \
   -v <local_path_to_image>:/usr/src/app/results:rw \
   soosio/csa \
   --clientId=<client_id> \
   --apiKey=<api_key> \
   --projectName="<project_name>" \
   /usr/src/app/results
   ```

In this command, replace `<local_path_to_image>` with the path to your local directory. The directory is mounted into the container, and the scan is executed on its contents.

---

## Reference
* To see the full list of available parameters go to the [SOOS Container Scan Parameters](https://github.com/soos-io/soos-csa#parameters).
