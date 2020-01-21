# How to Analyze Java Applications Using Inspect

This article shows you how to analyze your applications that are written in Java. It assumes that you have already [installed and authenticated](/inspect/getting-started/README.md) with ShiftLeft.

## Requirements

Inspect supports the analysis of applications written in Java 7 through Java 11.

Your build environment must have:

* Java 8 installed locally (depending on your application development needs, you may need to run multiple versions of Java concurrently))
* At least 16 GB of memory available

## Building Your Application

Inspect's code analysis is performed on the **compiled application bytecode** (*not* on the source code). As such, you must build your application before you can analyze the application with Inspect.

Some build tools you might consider include Maven, Gradle, sbt, etc.

## Analyzing Your Java Application

To analyze your Java application, run:

```bash
sl analyze --app <name> --java [<path>]
```

| Parameter | Description |
| - | - |
| `--app <name>` | The name of the application to be analyzed |
| `--java` | The flag identifying the application's language |
| `<path>` | The location of the application's `.jar` or .`war` file to be analyzed |

### CPG Mode

Optionally, you can choose to analyze your application using the Code Property Graph (CPG) mode. With CPG mode, ShiftLeft builds the CPG locally, then uploads it (rather than your application's code) to the ShiftLeft cloud for analysis.

To analyze your application using CPG mode, include the option `--cpg` in the `sl analyze` command (e.g., `sl analyze --app <name> --java --cpg <path>`).
