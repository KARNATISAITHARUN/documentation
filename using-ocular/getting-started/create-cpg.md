# Creating and Working with the Code Property Graph (CPG)

Once you've [started ShiftLeft Ocular](starting.md), you can create a CPG for a new application or for a new version of an application. When a CPG is created, it's [`base` and default layers](../about/layers.md) are also automatically generated, and all are saved to [your workspace](manage-workspace.md) and loaded into memory. However you must intentionally [generate and load the Security Profile.](generate-sp.md) 

By default, all operations are executed on the CPG that was last loaded into memory; this is the active CPG.

Working with existing CPGs involves:

* [Loading a CPG into memory](#loading-a-cpg-into-memory).
* [Unloading a CPG from memory](#unloading-a-cpg-from-memory).
* [Deleting a CPG from disk](#deleting-a-cpg-from-disk).

For additional information about the CPG, refer to:

* [Understanding the CPG](../../introduction/understanding-cpg.md)
* [Deep-Dive: The CPG](../about/cpg-deep-dive.md)
* [Querying the CPG and Security Profiles](query-cpg.md)
* [ShiftLeft Ocular API](https://ocular.shiftleft.io/api/io/shiftleft/repl/Console.html)

## Creating a CPG

The command, with all available functions, to create a CPG is

```scala
createCpg(inputPaths: List[String], namespaces: List[String], overlayCreators: List[String])
```

ShiftLeft Ocular provides several simpler versions of `createCpg` for convenience. 

The option `<inputPaths>` is the path of the target application; multiple applications are separated by a comma. The format of the CPG is automatically inferred by ShiftLeft Ocular.

When you create a CPG, a file named `cpg.bin.zip` containing the CPG in a binary format, is created in your workspace and automatically loaded into memory; it becomes the active CPG.

### Creating a CPG for Java Applications

For Java applications, in order to create CPGs that contain the application code but do not include the code of libraries, ShiftLeft Ocular performs a smart inspection of the input file(s) to determine application code vs dependencies. Before creating the CPG you should [identify your application code and dependencies](../configure-extend/identify-code-dependencies.md); so that you know which parts of your application will be modelled. You can then decide whether to [include specific objects in the CPG](#creating-a-cpg-that-includes-objects-organized-by-namespaces).

### Creating a CPG for an Application

The easiest way to create a CPG is to use the following command

```scala
ocular> createCpg(<inputPath>)
```
For example `createCpg("subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar")`. 

You can create a single CPG from multiple applications, for example for an application and several of its dependencies, or for multiple microservices. To do so, use the command

```scala
ocular> createCpg(<inputPaths>)
``` 
For example `createCpg("subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar", "linux/driver", "linux/cryto")`.

### Creating a CPG and its Security Profile

To create a CPG and its Security Profile at the same time, use

```scala
ocular> createCpgAndSp(<inputPath>)
```
  
Both the CPG and the `securityprofile` layer are created in your workspace and automatically loaded into memory. The sp.bin.zip file contains the Security Profile in a binary format.

### Creating a CPG that Includes Objects Organized by Namespaces

If your application uses namespaces (e.g. Java packages) to organize objects, you can specify which namespaces contain code you want represented in the CPG. The command is

```scala
ocular> createCpg(<inputPath>, <namespace>)
```
where `<namespace>` is one or more namespaces you want to include in the CPG; multiple namespaces are separated by a comma. For example `createCpg("subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar", List("io.shiftleft", "org.foo")).`

## Working with CPGs

You work with the CPGs, and their associated layers, that are in your workspace and loaded into memory. By default, all operations are executed on the CPG that was last loaded into memory; this is the active CPG.

The option `<names>` is the name of one or more CPGs you want to work with; multiple names are separated by a comma. For example `"hello-shiftleft-0.0.1-SNAPSHOT.jar"`. The format of the CPG is automatically inferred by ShiftLeft Ocular.

### Loading a CPG into Memory

When you load an existing CPG into memory, all default layers and the `securityprofile` layer (if it already exists), are also automatically loaded. To load a CPG, use the command

```scala
ocular> loadCpg(<names>)
```
The number and size of CPGs that can be loaded into your workspace at any one time is limited by the amount of [available memory](../about/ocular-memory-size.md).

### Unloading a CPG from Memory

Unloading a CPG (and its associated layers) removes it from memory, but not from disk. When you unload a CPG, the most recently loaded CPG becomes the active CPG.

To unload the active CPG, use the command

```scala
ocular> unloadCpg
```

To unload a specific CPG, use the command

```scala
ocular> unloadCpg(<names>)
```
  
### Deleting a CPG from Disk

Deleting a CPG (and its associated layers) from disk also deletes it from your workspace. When you delete a CPG, the most recently loaded CPG become the active CPG.

To delete the active CPG, use the command

```scala
ocular> deleteCpg
```

To delete a specific CPG, use the command

```scala
ocular> deleteCpg(<names>)
```
