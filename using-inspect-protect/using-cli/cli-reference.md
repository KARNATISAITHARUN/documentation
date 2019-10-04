# ShiftLeft Command Line Interface (CLI) Reference

Make sure you have [installed the CLI](install-cli.md). It is also recommended that you have a valid ShiftLeft account and have [authenticated with ShiftLeft](authenticating.md), since some commands require this authorization before using.

The CLI is invoked using the following syntax:

```
sl [global options] command [command options] [arguments...]
```

## `sl` Commands

Refer to [Global Options](options/global-options.md) for information on options that can be used with any `sl` command.

Command | Description
--- | ---
`analyze [<path>]` | [Analyze your application using ShiftLeft Inspect](../../using-inspect-protect/inspect/analyzing-applications.md).  `<path>` can be the path to a `.jar`, `.war`, `.ear`, `.csproj`, or `.sln` file, or it can be the path to a Java project directory.  [The `analyze` command includes options](options/analyze-options.md).
`auth` | [Authenticate the CLI with your ShiftLeft account](../using-cli/authenticating.md).
`config-file` | [Create a configuration file](../protect/json-file.md) and write it to standard output or a supplied filename. Note that the [application name and version ID options](options/config-file-options.md) are required for this command. 
`help`, `h` | List ShiftLeft CLI commands or help for one command.
`install [dotnet-agent]` | Install [ShiftLeft Protect for .NET](../protect/run-protect.md). [The `install` command includes options](options/install-options.md).
`policy <command>` | Manage [custom Policies](../../policies/custom-policy.md).
`run -- <command>` | [Run the target command with ShiftLeft Protect's Microagent](../protect/protect-java/configuring-the-microagent.md). [The `run` command includes options](options/run-options.md).
`update [java-agent,libplugin]` | Update components of the ShiftLeft CLI, including the ShiftLeft Protect for Java Microagent (`sl update java-agent`).

## `sl policy` Commands

Refer to [Policy Options](options/policy-options.md) for information on options that can be used with the `sl policy` commands.

`sl policy` Command | Description
--- | ---
`assignment` | Manage default Policies assigned to the organization and/or project.
`create` | [Create a custom Policy from a default template](../../policies/custom-policy.md).
`info` | Return Policy metadata.
`pull` | Download a Policy from the ShiftLeft repository.
`push` | Upload a Policy to the ShiftLeft repository.
`validate` | Validate syntactically and semantically a Policy.


## `sl policy assignment` Commands

Refer to [Policy Options](options/policy-options.md) for information on options that can be used with the `sl policy` commands.

`sl policy assignment` Command | Arguments | Parameters | Description
--- | --- | --- | ---
`list` | | | List all Policy assignments for your organization or project that you have previously identified for use with ShiftLeft Inspect.
`remove` | | `[--project <project-name>]` | Remove the default Policy of your organization or (if the `project` parameter is provided) project.
`set` | `<policy-label>[:<policy-tag>]` | `[--project <project-name>]` | Specify the default Policy used by ShiftLeft Inspect. If the optional `project` parameter is provided, the default Policy is only applied when ShiftLeft Inspect analyses the application specified by the `project-name` argument. If the `project` parameter is missing, the Policy is applied globally to all your organization's applications.

## Artifacts Stored by `sl`

Artifact  | Description
--- | ---
`./shiftleft.json` | ShiftLeft Inspect output containing information necessary for running ShiftLeft Protect. Used to correctly associate the analysis with the specific application version.
`$SHIFTLEFT_HOME/config.json` | Credentials file generated on successful authentication.
`$SHIFTLEFT_HOME/libplugin-a.b.c.jar` | Symlink to the ShiftLeft Analyzer Plugin downloaded or updated during analysis. The Plugin uploads the code and optionally turns a JAR into a CPG.
`$SHIFTLEFT_HOME/libplugin-latest.jar` | Symlink to the latest downloaded version of the ShiftLeft Analyzer Plugin. The Plugin uploads the code and optionally turns a JAR into a CPG.
`$SHIFTLEFT_HOME/sl-microagent-x.y.z.jar` | Symlink to the ShiftLeft Protect Microagent.
`$SHIFTLEFT_HOME/sl-microagent-latest.jar` | Symlink to the latest downloaded of the ShiftLeft Protect Microagent.
