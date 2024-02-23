# How to Integrate SOOS DAST with your OS Shell


<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/shell.png" alt="SOOS" width="128" height="128">
</div>

Currently you have the option to integrate the SOOS DAST product using the command line from your OS.


## Prerequisites

- You need to have a [SOOS account.](https://app.soos.io/register)
- Have [docker installed.](https://docs.docker.com/get-docker/) in the Host machine.
## Steps
### **Getting the script**
* Navigate to the [Script DAST integration page on the SOOS App](https://app.soos.io/integrate/dast?id=script) and copy the script content according to the OS you're running.

### **Environment Setup**
* In all of our example scripts we are using environment variables, we strongly recommend that you also set up your SOOS_CLIENT_ID and SOOS_API_KEY as environment variables too.


### Script Arguments

| Argument | Required | Description |
| --- | --- | --- |
| `targetURL` | Yes | Target URL - URL of the site or api to scan. It can be the path of an API specification file when doing apiscan. The URL should include the protocol. Ex: https://www.example.com |

### Script Parameters

| Argument | Default | Description |
| --- | --- | --- |
| `--ajaxSpider` | None | Ajax Spider - Use the ajax spider in addition to the traditional one. Additional information: https://www.zaproxy.org/docs/desktop/addons/ajax-spider/ |
| `--apiKey` | None | SOOS API Key - get yours from https://app.soos.io/integrate/dast |
| `--appVersion` | None | App Version - Intended for internal use only. |
| `--authDelayTime` | 5 | Delay time in seconds to wait for the page to load after performing actions in the form. (Used only on authFormType: wait_for_password and multi_page) |
| `--authFormType` | simple | simple (all fields are displayed at once), wait_for_password (Password field is displayed only after username is filled), or multi_page (Password field is displayed only after username is filled and submit is clicked) |
| `--authLoginURL` | None | Login url to use when authentication is required |
| `--authPassword` | None | Password to use when authentication is required |
| `--authPasswordField` | None | Password input id to use when authentication is required |
| `--authSecondSubmitField` | None | Second submit button id to use when authentication is required (for multi-page forms) |
| `--authSubmitAction` | None | Submit action to perform on form filled. Options: click or submit |
| `--authSubmitField` | None | Submit button id to use when authentication is required |
| `--authUsername` | None | Username to use when authentication is required |
| `--authUsernameField` | None | Username input id to use when authentication is required |
| `--authVerificationURL` | None | URL used to verify authentication success, should be an URL that is expected to throw 200/302 during any authFormType authentication. If authentication fails when this URL is provided, the scan will be terminated. |
| `--bearerToken` | None | Bearer token to use in a request header for auth |
| `--branchName` | None | The name of the branch from the SCM System |
| `--branchURI` | None | The URI to the branch from the SCM System |
| `--buildURI` | None | URI to CI build info |
| `--buildVersion` | None | Version of application build artifacts |
| `--checkoutDir` | None | Checkout directory to locate SARIF report |
| `--clientId` | None | SOOS Client ID - get yours from https://app.soos.io/integrate/dast |
| `--commitHash` | None | The commit hash value from the SCM System |
| `--contextFile` | None | Context file which will be loaded prior to scanning the target |
| `--debug` | False | Enable debug logging for ZAP. |
| `--excludeUrlsFile` | | Path to a file containing regex URLs to exclude, one per line. eg `--excludeUrlsFile=exclude_urls.txt`
| `--disableRules` | None | Comma separated list of ZAP rules IDs to disable. List for reference https://www.zaproxy.org/docs/alerts/ |
| `--fullScanMinutes` | None | Number of minutes for the spider to run |
| `--help`, `-h` | ==SUPPRESS== | show this help message and exit |
| `--integrationName` | None | Integration Name - Intended for internal use only. |
| `--integrationType` | None | Integration Type - Intended for internal use only. |
| `--logLevel` | None | Minimum level to show logs: INFO, WARN, FAIL, DEBUG, ERROR. |
| `--oauthParameters` | None | Parameters to be added to the oauth token request. (eg --oauthParameters="client_id:clientID, client_secret:clientSecret, grant_type:client_credentials") |
| `--oauthTokenUrl` | None | The authentication URL that grants the access_token. |
| `--onFailure` | continue_on_failure | Action to perform when the scan fails. Options: fail_the_build, continue_on_failure |
| `--operatingEnvironment` | None | Set Operating environment for information purposes only |
| `--otherOptions` | None | Additional command line arguments for items not supported by the set of parameters above |
| `--outputFormat` | None | Output format for vulnerabilities: only the value SARIF is available at the moment |
| `--projectName` | None | Project Name - this is what will be displayed in the SOOS app |
| `--requestCookies` | None | Set Cookie value(s) for each request |
| `--requestHeaders` | None | Set extra request headers set on each request |
| `--scanMode` | baseline | Scan Mode - Available modes: baseline, fullscan, and apiscan (for more information about scan modes visit https://github.com/soos-io/soos-dast#scan-modes) |
| `--scriptVersion` | None | Script Version - Intended for internal use only. |

### **Scan Modes**

**Baseline**

It runs the [ZAP](https://www.zaproxy.org/) spider against the specified target for (by default) 1 minute and then waits for the passive scanning to complete before reporting the results.

This means that the script doesn't perform any actual ‘attacks’ and will run for a relatively short period of time (a few minutes at most).

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

--- 

### How to Use the `excludeUrlsFile` Parameter

The `excludeUrlsFile` parameter allows you to specify a list of URLs that you want to exclude from the DAST scan. This is particularly useful for focusing the scan on specific parts of your application and avoiding unnecessary analysis of irrelevant areas. To use this feature effectively, you need to create a text file containing regular expressions that match the URLs you wish to exclude.

#### Example Usage:

When you run the SOOS DAST tool, you can include the `--excludeUrlsFile` parameter followed by the path to your file containing the exclusion patterns. For example:

```bash
docker run -it soosio/dast ... --scanMode="fullscan" --excludeUrlsFile="exclude_urls.txt"
```

In this example, `exclude_urls.txt` is a file containing regular expression patterns. Each pattern in this file specifies a URL or a set of URLs to be excluded from the scan.

#### Content Format of `exclude_urls.txt`:

Here’s an example of what the contents of `exclude_urls.txt` might look like:

```txt
^https://app\.soos\.io/research/vulnerabilities/(?!CVE-2015-8048/|CVE-2015-8047/).+$
^https://app\.soos\.io/research/repositories/(?!github/soos-io/soos-ci-analysis-python/).+$
^https://app\.soos\.io/research/licenses/(?!0BSD/|AAL/).+$
^https://app\.soos\.io/research/packages/[^/]+/(?!@soos-io/soos-sbom/|@soos-io/soos-sca/).+$
^https://app\.soos\.io/research/license-exceptions/(?!vsftpd-openssl-exception).+$
```

Each line in this file is a regular expression that matches URLs to be excluded, except for certain exceptions specified within the pattern.

#### Understanding the Regex Patterns:

- `^https://app\.soos\.io/research/vulnerabilities/(?!CVE-2015-8048/|CVE-2015-8047/).+$`: This pattern matches all URLs under the "vulnerabilities" directory except for `CVE-2015-8048` and `CVE-2015-8047`. Only these two exceptions will be scanned.
  
- `^https://app\.soos\.io/research/repositories/(?!github/soos-io/soos-ci-analysis-python/).+$`: This pattern matches all URLs under the "repositories" directory except for the `soos-ci-analysis-python` repository on GitHub.

- `^https://app\.soos\.io/research/licenses/(?!0BSD/|AAL/).+$`: This pattern excludes all URLs under the "licenses" directory except for `0BSD` and `AAL` licenses.

- `^https://app\.soos\.io/research/packages/[^/]+/(?!@soos-io/soos-sbom/|@soos-io/soos-sca/).+$`: This pattern matches all URLs under the "packages" directory except for `soos-sbom` and `soos-sca` packages.

- `^https://app\.soos\.io/research/license-exceptions/(?!vsftpd-openssl-exception).+$`: This pattern excludes all URLs under the "license-exceptions" directory except for `vsftpd-openssl-exception`.

By leveraging the `excludeUrlsFile` parameter, you can tailor your DAST scans to focus on specific areas of your application, enhancing efficiency and relevance of the scan results.

### How to Configure Rules by Passing a Rules File

We support ZAP rules configuration by passing a file to indicate to INFO, IGNORE or FAIL rules.

This is especially useful as it lets you tailor your scan and focus only on the rules that matter for your application, reducing the time the scan takes to complete and with that the amount of request being done against the site.

#### Getting the config file to edit

To get a default config with all the rules being executed you should run this command first:

```bash
docker run -it <YOUR_LOCAL_MACHINE_PATH>:/zap/wrk:rw soosio/dast --apiKey=<YOUR_API_KEY> --clientId=<YOUR_CLIENT_ID> --projectName=<YOUR_PROJECT_NAME>   --otherOptions="-g rules_config.txt" <TARGET_URL>
```

Once you run the command above, you should be able to find in the path you've specified mounting the docker volume, a new file `rules_config.txt` that would look something like this (it might vary depending on the `scanType` chosen):

```rules_config.txt
# zap-baseline rule configuration file
# Change WARN to IGNORE to ignore rule or FAIL to fail if rule matches
# Only the rule identifiers are used - the names are just for info
# You can add your own messages to each rule by appending them after a tab on each line.
....Other rules
10054	WARN	(Cookie without SameSite Attribute)
10055	WARN	(CSP)
10056	WARN	(X-Debug-Token Information Leak)
10057	WARN	(Username Hash Found)
10061	WARN	(X-AspNet-Version Response Header)
10062	WARN	(PII Disclosure)
10063	WARN	(Permissions Policy Header Not Set)
....Other rules
```

Now the next step would be to modify this file to `IGNORE` or `FAIL` according to each rule. For example, if one doesn't use `ASP.Net` in the application, it doesn't make much sense to have the rule `10061	WARN	(X-AspNet-Version Response Header)` enabled, so we would just switch that from `WARN` to `IGNORE`.

Resulting in this txt:

```rules_config.txt
# zap-baseline rule configuration file
# Change WARN to IGNORE to ignore rule or FAIL to fail if rule matches
# Only the rule identifiers are used - the names are just for info
# You can add your own messages to each rule by appending them after a tab on each line.
....Other rules
10054	WARN	(Cookie without SameSite Attribute)
10055	WARN	(CSP)
10056	WARN	(X-Debug-Token Information Leak)
10057	WARN	(Username Hash Found)
10061	IGNORE	(X-AspNet-Version Response Header)
10062	WARN	(PII Disclosure)
10063	WARN	(Permissions Policy Header Not Set)
....Other rules
```

#### Using the modified config rules

Once we have modified and saved the `rules_config.txt` with the modified rules, it's time to run the scan with this configuration.

It's similar to the command to generate the rules, but the only change is that we switch `-g` for `-c` to pass in the new `rules_config.txt` file:

```bash
docker run -it <YOUR_LOCAL_MACHINE_PATH>:/zap/wrk:rw soosio/dast --apiKey=<YOUR_API_KEY> --clientId=<YOUR_CLIENT_ID> --projectName=<YOUR_PROJECT_NAME> --otherOptions="-c rules_config.txt" <TARGET_URL>
```

## Reference
* To see the full list of available parameters go to [DAST repository parameters description](https://github.com/soos-io/soos-dast#parameters)
