# `CpgLoaderConfig`

The behavior of the method `CpgLoader.load` can be altered by passing an optional configuration object of type `CpgLoaderConfig` to it as a second parameter. This class is defined in the package `io.shiftleft.codepropertygraph.cpgloading`. `CpgLoaderConfig.withDefaults` provides a default configuration. In Scala, the shorthand notation `CpgLoaderConfig()` has the same effect.

CpgLoaderConfig has the following fields:

* `createIndexes` - a Boolean value which indicates whether or not to create indexes as part of the loading process.
* `overflowDbConfig` - The configuration for overflowDB.

These fields can be set manually. For example, a configuration that deviates from the default only in that indices are not to be created can be obtained as follows,

```
val config = CpgLoaderConfig(createIndexes = false)
```

or with the following code:

```
val config = CpgLoaderConfig()
config.createIndexes = false
```
# OverflowDbConfig

The behavior of OverflowDb can be controlled via the object `config.overflowDbConfig` of type `OverflowConfig` where `config` is an object of type `CpgLoaderConfig`. The class `OverflowDbConfig` has the following fields:

* enabled - indicates whether the OverflowDB feature should be enabled or not
* graphLocation - The location of the Overflow database on disk.
* heapPercentageThreshold - The default heap percentage threshold at which overflow to disk should occur

