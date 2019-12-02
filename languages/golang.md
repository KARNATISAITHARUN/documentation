# ShiftLeft for Golang

ShiftLeft supports analyzing Go projects using ShiftLeft Inspect, and investigating your Golang applications using ShiftLeft Ocular.

You can analyze and investigate only Go source code (**not** compiled applications). And a fully working build environment for the target project is required. 

## ShiftLeft Inspect for Golang

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md) and [authenticating](../using-cli/authenticating.md), use the following command to analyze your Golang application with ShiftLeft Inspect

```scala
sl analyze --app <name> --go [<path>]
```
  
where 

`--app <name>` analyze the application of `<name>`. 

`--go` identity of the application's language.

`<path>` location of the `.go` file to be analyzed.

### Next Steps

[Analyze Applications](../using-inspect-protect/inspect/analyzing-applications.md)

[Identify Branch Names](../using-inspect-protect/inspect/identify-branches.md)

[Fail a Build Based on Analysis Results](../using-inspect-protect/inspect/fail-build.md)

## ShiftLeft Ocular for Golang

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md), [authenticating](../using-cli/authenticating.md) and [starting ShiftLeft Ocular](../using-ocular/getting-started/starting.md), [create the Code Property Graph (CPG)](../using-ocular/getting-started/create-cpg.md) for your Golang application using

```scala
ocular> createCpg(<inputPath>)
```

or 

```scala
ocular> createCpg(List (<inputPath>), "GOLANG")
```

where `<inputPath>` is the path of the target application. For Golang, the path is the package or a package specifier that includes all of the subprojects, using the same arguments you would pass in  a `go build` command. For example `createCpg("helloshiftleftgo")`. 

## Next Steps

[Generate the Security Profile](../using-ocular/getting-started/generate-sp.md)

[Querying the CPG and Security Profile](../using-ocular/getting-started/query-cpg.md)

[Uncover the Attack Surface](../using-ocular/use-cases/attack-surface.md)
