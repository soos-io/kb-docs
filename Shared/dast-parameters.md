### Parameters

| Name              | Required                          | Description                                                                                     |
|-------------------|-----------------------------------|-------------------------------------------------------------------------------------------------|
| `clientId`        | Yes                               | SOOS client id                                                                                  |
| `apiKey`          | Yes                               | SOOS API key                                                                                    |
| `projectName`     | Yes                               | SOOS project name                                                                               |
| `scanMode`        |                                   | SOOS DAST scan mode. Values: `baseline`, `fullscan`, or `apiscan`. By Default: `baseline`       |
| `apiURL`          |                                   | SOOS API URL. By Default: `https://app.soos.io/api/`                                            |
| `debug`           |                                   | show debug messages. By Default: `false`                                                        |
| `ajaxSpider`      |                                   | use the Ajax spider in addition to the traditional one. By Default: `false`                     |
| `rules`           |                                   | rules file to use for `INFO`, `IGNORE` or `FAIL` warnings                                       |
| `contextFile`     |                                   | context file which will be loaded prior to scanning the target. Required for authenticated URLs |
| `contextUser`     |                                   | username to use for authenticated scans - must be defined in the given context file             |
| `fullScanMinutes` | Yes - if `scanMode` is `fullscan` | the number of minutes for spider to run                                                         |
| `apiScanFormat`   | Yes - if `scanMode` is `apiscan`  | target API format: `openapi`, `soap`, or `graphql`                                              |
| `level`           |                                   | minimum level to show: `PASS`, `IGNORE`, `INFO`, `WARN` or `FAIL`                               |
| `zapOptions`      |                                   | add zap options                                                                                 |
| `requestCookies`  |                                   | add custom cookies to the requests                                                              |
| `requestHeaders`  |                                   | add custom headers to the requests                                                              |
| `integrationName` |                                   | identify the integration system used. By Default: `Script`                                      |