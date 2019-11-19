# `sl analyze` Options

`sl analyze` option | Environment Variable | Description
--- | --- | ---
`--app <name>`, `-a <name>` | `SHIFTLEFT_APP=<name>` | Associate ShiftLeft Inspect analysis with the application of this  name. This application name is also used by the ShiftLeft Dashboard.
`--analysis-timeout timeout` | `SHIFTLEFT_ANALYSIS_TIMEOUT` | Requested timeout (e.g. '15m') to be used for analysis (may be reduced by backend). If the `timeout` parameter is not set, the default is 15 minutes (15m0s). 
`--cpg` | `SHIFTLEFT_CPG=true` | Submit application for ShiftLeft Inspect analysis using the Code Property Graph (CPG) mode. This mode builds the CPG locally (on your machine) and then uploads the CPG, rather than the code, to the ShiftLeft cloud for analysis. 
`--csharp` | `SHIFTLEFT_LANG_CSHARP=true` | Analyze C# code.
`--dotnet-core` | | Use .NET Core. (Only valid for C#.)
`--dotnet-framework` | | Use .NET Framework. (Only valid for C#.)
`--force` | | Force new analysis (instead of using a cached result) and upload.
`--go` | `SHIFTLEFT_LANG_GO=true` | Analyze Golang code.
`--java` | `SHIFTLEFT_LANG_JAVA=true` | Analyze Java code (implicit).
`--no-cpg`| `SHIFTLEFT_NO_CPG` | Disable CPG mode.
`--policy ID` | | The ShiftLeft Policy ID used by ShiftLeft. If not set, the default Policy is used.
`--shiftleft-json-file` | `SHIFTLEFT_JSON_FILE=<path>` | Path of the ShiftLeft Inspect [configuration file `shiftleft.json`](../../using-inspect-protect/protect/json-file.md). Defaults to `shiftleft.json` (in the current working directory).
`--tag app.group=<name>` |  | Displays in the ShiftLeft Dashboard an [application group](../../using-inspect-protect/using-dashboard/view-results.md#grouping-application-results), made up of the individual applications or microservices of the application. 
`--tag branch=<name>` |  | Displays in the ShiftLeft Dashboard the [application branch](../../using-inspect-protect/inspect/identify-branches.md). `<name>` is the name of the branch.
`--wait`, `-w` | `SHIFTLEFT_WAIT=true` | Wait for ShiftLeft Inspect to finish analysis before returning control.
