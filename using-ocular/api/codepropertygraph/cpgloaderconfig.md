# `CpgLoaderConfig`

The `CpgLoaderConfig` class alters the behavior of the `CpgLoader.load` class, by passing an optional configuration object of type `CpgLoaderConfig` to the method. 

`CpgLoaderConfig` offers a static method named `default`, which provides the default configuration for `CpgLoader.load`, specifically:

* Loads the CPG.
* Creates indexes
* Enables the `overflowdb` feature to swap graphs to disk.

## Configuring `CpgLoader.load`

The configuration of `CpgLoader.load` can be altered using the following methods:

* `withStorage(path: String)`. Specifies the `overflowdb` path.
* `withoutOverflow`. Turns off `overflowdb`.
*  `doNotCreateIndexesOnLoad`. Turns off indexing.
* `createIndexesOnLoad`. Turns on indexing.

For example, if you wanted to configure `CpgLoader.load` to load a CPG, but *not* create indexes nor use the `overflowdb`feature, you would use the following Scala code

```scala
ocular> val config = CpgLoaderConfig().default.doNotCreateIndexesOnLoad.withoutOverflow
ocular> val cpg = CpgLoader.load(filename, config)
```
