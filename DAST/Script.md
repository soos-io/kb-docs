# How to Integrate SOOS DAST with your OS Shell


<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/shell.png" alt="SOOS" width="128" height="128">
</div>

Currently you have the option to integrate the SOOS DAST product using the command line from your OS.


## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- Have [docker installed.](https://docs.docker.com/get-docker/) in the Host machine.
- Not using an M1/M2 Mac as host machine. (SOOS Dast docker image is incompatible with this platform)
## Steps
### **Getting the script**
* Navigate to the [Script DAST integration page on the SOOS App](https://app.soos.io/integrate/dast?id=script) and copy the script content according to the OS you're running.

### **Environment Setup**
* In all of our example scripts we are using environment variables, we strongly recommend that you also set up your SOOS_CLIENT_ID and SOOS_API_KEY as environment variables too.


### **Script Arguments**

| Name        | Required | Description                                                   |
|-------------|----------|---------------------------------------------------------------|
| `targetURL` | Yes      | target URL including the protocol, eg https://www.example.com |

### **Script Parameters**

| Argument | Default | Description |
| --- | --- | --- |
| -h, --help | ==SUPPRESS== | show this help message and exit |
| -hf, --helpFormatted | False | Print the --help command in markdown table format |
| --configFile | None | SOOS yaml file with all the configuration for the DAST Analysis (See https://github.com/soos-io/soos-dast#config-file-definition) |
| --clientId | None | SOOS Client ID get yours from https://app.soos.io/integrate/sca |
| --apiKey | None | SOOS API Key get yours from https://app.soos.io/integrate/sca |
| --projectName | None | Project name (this will be the one used inside of the SOOS App) |
| --scanMode | baseline | SOOS DAST scan mode. Values available: baseline, fullscan, and apiscan (for more information about scan modes visit https://github.com/soos-io/soos-dast#scan-modes) |
| --apiURL | https://api.soos.io/api/ | SOOS API URL, internal use only, do not modify. |
| --debug | False | Show debug messages |
| --ajaxSpider | None | Use the Ajax spider in addition to the traditional one (About AjaxSpider https://www.zaproxy.org/docs/desktop/addons/ajax-spider/) |
| --rules | None | Rules file to use to INFO, IGNORE or FAIL warnings |
| --contextFile | None | Context file which will be loaded prior to scanning the target |
| --contextUser | None | Username to use for authenticated scans - must be defined in the given context file |
| --fullScanMinutes | None | The number of minutes for spider to run |
| --apiScanFormat | None | Target API format: OpenAPI, SOAP, or GraphQL |
| --level | None | minimum level to show: PASS, IGNORE, INFO, WARN or FAIL |
| --integrationName | None | Integration Name. Intended for internal use only. |
| --integrationType | None | Integration Type. Intended for internal use only. |
| --scriptVersion | None | Script Version. Intended for internal use only. |
| --appVersion | None | App Version. Intended for internal use only. |
| --authDisplay | None | Minimum level to show: PASS, IGNORE, INFO, WARN or FAIL |
| --authUsername | None | Username to use in auth apps |
| --authPassword | None | Password to use in auth apps |
| --authLoginURL | None | Login url to use in auth apps |
| --authUsernameField | None | Username input id to use in auth apps |
| --authPasswordField | None | Password input id to use in auth apps |
| --authSubmitField | None | Submit button id to use in auth apps |
| --authFirstSubmitField | None | First submit button id to use in auth apps |
| --authSubmitAction | None | Submit action to perform on form filled, click or submit |
| --zapOptions | None | ZAP Additional Options |
| --requestCookies | None | Set Cookie values for the requests to the target URL |
| --requestHeaders | None | Set extra Header requests |
| --commitHash | None | The commit hash value from the SCM System |
| --branchName | None | The name of the branch from the SCM System |
| --branchURI | None | The URI to the branch from the SCM System |
| --buildVersion | None | Version of application build artifacts |
| --buildURI | None | URI to CI build info |
| --operatingEnvironment | None | Set Operating environment for information porpuses only |
| --reportRequestHeaders | False | Include request/response headers data in report |
| --outputFormat | None | Output format for vulnerabilities: only the value sarif is available at the moment |
| --gpat | None | GitHub Personal Authorization Token |
| --bearerToken | None | Bearer token to authenticate |
| --checkoutDir | None | Checkout Dir to locate sarif report |
| --sarifDestination | None | Sarif destination to upload report in the form of <repoowner>/<reponame> |
| --sarif | None | DEPRECATED sarif parameter is currently deprecated, for same functionality as before please use --outPutFormat='sarif' |
| --oauthTokenUrl | None | The fully qualified authentication URL that grants the access_token. |
| --oauthParameters | None | Parameters to be added to the oauth token request. (eg --oauthParameters="client_id:clientID, client_secret:clientSecret, grant_type:client_credentials") |


#### **Config File Definition**
``` yaml
config:
  clientId: 'SOOS_CLIENT_ID' # Required - SOOS Client Id provided by the Application
  apiKey: 'SOOS_API_KEY' # Required - SOOS API Key provided by the Application
  projectName: 'Project Name' # Required
  scanMode: 'baseline' # Required - DAST Scan mode. Values available: baseline, fullscan, and apiscan
  apiURL: 'https://app.soos.io/api/' # Required - The SOOS API URL
  debug: true # Optional - Enable console log debugging. Default: false 
  ajaxSpider: false # Optional - Enable Ajax Spider scanning - Useful for Modern Web Apps
  rules: '' # Optional - 
  context:
    file: '' # Optional
    user: '' # Optional
  fullScan:
    minutes: ''
  apiScan:
    format: 'openapi'
  level: 'PASS'
```

### **Scan Modes**

**Baseline**

It runs the [ZAP](https://www.zaproxy.org/) spider against the specified target for (by default) 1 minute and then waits for the passive scanning to complete before reporting the results.

This means that the script doesn't perform any actual ‘attacks’ and will run for a relatively short period of time (a few minutes at most).

By default, it reports all alerts as WARNings but you can specify a config file which can change any rules to `FAIL` or `IGNORE`.

This mode is intended to be ideal to run in a `CI/CD` environment, even against production sites.

**Full Scan**

It runs the [ZAP](https://www.zaproxy.org/) spider against the specified target (by default with no time limit) followed by an optional ajax spider scan and then a full `Active Scan` before reporting the results.

This means that the script does perform actual ‘attacks’ and can potentially run for a long period of time. You should NOT use it on web applications that you do not own. `Active Scan` can only find certain types of vulnerabilities. Logical vulnerabilities, such as broken access control, will not be found by any active or automated vulnerability scanning. Manual penetration testing should always be performed in addition to active scanning to find all types of vulnerabilities.

By default, it reports all alerts as WARNings but you can specify a config file which can change any rules to FAIL or IGNORE. The configuration works in a very similar way as the [Baseline Mode](#baseline)

**API Scan**

It is tuned for performing scans against APIs defined by `OpenAPI`, `SOAP`, or `GraphQL` via either a local file or a URL.

It imports the definition that you specify and then runs an `Active Scan` against the URLs found. The `Active Scan` is tuned to APIs, so it doesn't bother looking for things like `XSSs`.

It also includes 2 scripts that:
- Raise alerts for any HTTP Server Error response codes
- Raise alerts for any URLs that return content types that are not usually associated with APIs

Example snippet of API scan feeding spec file locally:

``` bash
docker run -v <path_to_folder_containing_file>:/zap/wrk/:rw -it soosio/dast <name_of_file>  --scanMode="apiscan" --apiScanFormat="openapi"  --clientId="YOUR_CLIENT_ID" --apiKey="YOUR_API_KEY" --projectName="YOUR_PROJECT_NAME" --apiURL="https://api.soos.io/api/"
```

### Running the script

To run the script just execute the file created with the sample script obtained from [getting the script](#getting-the-script), just make sure that you have replaced the project name and set up the environment variables properly.

## Authenticated scans

### Using bearer token

If you need to run a scan against url that needs authorization and the only thing needed is to set an authorization header in the form of `authorization: Bearer token-value` then this is the most straight forward workflow (note that for this method you should have the bearer token value beforehand).

Example snippet:

``` bash
docker run -it soosio/dast:latest "https://example.com/protectedendnpoint" --clientId="YOUR_CLIENT_ID" --apiKey="YOUR_API_KEY" --projectName="YOUR_PROJECT_NAME" --bearerToken="token-value" --apiURL="https://api.soos.io/api/"
```


### Authenticate throughout a login form and get the auth token.

Using this option there will be an automated login form authentication performed before running the DAST scan to get the bearer token that will be then added to every request as the authorization header.

This is how a example snippet will look like:

``` bash
docker run -it soosio/dast:latest "https://example.com" --clientId="YOUR_CLIENT_ID" --apiKey="YOUR_API_KEY" --projectName="YOUR_PROJECT_NAME" --authLoginURL="https://example.com/login" --authUsername="username" --authPassword="password" --authUsernameField="userName" --authPasswordField="password" --authSubmitField="login" --apiURL="https://api.soos.io/api/"
```


### Authenticate against an OAuth token url.

In case you need to perform a DAST analysis against an OAuth application this is the snippet that you should follow. In this scenario the DAST tool will perform a request to get the `access_token` before doing any analysis.

Snippet example:
``` bash
docker run -it soosio/dast:latest "https://example.com" --clientId="YOUR_CLIENT_ID" --apiKey="YOUR_API_KEY" --projectName="YOUR_PROJECT_NAME" --oauthTokenUrl="https://example.com/token" --oauthParameters="client_id:value, client_secret:value, grant_type:value" --apiURL="https://api.soos.io/api/"
```

## Reference
* To see the full list of available parameters go to [DAST repository parameters description](https://github.com/soos-io/soos-dast#parameters)