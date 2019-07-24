## Configuring `CpgLoader.load` Using 
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
