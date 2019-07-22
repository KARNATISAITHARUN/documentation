# About the tarpit Sample Application

[tarpit](https://github.com/conikeec/tarpit) is a open source sample Java Web application. It contains various classes of vulnerabilitie, 
such as backdoors, sensitive data leaks, logic bombs and magic values. 

tarpit is used in the documentation to illustrate using ShiftLeft Ocular for various use cases. Before using tarpit in a use case you must:

1. [Download and compile tarpit](#downloading-and-compiling-tarpit).
2. [Load tarpit into ShiftLeft Ocular](#loading-tarpit-into-shiftleft-ocular).
3. [Identify dependencies](#identifying-dependencies).
4. [Importing convenience functions](#import-convenience-functions).

## Downloading and Compiling tarpit

Before you can use tarpit with ShiftLeft Ocular, you must [clone the tarpit repository](https://github.com/conikeec/tarpit) and then compile it using either Maven or Gradle.

This creates the tarpit WAR file `servlettarpit.war`. The WAR file is used as the ShiftLeft Ocular target application.

## Loading tarpit into ShiftLeft Ocular

1. [Start ShiftLeft Ocular](../getting-started/starting.md).

2. Create the Code Property Graph (CPG) and Security Profile for the `tarpit` application, for example 

```scala
ocular> createCpgAndSp("tarpit/target/servlettarpit.war")

```

where `tarpit/target/servlettarpit.war` is the absolute path of the tarpit WAR file.

3. Load the new CPG

```scala
ocular> loadCpg("tarpit/target/servlettarpit.war")
```

## Identifying Dependencies

You use ShiftLeft Ocular to ask basic questions about tarpit by running queries, specifically to identify all dependencies associated with the application as a list of version dependent pairs.

```scala
ocular> cpg.dependency.l. 
```

## Importing Convenience Functions

Convenience functions provided by ShiftLeft allow you to take a repeatable value and incorporate it into a function for reuse. Use the following commands to import the convenience functions scripts.sca and scripts.globals.

```
{
   import $file.scripts.sca
   import $file.scripts.globals
}
```
