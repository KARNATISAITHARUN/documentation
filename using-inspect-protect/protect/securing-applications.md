# Securing Your Applications Using ShiftLeft Protect

ShiftLeft Protect secures applications against exploitation of vulnerabilities by leveraging code informed insights discovered by [ShiftLeft Inspect](../inspect/analyzing-applications.md) to create runtime security specifications. This approach allows ShiftLeft Protect to operate with minimal footprint and overhead.

The ShiftLeft Protect process is:

1. Download the latest version of the [ShiftLeft Protect Microagent](#shiftleft-protect-microagent), if necessary.
2. Deploy the Microagent, using the [specific environment variable](protect-java/configuring-the-microagent.md).
3. Use the [ShiftLeft JSON file](#the-shiftleft-json-file) to provide the Microagent with required information from the analysis. 
4. Connect to the [ShiftLeft Proxy Server](#the-shiftleft-proxy-server) to download the [ShiftLeft Security Profile for Runtime (SPR)](#the-shiftleft-security-profile-for-runtime). 
5. Push events and metrics to the [ShiftLeft Dashboard](#the-shiftleft-dashboard).

## ShiftLeft Protect Microagent

The Microagent is a lightweight agent that is deployed and runs in-memory with applications (in production or other environments such as staging, test and UAT) you want to monitor and protect from residual issues. The Microagent is customized to your application's specific shape and weaknesses. 

The Microagent is configured with default settings that support core operations. These default settings support various configuration mechanisms, including values in the `shiftleft.json` file, Java properties and environment variables. You can [configure the Microagent for your specific environment](protect-java/configuring-the-microagent.md).

## The ShiftLeft JSON File

ShiftLeft Inspect generates the `shiftleft.json` file as part of the process of analyzing your application. This file includes the parameters required by ShiftLeft Protect. 

Note that if you analyze your Java application using ShiftLeft Inspect as a [separate step before runtime](../inspect/analyzing-applications.md), the `shiftleft.json` file must be passed to ShiftLeft Protect. 

Refer to the article on [the ShiftLeft JSON File](json-file.md) for additional information.

## The ShiftLeft Proxy Server

The Microagent connects to the ShiftLeft Proxy server to obtain the SPR and push runtime metrics to the ShiftLeft Dashboard. The following options are available for Microagent-proxy connections.

**Important**. The ShiftLeft Proxy Server is not to be confused with system proxy configuration.

### Proxy Host Name

Proxy host name or IP address.

Parameter | Name
--- | ---
JSON | `slProxy.host`
JVM | `-Dshiftleft.sl.proxy.host=`
Environment Variable | `SHIFTLEFT_SL_PROXY_HOST`

### Proxy Port

Proxy listening TCP port.

Parameter | Name
--- | ---
JSON | `slProxy.port`
JVM | `-Dshiftleft.sl.proxy.port`
Environment Variable | `SHIFTLEFT_SL_PROXY_PORT`

## The ShiftLeft Security Profile for Runtime

The SPR is generated by the ShiftLeft code analyzer for use by ShiftLeft Protect. It specifies the vulnerabilities that the ShiftLeft Protect Microagent monitors and protects from in runtime.

## The ShiftLeft Dashboard

ShiftLeft Protect pushes runtime metrics to the [ShiftLeft Dashboard](../using-dashboard/vulnerability-dashboard.md). The Dashboard is a singular view of application security quality metrics, providing a list of vulnerabilities based on ShiftLeft Protect runtime analysis of your applications. 
