# Analyzing Applications in CI

Use the `sl analyze` command to submit applications to ShiftLeft for code analysis and security profiling.

## Analysis Workflow

- Review [code analysis requirements](../introduction/requirements.md).
- [Install ShiftLeft CLI](../using-cli/using-cli.md) on the host where you will submit applications for analysis.
- Successfully build the application using a supported build tool (maven, gradle, sbt) **before** you submit the app for analysis. 
- Submit the application for analysis using one of the supported analysis commands (see below).
- For each code commit, rebuild the application and resubmit it for analysis by integrating the CLI with an CI/CD build system to automate analysis: [Jenkins](integrating-with-shiftleft/integrating-jenkins-builds/integrating-jenkins-builds.md), [GoCD](integrating-with-shiftleft/integrating-gocd-builds.md), [Bamboo](integrating-with-shiftleft/integrating-bamboo-builds.md), [TeamCity](integrating-with-shiftleft/integrating-teamcity-builds.md), [Travis](integrating-with-shiftleft/integrating-travis-builds.md), CircleCI.

## Analysis Commands

The ShiftLeft CLI supports three modes of analysis. The mode you choose depends on your business requirements.

### `sl analyze --app <name> [<path>]`

Use `sl analyze --app <name> <path>` to upload the application to the ShiftLeft cloud for analysis. This mode of analysis is suitable for most use cases.

The flag `--app <name>` tells ShiftLeft to associate the analysis with the application `<name>`. This allows different analysis requests (of different versions of the same app) to be associated in the ShiftLeft UI.

The `sl analyze` command scans the artifact (JAR/WAR) provided by `<path>`.

### `sl analyze --app <name> --cpg [<path>]`

Use `sl analyze --app <name> --cpg <path>` to submit security metadata for proprietary application code (in lieu of bytecode), and bytecode for open source dependencies. 

Using `--cpg` mode is appropriate if you prefer to generate security metadata locally for proprietary code before submitting it to ShiftLeft for analysis. **Note**: The use of the --cpg flag consumes more memory on your build machine. Please ensure that you have at least 16GB RAM available on your build machine if you plan to use this flag.
