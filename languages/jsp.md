# ShiftLeft for JavaServer Pages (JSP)

ShiftLeft products support applications written in JSP.

## ShiftLeft Inspect for JSP

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md) and [authenticating](../using-cli/authenticating.md), use the following command to analyze your JSP application with ShiftLeft Inspect

```scala
sl analyze --app <name> --java [<path>]
```
where 

`--app <name>` analyze the application of `<name>`. 

`--java` identity of the application's language.

`<path>` location of the `.jsp` file to be analyzed.

### Next Steps

[Analyze Applications](../using-inspect-protect/inspect/analyzing-applications.md)

[Identify Branch Names](../using-inspect-protect/inspect/identify-branches.md)

[Fail a Build Based on Analysis Results](../using-inspect-protect/inspect/fail-build.md)

## ShiftLeft Ocular for JSP

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md), [authenticating](../using-cli/authenticating.md) and [starting ShiftLeft Ocular](../using-ocular/getting-started/starting.md), you create the Code Property Graph (CPG) for your JSP application using

```scala
ocular> createCpg(<inputPaths>)
```

where `<inputPaths>` is the path of the target application; multiple applications are separated by a comma. For JSP, the path is the archive (JAR or WAR file). For example, `createCpg("JavaVulnerableLab.war").`

For more information, including additional options, refer to the article [Creating the CPG](../using-ocular/getting-started/create-cpg.md) 

### Next Steps

[Generate the Security Profile](../using-ocular/getting-started/generate-sp.md)

[Querying the CPG and Security Profile](../using-ocular/getting-started/query-cpg.md)

[Using ShiftLeft Ocular with JSP](../using-ocular/tutorials/ocular-jsp.md)

## ShiftLeft Protect for JSP

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md) and [authenticating](../using-cli/authenticating.md), use the following command to monitor and protect your JSP application with ShiftLeft Protect

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
