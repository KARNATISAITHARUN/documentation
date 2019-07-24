# `CpgLoader` Class

Existing Code Property Graphs (CPGs) and their associated default layers and indexes are loaded into your workspace using the `CpgLoader` class. This class is defined in the package `io.shiftleft.codepropertygraph.cpgloading`. `CpgLoader` provides the following methods:

* `load`. Loads CPGs.
* `addOverlays`. Creates and loads layers.
* `createIndex`. Create indexes to increase the speed of subsequent queries

## `CpgLoader.load`

The method 'CpgLoader.load' loads the CPG of the target application(s).

For example, the following Scala command loads a CPG using `CpgLoader`, makes it available to the variable `cpg`, and queries the CPG to list all methods in your application code:

```Scala
val cpg = CpgLoader.load(filename)
cpg.method.fullName.l
```

`CpgLoader.load` can be configured using the method [`CpgLoaderConfig`](cpgloaderconfig.md).


# Creating indexes with `CpgLoader.createIndexes`
​
If you attempt to make large changes to a CPG right after loading it and before querying it, then you may want to delay creation of indexes. You can achieve this with the following code.
​
```
val cpg = CpgLoader.load(filename, CpgLoaderConfig.default.doNotCreateIndexesOnLoad)
// ... modify `cpg`
CpgLoader.createIndexes()
```
​
# Adding overlays with `CpgLoader.addOverlays`
​
Overlays stored in Zip archives can be added to CPGs using the method `CpgLoader.addOverlays`. The method accepts a sequence of filenames and a CPG on which to apply the overlays. For example, to load the CPG at `cpg.bin.zip` and the security profile overlay at `sp.bin.zip`, you can use the following code.
​
```
val cpg = CpgLoader.load("cpg.bin.zip")
CpgLoader.addOverlays(Seq("sp.bin.zip"), cpg)
