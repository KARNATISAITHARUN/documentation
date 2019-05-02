# CLI Command Reference

The ShiftLeft CLI is invoked using the following syntax:

```
sl [global options] command [command options] [arguments...]
```

### `sl` Commands

Command | Description
--- | ---
`auth` | [Authenticate the CLI with your ShiftLeft account](../using-cli/authenticating.md).
`analyze [<path>]` | [Use ShiftLeft Inspect to analyze your application](../../using-inspect-protect/analyzing-applications-in-ci.md).  `<path>` can be the path to a `.jar`, `.war` or `.ear` file, or it can be the path to a Java project directory. If `<path>` is not provided, then `.` is implied.
`run -- <command>` | [Run the target command with ShiftLeft Protect's Microagent](../protect-java/configuring-the-microagent.md).
`push <path> [<path>...]` | (Currently not implemented) Upload policies to ShiftLeft.
`update [java-agent,libplugin]` | Update certain components of the ShiftLeft CLI, including the ShiftLeft Java Microagent (`sl update java-agent`).
`install [dotnet-agent]` | Run the ShiftLeft .NET Microagent installer.
`help`, `h` | List ShiftLeft CLI commands or help for one command.

### `sl` Global Options

Global Option | Environment Variable | Description
--- | --- | ---
`--verbose` | `SHIFTLEFT_VERBOSE=true` | Verbose mode.
`--sl-home <path>` | `SHIFTLEFT_HOME=<path>` | Path for config files and artifacts. If `$HOME` is set, defaults to `$HOME/.shiftleft`; otherwise to `/etc/shiftleft`.
`--help`, `-h` | | Display ShiftLeft CLI help text.
`--version`, `-v` | | Display the ShiftLeft CLI version.
None | `https_proxy=<proxy>` | Proxy configuration.

### `sl analyze` Options

`sl analyze` option | Environment Variable | Description
--- | --- | ---
`--cpg` | `SHIFTLEFT_CPG=true` | Submit application for analysis using CPG mode.
`--app <name>`, `-a <name>` | `SHIFTLEFT_APP=<name>` | Associate analysis with this application name. This name is used in the ShiftLeft UI.
`--wait`, `-w` | `SHIFTLEFT_WAIT=true` | Wait for analysis to finish before returning control.
`--java` | `SHIFTLEFT_LANG_JAVA=true` | Analyze Java code (implicit).
`--csharp` | `SHIFTLEFT_LANG_CSHARP=true` | Analyze C# code. 
`--estimate` | | Produce an estimate of the number of statements of the analyzed code. Requires `--cpg` to be set. (Only valid for Java.)
`--force` | | Force analysis (instead of using a cached result).
`--dotnet-core` | | Use .NET Core. (Only valid for C#.)
`--dotnet-framework` | | Use .NET Framework. (Only valid for C#.)
`--shiftleft-json-file` | `SHIFTLEFT_JSON_FILE=<path>` | Path of Microagent configuration file `shiftleft.json`. Defaults to `shiftleft.json` (in the current working directory).

### `sl run` Options

`sl run` option | Environment Variable | Description
--- | --- | ---
`--analyze <file.jar>` | `SHIFTLEFT_ANALYZE=<file.jar>` | Perform analysis before running the application. If analysis has previously been performed on this version of the application, then no further analysis is performed and the application is started immediately.
`--app <name>`, `-a <name>` | `SHIFTLEFT_APP=<name>` | Associate analysis with this application name. The name is used in the ShiftLeft UI. Only used when `--analyze` is passed.
`--cpg` | `SHIFTLEFT_CPG=true` | Submit application for analysis using CPG mode. Only used when `--analyze` is passed.
`--java` | `SHIFTLEFT_JAVA=true` | Identifies a Java application (implicit).
None | `SHIFTLEFT_CONFIG=<path> ` | Path to the `shiftleft.json` file. Defaults to `./shiftleft.json`.

### `sl install` Options

`sl install` option | Description
--- | ---
`--dotnet-core` | Use .NET Core.
`--dotnet-framework` | Use .NET Framework.

### `sl config-file` Options

`sl config-file` option | Description
--- | ---
`--app <name>`, `-a <name>` | `SHIFTLEFT_APP=<name>` | Associate analysis with this application name. This name is used in the ShiftLeft UI.
`--version-id` | | Sets the version field of the SPR ID that's written to the config file.

## Artifacts Stored by `sl`

Artifact  | Description
--- | ---
`./shiftleft.json` | Analyzer output containing information necessary for running the ShiftLeft Microagent. Used to correctly associate the analysis with the specific application version.
`$SHIFTLEFT_HOME/config.json` | Credentials file generated on successful authentication.
`$SHIFTLEFT_HOME/libplugin-a.b.c.jar` | ShiftLeft Analyzer Plugin downloaded or updated during analysis.
`$SHIFTLEFT_HOME/libplugin-latest.jar` | Symlink to the latest downloaded ShiftLeft Analyzer Plugin.
`$SHIFTLEFT_HOME/sl-microagent-x.y.z.jar` | Downloaded ShiftLeft Microagent.
`$SHIFTLEFT_HOME/sl-microagent-latest.jar` | Symlink to the latest downloaded ShiftLeft Microagent.
