# Analyzing Java and Scala Applications using ShiftLeft Inspect

Make sure that you have successfully built the application using a supported build tool (maven, gradle, sbt) before you submit the application to ShiftLeft Inspect for analysis. 

For Java (including JSP) and Scala applications, use the following [ShiftLeft Command Line Interface (CLI)](../using-cli/cli-reference.md) command

```
sl analyze --app <name> --java <path>
```

to upload your Java application to the ShiftLeft cloud for analysis. 

The flag `--app <name>` tells ShiftLeft Inspect to analyze the application of `<name>`. This allows different analysis requests (for different versions of the same application) to be returned and displayed in the [ShiftLeft Dashboard](../using-dashboard/vulnerability-dashboard.md).

The `sl analyze` command analyzes the artifact (i.e. the JAR/WAR file) provided by `<path>`.

You can also optionally choose to instead generate security metadata locally using the [CPG mode](analyzing-applications.md#cpg-mode).
