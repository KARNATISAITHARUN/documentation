# Analyzing C# Applications using ShiftLeft Inspect

Make sure you have restored dependencies and project-specific tools, before you submit the application to ShiftLeft Inspect for analysis. Use `dotnet restore` for applications based on .NET Core; use `nuget restore` for .NET Framework.

Use the [ShiftLeft CLI](../using-cli/cli-reference.md) command 

```
sl analyze --app <name> --csharp [--dotnet-core|--dotnet-framework] [<path>]
```
  
to build the CPG locally and then upload the CPG to the ShiftLeft cloud for analysis (i.e. [CPG mode](analyzing-applications.md#cpg-mode)). 
  
where 

`--app <name>` analyze the application of `<name>`. This allows different analysis requests (for different versions of the same application) to be returned and displayed in the [ShiftLeft Dashboard](../using-dashboard/vulnerability-dashboard.md).

`--csharp` identity of the application's language.

`<path>` location of a `.csproj` or `.sln` file to be analyzed.
