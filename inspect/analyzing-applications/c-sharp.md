# How to Analyze C# Applications Using Inspect

This article shows you how to analyze your applications that are written in C#. It assumes that you have already [installed and authenticated](/inspect/getting-started/getting-started.md) with ShiftLeft.

## Requirements

Inspect supports the analysis of applications written in C# 7.0 (or later) with the following characteristics:

| **Component** | **Requirement** |
| - | - |
| **Specification** | [MSBuild format](https://docs.microsoft.com/en-us/visualstudio/msbuild/msbuild?view=vs-2017) (.csproj file). |
| **Build Environment** (.NET Framework) | .NET 4.6.1 and [MSBuild 15.0+](#determining-your-msbuild-version) |
| **Build Environment** (.NET Core) | .NET Core 2.1+ |

### Determining Your MSBuild Version

You can determine the version of MSBuild installed by:

1. Launching the [Developer Command Prompt for Visual Studio](https://docs.microsoft.com/en-us/dotnet/framework/tools/developer-command-prompt-for-vs)
2. Running `msbuild /version` in the newly-launched prompt

## Building Your Application

Before analyzing your code with Inspect, we recommend building your application to make sure that you have restored dependencies and included project-specific tools. For applications based on the .NET Core, use `dotnet restore`; for applications based on the .NET Framework, use `nuget restore`.

For example, you can verify that a .NET Framework-based application can be built by doing the following:

1. Launch the [Developer Command Prompt for Visual Studio](https://docs.microsoft.com/en-us/dotnet/framework/tools/developer-command-prompt-for-vs)
2. In the newly-launched command prompt, navigate to your project location
3. Restore NuGet packages using `nuget.exe restore <MySolution.sln>`
4. Start the build with `msbuild <MyProject.csproj>`. Depending on your application, you may need to apply additional options

## Analyzing Your C# Application

To analyze your C# application, run:

```bash
sl analyze --app <name> --csharp [--dotnet-core|--dotnet-framework] [<path>]
```

| Parameter | Description |
| - | - |
| `--app <name>` | The name of the application to be analyzed |
| `--csharp` | The flag identifying the application's language |
| `--dotnet-core` or `--dotnet-framework` | Whether you're using .NET Core or .NET Framework in the development of your application |
| `<path>` | The location of the application's `.csproj` or .`sln` file to be analyzed |