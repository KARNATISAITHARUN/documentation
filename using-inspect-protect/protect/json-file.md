# The ShiftLeft JSON File

The `shiftleft.json` file is generated by ShiftLeft Inspect as part of the process of analyzing your application. This file is used by the ShiftLeft Microagent to customize the monitoring and securing of your application.  The `shiftleft.json` file  by default is located in the directory from which you run the analysis. 

If you [analyze your application](../inspect/analyzing-applications.md) using ShiftLeft Inspect as a separate step before runtime, the `shiftleft.json` file must be passed to the Microagent. The Microagent expects the file to be in the default location. If you are running your application in a different environment, copy `shiftleft.json` to the working directory of that environment.  Or you can [use the `SHIFTLEFT_CONFIG` environment variable](protect-java/configuring-the-microagent.md) to pass to the Microagent the path of the `shiftleft.json` file.

Note that if you reanalyze your application after code changes, the `sprId` parameter changes. Since you now have a new application version, you must restart the JVM of your application with the Microagent using the **updated** `shiftleft.json` file. 

## Parameters

The `shiftleft.json` file includes the parameters `accessToken` and `sprId`, which are required by the Microagent.

### `accessToken`

The `accessToken` property represents the client access token that authorizes the Microagent to use ShiftLeft services.

### `sprId`

The `sprId` property identifies the Security Profile for Runtime (SPR) for your application. 

```json
{
  "accessToken": "${access-token-string}",
  "sprId": "${sprd-id}"
}
```

The `sprId` is a string containing three main parts: organization ID, application name, and a unique version hash. For example

```
"sprId": "sl/418a892e-32fe-4d6e-b0cd-a44c24026b7a/org.springframework-my-rest-service-jar/f0e2bafa21a5790b1d70f6309189de6a1c888e16"
```

## Modifying the ShiftLeft JSON File

You can configure the Microagent by modifying the `shiftleft.json` file. Note that the syntax must conform to the ShiftLeft format.

For example, to configure the proxy and log level using the `shiftleft.json` file

```json
{
  "accessToken": "eyJhbGciOiJSUzUxMiIsInR5cCI6IkpXVCJ9.eyJl...",
  "sprId": "sl/0adb2179-40f6-443e-b346-e7b54e5efbfb/cli-hello-shiftleft-0.0.1.jar/.%2Fhello-shiftleft-0.0.1.jar/v/baseline",
  "slProxy": {
    "host": "10.8.8.10",
    "port": 443,
    "certificate": "*"
    },
  "log": {
    "level": "TRACE",
    "maxFiles": 5,
    "maxFileBytes": 10000000
  }
}
```

For additional configurion options, refer to [Configuring the ShiftLeft Protect for Java Microagent](protect-java/configuring-the-microagent.md).