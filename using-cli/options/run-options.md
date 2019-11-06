## `sl run` Options

`sl run` option | Environment Variable | Description
--- | --- | ---
`--analysis-timeout timeout` | `SHIFTLEFT_ANALYSIS_TIMEOUT` | Requested timeout (e.g. '15m') to be used for analysis (may be reduced by backend). If the `timeout` parameter is not set, the default is 15 minutes (15m0s). 
`--analyze <file.jar>` | `SHIFTLEFT_ANALYZE=<file.jar>` | Perform ShiftLeft Inspect analysis before running the application with ShiftLeft Protect. If analysis has previously been performed on this version of the application, then no further analysis is performed and the application is started immediately.
`--app <name>`, `-a <name>` | `SHIFTLEFT_APP=<name>` | Associate ShiftLeft Protect with the application of this name. This  application name is also used by the ShiftLeft Dashboard. Only used when `--analyze` is passed.
`--cpg` | `SHIFTLEFT_CPG=true` | Submit application to ShiftLeft Protect using the CPG mode. This mode builds the CPG locally (on your machine) and provides as the analysis result only the CPG instead of code. Only used when `--analyze` is passed.
`--csharp` | SHIFTLEFT_LANG_CSHARP | Analyze C# code.
`--dotnet-core` | | Use .NET Core. (Only valid for C#.)
`--dotnet-framework` | | Use .NET Framework. (Only valid for C#.)
`--force` | | Force analysis (instead of using a cached result) and upload.
`--install-directory value` | | Installation directory of your software package.
`--java` | `SHIFTLEFT_LANG_JAVA=true` | Analyze Java code (implicit).
`--no-cpg`| `SHIFTLEFT_NO_CPG` | Disable CPG mode.
`--policy ID` | | The ShiftLeft Policy ID used by ShiftLeft. If not set, the default Policy is used.
`--shiftleft-json-file path` | SHIFTLEFT_JSON_FILE | Path of the [Shiftleft JSON file](../../using-inspect-protect/protect/json-file.md) (default `shiftleft.json`).
`--version-id version` | | Version of the submitted application file. If unset, the version is automatically derived from the input file(s). 
None | `SHIFTLEFT_CONFIG=<path> ` |  Path of ShiftLeft Protect configuration file. Defaults to the [`shiftleft.json`](../../using-inspect-protect/protect/json-file.md) file, located in the current working directory.
