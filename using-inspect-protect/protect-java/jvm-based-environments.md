# JVM Based Environments

The ShiftLeft Microagent is run with an application that has been analyzed by ShiftLeft. The Microagent runs alongside the application in production (or other environments such as staging, test, UAT).

## Requirements

To run the ShiftLeft microagent, you must:
- Use a [supported 64-bit JVM](../../introduction/requirements.md)
- Analyze the application beforehand OR pass in `--analyze <path.jar>` flag to perform analysis before running the application. If analyzed beforehand, include the `shiftleft.json` file (that was generated during analysis) with the target application you are deploying, or specify the `SHIFTLEFT_CONFIG` environment variable that points to the path for this file.

## Performing Analysis

Before running the Microagent, analysis of the target application must be performed. This allows ShiftLeft to generate instrumentation custom tailored to the specific version of the application that is run.

Performing analysis can be performed either as a separate step, allowing installation in a build / Continuous Integration (CI) environment, or in the same command used for running the agent itself.

To perform analysis beforehand, see [Analyzing Applications in CI](../../using-inspect-protect/analyzing-applications-in-ci.md). Note that in this case, you will need to carry the produced `shiftleft.json` file to your runtime environment and make it available to the ShiftLeft Microagent. This allows the Microagent to associate the application to be run with the analysis.

To perform analysis in the same command as running the agent, pass the `--analyze` flag to the `sl run` command as shown below. The system will then submit the application to the cloud for analysis, wait for it to finish, then run the application with the Microagent installed. Subsequent runs of the same command, with the same version of the application will start immediately with the Microagent installed, as the analysis is only performed the first time.

## Running your application with the ShiftLeft Microagent

### Option 1 (recommended): sl run

Use `sl run [--analyze <file.jar> --app myapp] -- <command>` to run a target application with the ShiftLeft Microagent:

For example:

```bash
sl run --analyze target/hello-shiftleft-0.0.1.jar --app hello-shiftleft -- java -jar target/hello-shiftleft-0.0.1.jar
```

or

```bash
sl run -- sbt run
```

The command performs the following tasks:

* If the `--analyze <file.jar>` flag was provided, it checks with the ShiftLeft server whether analysis of this version of the application was performed before. If it was not, it performs analysis in the cloud and waits for it to complete. If this flag is not passed, then the agent will look for the `shiftleft.json` file which resulted from performing analysis ahead of time. **Note: if you are a current ShiftLeft user and you plan to use the `--analyze` option here, please ensure that you have authenticated yourself to your ShiftLeft tenant using `sl auth` (see [Authenticating with ShiftLeft](../../using-inspect-protect/using-cli/authenticating.md)).
**
* If not already at the latest version, it downloads the latest Microagent to a local directory.
* It runs the `<command>` with a certain environment variable set, which instructs the JVM to load the Microagent.
* Before the application code is triggered, the Microagent connects to the ShiftLeft Proxy server to fetch the SPR (Security Profile for Runtime) that results from analysis.

The `sl run` process is a wrapper around the target Java application process. The `sl run` process spawns the target application process as a child. When you deploy your application using `sl run`, you will see two processes: the parent (`sl run`) and the child (application).

### Option 2: -javaagent JVM flag

If wrapping your application command line with `sl run` is not an option for you, then it is possible to instruct the JVM to load the ShiftLeft Microagent via the Java flag `-javaagent:<path-to-microagent-jar>`.

Note that passing the `javaagent` flag directly skips the ShiftLeft's Microagent's auto-update mechanism. In this situation, we strongly recommend running this command

```bash
sl update java-agent
```

every time, before starting the application, to ensure that you have the latest version. Also, use the `~/.shiftleft/sl-microagent-latest.jar` symlink, which points to the latest downloaded version of the ShiftLeft Microagent.

Example:

```bash
# Optionally analyze app (if not analyzed before)
sl analyze --wait --app example-app exampleapp.jar
# Auto-update ShiftLeft Microagent.
sl update java-agent
# Run application with ShiftLeft Microagent.
java -javaagent:~/.shiftleft/sl-microagent-latest.jar -jar exampleapp.jar
```

## shiftleft.json

The ShiftLeft Microagent is a lightweight agent that runs in-memory within the JVM of each app you want to monitor and protect using ShiftLeft. Once started, the microagent periodically pushes runtime data and metrics to the proxy server.

If the `--analyze` flag was not provided, the microagent requires the `shiftleft.json` file that is generated during analysis. The `shiftleft.json` file contains the License and SPR ID which are required parameters that must be passed using the `shiftleft.json` file.

By default the microagent expects the `shiftleft.json` file to be in the working directory where you run the application. If you are running the application in an environment different from where you performed analysis, copy `shiftleft.json` to the working directory where the application is run. 

Alternatively you can set the `SHIFTLEFT_CONFIG` environment variable to pass in the path to the `shiftleft.json` file. For example: 

```bash
export SHIFTLEFT_CONFIG=/path/to/shiftleft.json
```

## Verifying Microagent Connectivity

There are various ways to confirm that the ShiftLeft Microagent is running.

### Dashboard confirmation

As shown below, when the microagent is connected you will see this reflected in the **App Card** as "X Instances." If you don't see this, check the microagent command output.

![App Card](img/app-card.png)

In addition, runtime metrics and throughput data will appear at the **App Summary** screen for a selected application. For example:

![App Summary](img/app-sum.png)

### Process verification

The `sl run` command is a wrapper around the target Java application process. The `sl run` process spawns the target application process as a child process. So, when you deploy your application using `sl run`, you will see two Java processes: the parent `sl run` process and the child `application process`.

### Microagent output: Successful connectivity

When you run the microagent and it connects to the ShiftLeft Proxy server, you will see a success message similar to the following in the microagent output:

```
- ------------------------------
<< ShiftLeft Microagent (0.4.0)
- ------------------------------
<< 2018-02-05 11:20:01.257 INFO 1 Starting ShiftLeft Microagent...
<< 2018-02-05 11:20:01.354 INFO 6 Connecting to ShiftLeft Proxy server at agentproxy.shiftleft.io:443 (TLS enabled. ShiftLeft Proxy certificate validated via root CAs)
<< 2018-02-05 11:20:03.415 INFO 6 Requesting application SPR (non-strict mode)
<< 2018-02-05 11:20:05.205 INFO 9 Obtained SPR
```

## Configuration Options

The ShiftLeft Microagent for Java runs out-of-the-box with default settings. See [Configuring the Microagent](configuring-the-microagent.md) for details on configuration options.
