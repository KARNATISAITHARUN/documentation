# How to Analyze Scala Applications Using Inspect

This article shows you how to analyze your applications that are written in Scala. It assumes that you have already [installed and authenticated](/inspect/getting-started/README.md) with ShiftLeft.

## Requirements

Inspect supports the analysis of applications written in Scala 2.12 (or later).

## Building Your Application

Inspect's code analysis is performed on **compiled application bytecode** (*not* on source code) and the code analysis process includes a build of your application. As such, you must build your application before you can analyze the application with Inspect.

Some build tools you might consider include Maven, Gradle, sbt, etc.

## Analyzing Your Scala Application

To analyze your Scala application, run:

```bash
sl analyze --app <name> --scala [<path>]
```

| Parameter | Description |
| - | - |
| `--app <name>` | The name of the application to be analyzed |
| `--scala` | The flag identifying the application's language |
| `<path>` | The location of the application's `.scala` file to be analyzed |

### CPG Mode

Optionally, you can choose to analyze your application using the Code Property Graph (CPG) mode. With CPG mode, ShiftLeft builds the CPG locally, then uploads it (rather than your application's code) to the ShiftLeft cloud for analysis.

To analyze your application using CPG mode, include the option `--cpg` in the `sl analyze` command (e.g., `sl analyze --app <name> --scala --cpg <path>`).
