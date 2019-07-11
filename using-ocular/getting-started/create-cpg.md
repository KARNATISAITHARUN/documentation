# Creating the Code Property Graph (CPG)

Once you've [started ShiftLeft Ocular](starting.md), you create a CPG for a new application or for a new version of an application. The CPG is the basis of custom static analysis, since it provides an extensible, multi-layered representation of each unique code version, including the various levels of abstraction. When a CPG is created, the associated default layers (security profile, semanticcpg and tagging) are also automatically created. You can create a CPG with specific options.

A new CPG is loaded into your workspace and becomes the active CPG. By default, all commands operate on the most recently loaded, active CPG. 

In order to create CPGs that contain the application code but do not include the code of libraries, ShiftLeft Ocular performs a smart inspection of the input file(s) to determine application code vs dependencies. Refer to the [API for information on the methods used to gain visibility into this process](https://ocular.shiftleft.io/api/io/shiftleft/repl/Console.html): `namespaces`, `appNamespaces` and `depNamespaces`.

The command, with all avaiable options, to create a CPG is

`createCpg(inputPaths: List[String], namespaces: List[String], overlayCreators: List[String])`

For additional information about the CPG, refer to:

* [Understanding the CPG](../../introduction/understanding-cpg.md)
* [Deep-Dive: The CPG](cpg-deep-dive.md)
* [ShiftLeft Ocular API](https://ocular.shiftleft.io/api/io/shiftleft/repl/Console.html)

## Basic Command

The easiest way to create a CPG, is to use the following command

```scala
ocular> createCpg(<inputPath>)
```

where `<inputPath>` is the location and filename of the target application, for example `"subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar"`. A file named cpg.bin.zip, containing the CPG in a binary format, is created and automatically loaded into your workspace. 

## Creating a CPG from Multiple Targets

You can create a single CPG from multiple target applications, for example for an application and several of its dependencies, or for multiple microservices. To do so, use the command

```scala
ocular> createCpg(<inputPaths>)
```

where `<inputPaths>` is a list of the locations and filenames of the target applications, separated by a comma. A file named cpg.bin.zip, containing the CPG in a binary format, is created and automatically loaded into your workspace. 

## Creating a CPG with Additional Layers 

When creating a new CPG, you can also at the same time create your own additional layers, by using the option

```scala
ocular> createCpg(<inputPath>, <layer>)
```

where `<layer>` is an additional CPG layer you want to create, as a list separated by commas. 
