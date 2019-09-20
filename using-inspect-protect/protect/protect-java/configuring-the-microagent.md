# Configuring the ShiftLeft Protect for Java Microagent

The ShiftLeft Protect for Java Microagent runs out-of-the-box with default settings. Optionally you can configure the Microagent for your specific environment.

There are various types of configuration options for the Microagent:

* [Java properties](#java-properties) 
* [Environment variables](#environment-variables) 
* [Strictness settings](#strictness) 
* [Logging settings](#logging) 
* [Security settings](#security) 
* [HTTPS Proxy settings](#https-proxy-configuration)

Note that Java properties override values in the [ShiftLeft JSON file](../json-file.md); environment variables override the values of both the ShiftLeft JSON file and Java properties.

## Java Properties 

Configure the Microagent using Java system properties specified as arguments to the JVM command. Java properties override values in the [ShiftLeft JSON file](../json-file.md). The syntax is

`-D${prop-name}=${prop-value}`

For example, to set the debug level for logging using Java properties using

```
java -javaagent:sl-microagent-x.y.z.jar -Dshiftleft.log.level=DEBUG -jar /hello-shiftleft-x.y.z.jar
```

## Environment Variables 

Configure the Microagent using process environment variables passed to the JVM by its parent process. Environment variables override the values of both the [ShiftLeft JSON file](../json-file.md) and Java properties. The syntax is

`SHIFTLEFT_PROP_NAME="prop-value"`

For example, you can use the `SHIFTLEFT_CONFIG` environment variable to pass to the Microagent the path of the ShiftLeft JSON file.

`$ SHIFTLEFT_CONFIG=/home/myhome/shiftLeftFiles/shiftleft.json sl run -- <....>`

Another example is passing process environment variables to the JVM by setting in bash as 

```bash
export SHIFTLEFT_LOG_LEVEL=DEBUG 
```
## Strictness

The strictness setting determines how the Microagent behaves if there is a disconnection between it and the Proxy Server.  The Microagent runs in two modes: non-strict (default) and strict.

In non-strict mode (default) (`"strict":"false"`), the Microagent does not require the Security Profile at Runtime (SPR) at startup. In this case, your appliation starts but may not be monitored by the Microagent, or if it is, metrics generated during disconnection are ignored.

In strict mode (`"strict":"true"`) the Microagent requires the SPR at startup. If the Microagent loses connection with the proxy, the application methods calling the Proxy Server are paused until connection is reestablished.

Parameter | Name
--- | ---
JSON | `strict`
JVM | `-Dshiftleft.strict`
Environment Variable | `SHIFTLEFT_STRICT`

## Logging

Log level determines the level of detail that the Microagent logger outputs.

Parameter | Name
--- | ---
JSON | `log.level`
JVM | `-Dshiftleft.log.level`
Environment Variable | `SHIFTLEFT_LOG_LEVEL`

### Log Level Values

From most to least logging information.

- `TRACE`: Finest level. Useful for technical debugging. Not for use in production environments.
- `DEBUG`: Detailed level. Useful for debugging. Not for use in production environments.
- `INFO`: Reasonable informative level. Returns information relevant to the user.
- `WARNING`: Warning informative level.
- `ERROR`: Show only errors.
- `QUIET`: (default) Does not create a log file. Logging is redirected to the application `stderr`.

### Log Files

Used to write logs to file system. Denotes the file name pattern for a rolling set of logs files. If not specified, the Microagent logs are redirected to target application `stderr`.

Parameter | Name
--- | ---
JSON | `log.file`
JVM | `-Dshiftleft.log.file`
Environment Variable | `SHIFTLEFT_LOG_FILE`

### Rolling Log Files

Number of files to use in the rolling file set. Only used if logging to file system.

Parameter | Name
--- | ---
JSON | `log.maxFiles`
JVM | `-Dshiftleft.log.max.files`
Environment Variable | `SHIFTLEFT_LOG_MAX_FILES`

### Log File Size

Size limit per log file, in bytes. Only used if logging to file system.

Parameter | Name
--- | ---
JSON | `log.maxFileBytes`
JVM | `-Dshiftleft.log.max.file.bytes`
Environment Variable | `SHIFTLEFT_LOG_MAX_FILE_BYTES`

## Security

Security setting configure the detection and blocking capabilities of the Microagent.

### Mode

Security mode that determines how the Microagent respond to attacks (malicious external payloads).

Parameter | Name
--- | ---
JSON | `sec.mode`
JVM | `-Dshiftleft.sec.mode`
Environment Variable | `SHIFTLEFT_SEC_MODE`

Values:
- `REPORT`(default): Do not alter application behavior, just report the detected attacks
- `BLOCK`: Block application execution when attacks are found, by throwing a java.lang.SecurityException, and report the attack

### XXE

Type of protection to adopt when parsing XML documents of non trusted origin.

Parameter | Name
--- | ---
JSON | `sec.xxe`
JVM | `-Dshiftleft.sec.xxe`
Environment Variable | `SHIFTLEFT_SEC_XXE`

Values:
  - `OFF`: No XXE protection (default)
  - `DTD`: Disable DTDs completely. Almost all XML entity attacks are prevented including denial of services (DOS) attacks such as Billion Laughs.
  - `EXTERNAL`: Disable only external DTDs and entities. This protects against XXE attacks but not against denial of services (DOS) attacks such as Billion Laughs.

### Collect Attack Information

Enables collecting full payloads of attack events. This might include sensitive information, which is stored encrypted. Disabled by default.

Parameter | Name
--- | ---
JSON | `sec.collect.attack.info`
JVM | `-Dshiftleft.sec.collect.attack.info`
Environment Variable | `SHIFTLEFT_SEC_COLLECT_ATTACK_INFO`

 Values:
  - `true`: Attack payloads are collected and sent to ShiftLeft's infrastructure for viewing in the [Vulnerability Dashboard Event Viewer](../../../using-inspect-protect/using-workflow/vulnerability-dashboard.md#event-details).
  - `false`: (default)

## HTTPS Proxy Configuration

The Microagent supports the commonly used environment variable `https_proxy` for configuring a HTTPS proxy.

```bash
export https_proxy="http://[$user:$password@]$host:$port"
```
