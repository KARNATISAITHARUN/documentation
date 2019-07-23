Loading of code property graphs and overlays is performed via the class `CpgLoader` defined in the package `io.shiftleft.codepropertygraph.cpgloading`. The class provides a method to load code property graphs (`load`), a method to add overlays (`addOverlays`), and a method to create indexes to increase the speed of subsequent queries (`createIndex`).
​
# Loading CPGs with `CpgLoader.load`
​
Loading of CPGs from files can be achieved with the method
​
```
CpgLoader.
```
​
The following Scala code loads a CPG via `CpgLoader`, makes it available via the variable `cpg`, and queries it to list all methods:
​
```
val cpg = CpgLoader.load(filename)
cpg.method.fullName.l
```
​
# The CPG Loader Configuration `CpgLoaderConfig`
​
By default, `CpgLoader.load` will
-  load the CPG
- create indexes
- use the `overflowdb` feature to swap graphs to disk.
​
To alter the behavior of `CpgLoader.load`, you can pass an optional configuration object of type `CpgLoaderConfig` to the method. For example, the following code snippet loads a CPG but does not create indexes, and does not use the overflowdb feature.
​
```
val config = CpgLoaderConfig().default.doNotCreateIndexesOnLoad.withoutOverflow
val cpg = CpgLoader.load(filename, config)
```
​
Just like `CpgLoader`, `CpgLoaderConfig` is defined in the package `io.shiftleft.codepropertygraph.cpgloading`. It offers a static method named `default`, which yields a default configuration. This configuration can be modified via the following methods:
​
​
- withStorage(path: String): returns the existing configuration with overflowdb path as specified by `path`
- withoutOverflow: returns existing configuration with overflowdb turned off.
- doNotCreateIndexesOnLoad: existing configuration without indexing on load.
- createIndexesOnLoad: returns existing configuration but with indexing on load.
​
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
# Adding overlays
​
...
Collapse



I think I can finish this one today. For now addOverlays is still missing (edited) 
fabs 5:23 PM
There wasn't much left to do. Here is the complete set of notes for the CpgLoader API.
CpgLoader.md 
​
​
The `codepropertygraph` project offers a Scala API for processing of code property graphs, and in the following, we provide Scala examples for accessing this API. The API can however be accessed via any JVM-based language, e.g., from Java, Kotlin, or Groovy.
​
Loading of code property graphs and overlays is performed via the class `CpgLoader` defined in the package `io.shiftleft.codepropertygraph.cpgloading`. The class provides a method to load code property graphs (`load`), a method to add overlays (`addOverlays`), and a method to create indexes to increase the speed of subsequent queries (`createIndex`).
​
# Loading CPGs with `CpgLoader.load`
​
Loading of CPGs from Zip archives can be achieved with the method
​
```
CpgLoader.
```
​
The following Scala code loads a CPG via `CpgLoader`, makes it available via the variable `cpg`, and queries it to list all methods:
​
```
val cpg = CpgLoader.load(filename)
cpg.method.fullName.l
```
​
# The CPG Loader Configuration `CpgLoaderConfig`
​
By default, `CpgLoader.load` will
-  load the CPG
- create indexes
- use the `overflowdb` feature to swap graphs to disk.
​
To alter the behavior of `CpgLoader.load`, you can pass an optional configuration object of type `CpgLoaderConfig` to the method. For example, the following code snippet loads a CPG but does not create indexes, and does not use the overflowdb feature.
​
```
val config = CpgLoaderConfig().default.doNotCreateIndexesOnLoad.withoutOverflow
val cpg = CpgLoader.load(filename, config)
```
​
Just like `CpgLoader`, `CpgLoaderConfig` is defined in the package `io.shiftleft.codepropertygraph.cpgloading`. It offers a static method named `default`, which yields a default configuration. This configuration can be modified via the following methods:
​
​
- withStorage(path: String): returns the existing configuration with overflowdb path as specified by `path`
- withoutOverflow: returns existing configuration with overflowdb turned off.
- doNotCreateIndexesOnLoad: existing configuration without indexing on load.
- createIndexesOnLoad: returns existing configuration but with indexing on load.
​
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
```
