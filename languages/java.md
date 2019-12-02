# ShiftLeft for Java

ShiftLeft products support applications written in Java 7+. 

## ShiftLeft Inspect for Java

ShiftLeft Inspect's Java code analysis is performed on compiled application bytecode (**not** source code). As such, you **must** successfully build your application using a supported build tool before you analyze your application using ShiftLeft Inspect.  

Component | Requirement
--- | ---
System | Linux, MacOS X, Windows
Application Type | **Java 7** through **Java 11**. To verify that you are running the supported Java version, use the `java -version` command.
Build environment | Linux or Mac with **Java 8** installed locally and with 16GB of memory available.

Analysis should be performed for each code commit or build of the application. You can [automate analysis submissions](https://docs.shiftleft.io/shiftleft/using-shiftleft-inspect-and-shiftleft-protect/workflow-integrations) using your preferred CI/CD system.

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md) and [authenticating](../using-cli/authenticating.md), use the following command to analyze your Java application with ShiftLeft Inspect

```scala
sl analyze --app <name> --java [<path>]
```
where 

`--app <name>` analyze the application of `<name>`. 

`--java` identity of the application's language.

`<path>` location of the JAR or WAR file to be analyzed.

### Next Steps

[Analyze Applications](../using-inspect-protect/inspect/analyzing-applications.md)

[Identify Branch Names](../using-inspect-protect/inspect/identify-branches.md)

[Fail a Build Based on Analysis Results](../using-inspect-protect/inspect/fail-build.md)

## ShiftLeft Ocular for Java

You can examine and investigate only compiled application bytecode (not source code) using ShiftLeft Ocular. This means that for Java applications, you must successfully build your application using a supported build tool beforehand.

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md), [authenticating](../using-cli/authenticating.md) and [starting ShiftLeft Ocular](../using-ocular/getting-started/starting.md), in order to create CPGs that contain the application code but do not include the code of libraries, ShiftLeft Ocular performs a smart inspection of the input file(s) to determine application code vs dependencies. Before creating the CPG you should [identify your application code and dependencies](../using-ocular/configure-extend/identify-code-dependencies.md); so that you know which parts of your application will be modelled. You can then decide whether to [include specific objects in the CPG](../using-ocular/getting-started/create-cpg.md#creating-a-cpg-that-includes-objects-organized-by-namespaces).

You create the Code Property Graph (CPG) for your Java application using

```scala
ocular> createCpg(<inputPaths>)
```

where `<inputPaths>` is the path of the target application; multiple applications are separated by a comma. For Java, the path is the archive (JAR, WAR or EAR file). For example, `createCpg("subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar").`

For more information, including additional options, refer to the article [Creating the CPG](../using-ocular/getting-started/create-cpg.md).

### Next Steps

[Generate the Security Profile](../using-ocular/getting-started/generate-sp.md)

[Investigate Java Vulnerable Lab](../using-ocular/tutorials/java-vuln.md)

[Examine a Java Application for Deserialization Sinks](../using-ocular/tutorials/deserialization.md)

## ShiftLeft Protect for Java

ShiftLeft Protect requires a supported 64-bit Java Runtime Environment (JRE). The Microagent memory footprint is a minimal (~50MB). ShiftLeft Protect must be able to download data from, and push metrics to, the ShiftLeft Proxy server over the specified port. If using a firewall, open a connection to a remote service on the specified TCP port.

Component | Requirement
--- | ---
System | Linux, MacOS X, Windows
JVM | 64-bit **JRE** version **7** or higher. To verify that you are running a supported 64-bit JRE, use the `java -version` command.

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md) and [authenticating](../using-cli/authenticating.md), use the following command to monitor and protect your Java application with ShiftLeft Protect

```
sl run --app <name> --java
```

where

`--app <name>`. Specifies your application's unique name.

`--java` identity of the application's language.

### Next Steps

[Secure Your Applications Using ShiftLeft Protect](../using-inspect-protect/protect/securing-applications.md)
  
[Run ShiftLeft Protect](../using-inspect-protect/protect/run-protect.md)
    
[ShiftLeft Protect for Java](https://docs.shiftleft.io/shiftleft/using-shiftleft-inspect-and-shiftleft-protect/protecting-runtime-applications/shiftleft-protect-for-java)

