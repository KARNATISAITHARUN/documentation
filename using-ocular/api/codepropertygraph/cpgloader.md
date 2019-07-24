# `CpgLoader` Class

Existing Code Property Graphs (CPGs) and their associated default layers (overlays) and indexes are loaded into your workspace using the `CpgLoader` class. This class is defined in the package `io.shiftleft.codepropertygraph.cpgloading`. `CpgLoader` provides the following methods:

* `load`. Loads CPGs.
* `addOverlays`. Loads layers and adds them to the active CPG.
* `createIndex`. Create indexes to increase the speed of subsequent queries

The default behavior of `CpgLoader` can be altered using the [`CpgLoaderConfig` class](cpgloaderconfig.md)

## `CpgLoader.load`

The method 'CpgLoader.load' loads the CPG of the target application(s).

For example, the following Scala command loads a CPG using `CpgLoader`, makes it available to the variable `cpg`, and queries the CPG to list all methods in your application code:

```scala
ocular> val cpg = CpgLoader.load(<filename>)
cpg.method.fullName.l
```

where `filename` is the absolute location of the target application, for example "cpg.bin.zip".
  
## `CpgLoader.addOverlays`

The method `CpgLoader.addOverlays` loads a layer and adds it to the active CPG. 

For example, use the following Scala code to load both the CPG and its Security Profile layer 

```scala
ocular> val cpg = CpgLoader.load(filename)
CpgLoader.addOverlays(Seq(filename), cpg)
```
where `filename` is the absolute location of the layer, for example "sp.bin.zip".
  
## `CpgLoader.createIndexes`

CpgLoader.createIndexes(cpg)

look in the CPG for all methods with the name foo, if the graph is not indexed that Ocular would have to go to every node to check, taking time. Instead create an index once, which precalculate which nodes which have that name. Then the next time you make the query, much faster. By default, Ocular creates indexes. Performance consideration to turn off indexes, make changes to CPG, and then create the indexes. Indexes are not stored, and are recreated each time a CPG. Expensive in terms of run-time (how long your program runs)

Modify CPG for example to add nodes or edges. Use case tag methods of interest, like routines that you have already seen, or false positives.


