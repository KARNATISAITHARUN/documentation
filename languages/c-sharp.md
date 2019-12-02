# ShiftLeft for C#

ShiftLeft products support applications written in C# 7.0.

## ShiftLeft Inspect for C#

The analysis of .NET applications with the following characteristics is supported:

Component | Target Type | Requirement
--- | --- | ---
Specification | | [MSBuild format](https://docs.microsoft.com/en-us/visualstudio/msbuild/msbuild?view=vs-2017) (.csproj file).
Build environment | **.NET Framework** targets | .NET **4.6.1** and [MSBuild **15.0**](#required-msbuild-information). 
| | **.NET Core** targets | .NET Core **2.1** or above.

ShiftLeft Inspect's .NET code analysis is performed on **source code** (**not** on the equivalence of byte code), and does an actual build of your application as part of the process. Therefore, the system that ShiftLeft Inspect analyzes **must** be able to [successfully build your application](#required-msbuild-information). 

Make sure you have restored dependencies and project-specific tools, before you analyze your application using ShiftLeft Inspect. Use `dotnet restore` for applications based on .NET Core; use `nuget restore` for .NET Framework.

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md) and [authenticating](../using-cli/authenticating.md), use the following command to analyze your C# application with ShiftLeft Inspect

```scala
sl analyze --app <name> --csharp [--dotnet-core|--dotnet-framework] [<path>]
```
where 

`--app <name>` analyze the application of `<name>`. 

`--csharp` identity of the application's language.

`<path>` location of the `.csproj` or .`sln` file to be analyzed.

### Required MSBuild Information

To determine the version of MSBuild installed in your system:

1. In the Windows search bar, type *Developer Command Prompt for VS*.
2. Invoke MSBuild with the version flag: `msbuild /version`.

To verify whether a .NET Framework target can be built with MSBuild 15.0:

1. Open the *Developer Command Prompt for VS*.
2. Navigate to the project location.
3. Restore NuGet packages, if any, using the command `nuget.exe restore MySolution.sln`.
3. Trigger a build using the command `msbuild MyProject.csproj` (and apply additional options, if necessary).

### Next Steps

[Analyze Applications](../using-inspect-protect/inspect/analyzing-applications.md)

[Identify Branch Names](../using-inspect-protect/inspect/identify-branches.md)

[Fail a Build Based on Analysis Results](../using-inspect-protect/inspect/fail-build.md)

## ShiftLeft Ocular for C#

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md), [authenticating](../using-cli/authenticating.md) and [starting ShiftLeft Ocular](../using-ocular/getting-started/starting.md), [create the Code Property Graph (CPG)](../using-ocular/getting-started/create-cpg.md) for your C# application using

```scala
ocular> createCpg(<inputPaths>)
```

where `<inputPaths>` is the path of the target application; multiple applications are separated by a comma. For C#, the path is the project file .csproj.

### Next Steps

[Generate the Security Profile](../using-ocular/getting-started/generate-sp.md)

[Querying the CPG and Security Profile](../using-ocular/getting-started/query-cpg.md)

[Uncover the Attack Surface](../using-ocular/use-cases/attack-surface.md)

## ShiftLeft Protect for C#

ShiftLeft Protect requires a Windows operating system with the .NET Framework runtime version 4.5 or later installed, and  works with 64-bit .NET Framework applications. ShiftLeft Protect must be able to download data from, and push metrics to, the ShiftLeft Proxy server over the secure port 443.

The ShiftLeft Protect Windows installer places the Microagent's .NET assemblies into the Global Assembly Cache (GAC). If you are running your application in Microsoft Azure as an AppService, you need to use a Windows Docker Container. See [Windows Container Support in Azure App Service](https://azure.microsoft.com/en-us/blog/announcing-the-public-preview-of-windows-container-support-in-azure-app-service/) for details on this configuration.

Component | Requirement
--- | ---
System | Windows
.NET Framework Runtime | version **4.5** or higher

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md) and [authenticating](../using-cli/authenticating.md), use the following command to monitor and protect your C# application with ShiftLeft Protect

```
sl run --app <name> --csharp
```

where

`--app <name>`. Specifies your application's unique name.

`--csharp` identity of the application's language.

### Next Steps

[Secure Your Applications Using ShiftLeft Protect](../using-inspect-protect/protect/securing-applications.md)
  
[Run ShiftLeft Protect](../using-inspect-protect/protect/run-protect.md)
    
[The ShiftLeft JSON File](../using-inspect-protect/protect/json-file.md)
