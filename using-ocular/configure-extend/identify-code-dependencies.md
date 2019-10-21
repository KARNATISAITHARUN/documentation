# Identifying Application Code and Dependencies

Java applications are distributed in Java Archives (JARs) and Web Application Archives (WARs). These archives contain both the application code and code for all dependencies.

Generally, you use ShiftLeft Ocular to examine your application code and determine vulnerable dependencies. As part of this 
process, ShiftLeft Ocular creates a Code Property Graph (CPG) for only the application code, with references to the dependency code. ShiftLeft Ocular makes this distinction through the use of a built-in Smart Jar Unpacker, which heuristically determines for each namespace whether it contains application or dependency code. For details, see the [API for information on the methods used to gain visibility into this process](https://ocular.shiftleft.io/api/io/shiftleft/repl/Console.html): `namespaces`, `appNamespaces` and `depNamespaces`.

Before creating the CPG for your Java application, you can identify both application code and dependencies. For information on including namespaces for analysis in the CPG, refer to the article [Creating the Code Property Graph](../getting-started/create-cpg.md)

## Show All Application Namespaces

Use the command 

```scala
namespaces(<inputPath>)
```

where <inputPath> is the location and filename of the target application, for example "subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar".
  
## Show All Application Packages

These are the packages that the  Smart Jar Unpacker treats as application packages. Use the command

```scala
appNamespaces(<inputPath>)
```

where <inputPath> is the location and filename of the target application, for example "subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar".
  
 ## Show Dependency Namespaces

Use the command 

```scala
depNamespaces(<inputPath>)
```

where <inputPath> is the location and filename of the target application, for example "subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar".
