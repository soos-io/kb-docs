# How to Integrate SOOS DAST with your OS Shell


<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/shell.png" alt="SOOS" width="128" height="128">
</div>

Set up a script and scan an endpoint with SOOS DAST

## Prerequisites

- You need to have a [SOOS account](https://app.soos.io/register) with DAST scanning enabled.
- Have [docker installed.](https://docs.docker.com/get-docker/) on the host machine.

## Steps

### **Get the Example**

* Navigate to the [Script DAST integration page on the SOOS App](https://app.soos.io/integrate/dast?id=script), copy the example, and modify it.

### **Run It**

* Execute the script

## Authenticated scans

### Using a Bearer Token

Example:

``` bash
docker run -it soosio/dast:latest "https://example.com/protectedendnpoint" --clientId="<client_id>" --apiKey="<api_key>" --projectName="<project_name>" --bearerToken="token-value"
```

### Authenticate with a Simple Form

Example:

``` bash
docker run -it soosio/dast:latest "https://example.com" --clientId="<client_id>" --apiKey="<api_key>" --projectName="<project_name>" --authLoginURL="https://example.com/login" --authUsername="username" --authPassword="password" --authUsernameField="userName" --authPasswordField="password" --authSubmitField="login"
```

### Authenticate with an OAuth Token URL.

Example:

``` bash
docker run -it soosio/dast:latest "https://example.com" --clientId="<client_id>" --apiKey="<api_key>" --projectName="<project_name>" --oauthTokenUrl="https://example.com/token" --oauthParameters="client_id:value, client_secret:value, grant_type:value"
```

## How to Use the `excludeUrlsFile` Parameter

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
docker run -v <YOUR_LOCAL_MACHINE_PATH>:/zap/wrk:rw -it soosio/dast --apiKey=<<api_key>> --clientId=<<client_id>> --projectName=<<project_name>>   --otherOptions="-g rules_config.txt" <TARGET_URL>
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
docker run -it <YOUR_LOCAL_MACHINE_PATH>:/zap/wrk:rw soosio/dast --apiKey=<<api_key>> --clientId=<<client_id>> --projectName=<<project_name>> --otherOptions="-c rules_config.txt" <TARGET_URL>
```

---

## Reference
* To see the full list of available parameters go to [SOOS DAST Scan Parameters](https://github.com/soos-io/soos-dast#parameters)
