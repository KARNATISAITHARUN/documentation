# ShiftLeft for Scala

ShiftLeft products support applications written in Scala 2.12+. 

## ShiftLeft Inspect for Scala

ShiftLeft Inspect's Scala code analysis is performed on compiled application bytecode (**not** source code). As such, you **must** successfully build your application using a supported build tool before you submit the application for analysis. 

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md) and [authenticating](../using-cli/authenticating.md), use the following command to analyze your Scala application with ShiftLeft Inspect

```scala
sl analyze --app <name> --java [<path>]
```
where 

`--app <name>` analyze the application of `<name>`. 

`--java` identity of the application's language.

`<path>` location of the `.scala` file to be analyzed.

### Next Steps

[Analyze Applications](../using-inspect-protect/inspect/analyzing-applications.md)

[Identify Branch Names](../using-inspect-protect/inspect/identify-branches.md)

[Fail a Build Based on Analysis Results](../using-inspect-protect/inspect/fail-build.md)

## ShiftLeft Ocular for Scala

You can examine and investigate only compiled application bytecode (**not** source code) using ShiftLeft Ocular. This means that for Scala applications, you **must** successfully build your application using a supported build tool beforehand. 

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md) and [authenticating](../using-cli/authenticating.md), use this command to run ShiftLeft Ocular

```scala
sl ocular
```

### Next Steps

[Create the Code Property Graph (CPG)](../using-ocular/getting-started/create-cpg.md)

[Generate the Security Profile](../using-ocular/getting-started/generate-sp.md)

[Uncover the Attack Surface](../using-ocular/use-cases/attack-surface.md)

## ShiftLeft Protect for Scala

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md) and [authenticating](../using-cli/authenticating.md), use the following command to monitor and protect your Scala application with ShiftLeft Protect

```
sl run --app <name> --java
```

where

`--app <name>`. Specifies your application's unique name.

`--java` identity of the application's language.

### Next Steps

[Secure Your Applications Using ShiftLeft Protect](../using-inspect-protect/protect/securing-applications.md)
  
[Run ShiftLeft Protect](../using-inspect-protect/protect/run-protect.md)
    
[The ShiftLeft JSON File](../using-inspect-protect/protect/json-file.md)
