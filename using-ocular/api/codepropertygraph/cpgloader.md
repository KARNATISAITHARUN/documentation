# `CpgLoader` Class

Existing Code Property Graphs (CPGs) and their associated layers (overlays) and indexes are loaded into memory using the `CpgLoader` class. This class is defined in the package `io.shiftleft.codepropertygraph.cpgloading`. `CpgLoader` provides the following methods:

* `load`. Loads the CPGs into memory
* `addOverlays`. Loads layers and adds them to the active CPG.
* `createIndex`. Create indexes to increase the speed of subsequent queries.

The default behavior of `CpgLoader` can be altered by providing a configuration of type [`CpgLoaderConfig` class](cpgloaderconfig.md) to `load`.

## `CpgLoader.load`

The method 'CpgLoader.load' loads the CPG of the target application(s), represented by the file `cpg.bin.zip` containing the CPG in a binary format.

For example, the following Scala command loads a CPG using `CpgLoader`, makes it available to the variable `cpg`, and queries the CPG to list all methods in your application code:

```scala
ocular> val cpg = CpgLoader.load(<inputpath>)
cpg.method.fullName.l
```

The option `inputPath` is the path of the target application. For example, ("subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar").The format of the CPG is automatically inferred by ShiftLeft Ocular.
  
## `CpgLoader.addOverlays`

The method `CpgLoader.addOverlays` loads a layer into memory and adds it to the specified CPG. 

For example, use the following Scala code to load both the CPG and its Security Profile layer 

```scala
ocular> val cpg = CpgLoader.load("cpg.bin.zip")
CpgLoader.addOverlays(Seq(<inputpath>), cpg)
```
where `inputpath` is the location of the target layer, represented by the file `sp.bin.zip` containing the Security Profile in a binary format.
  
## `CpgLoader.createIndexes`

The method `CpgLoader.createIndexes` creates an index and loads it into memory. Creating an index saves time when querying  the CPG, though at a run-time performance cost.

By default, ShiftLeft Ocular creates indexes for each CPG, each time you load a CPG (indexes are not persisted). It is therefore recommended that, if you plan to make changes to the CPG like adding nodes or edges, you first [turn off the creation of indexes](cpgloaderconfig.md), make your changes, and then create the indexes using the following command

```
CpgLoader.createIndexes(cpg)
```
