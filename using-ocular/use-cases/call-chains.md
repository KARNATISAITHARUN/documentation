# Identifying Call Chains

In addition to data flows, call chains can be identified
using scripts and the REPL. This ability is illustrated using 
`commons-io`. 

[Download commons-io](http://central.maven.org/maven2/commons-io/commons-io/2.5/commons-io-2.5.jar) and then generate a Code Property Graph (CPG) by

```
ocular> ./java2cpg.sh -f protobufzip -o commons-io-2.5.bin.zip commons-io-2.5.jar -nb

```

Next, start the REPL and load the CPG

```
$ ./ocular.sh 
ocular> loadCpg("commons-io-2.5.bin.zip") 
```

By searching for interesting methods in `commons-io` 2.5, you find `java.lang.Runtime.exec` 

```
ocular> cpg.method.fullName(".*exec.*").fullName.p 
java.lang.Runtime.exec:java.lang.Process(java.lang.String[])
```

To answer the questions "Where is the data
coming from? and "Can the data be controlled?, find 
the call stack by using the keyword `caller` 

```
ocular> cpg.method.fullName(".*exec.*").caller.fullName.p 
org.apache.commons.io.FileSystemUtils.openProcess:java.lang.Process(java.lang.String[])


ocular> cpg.method.fullName(".*exec.*").caller.caller.fullName.p 
org.apache.commons.io.FileSystemUtils.performCommand:java.util.List<java.lang.String>(java.lang.String[],int,long)


ocular> cpg.method.fullName(".*exec.*").caller.caller.caller.fullName.p 
org.apache.commons.io.FileSystemUtils.freeSpaceUnix:long(java.lang.String,boolean,boolean,long)
org.apache.commons.io.FileSystemUtils.freeSpaceWindows:long(java.lang.String,long)


ocular> cpg.method.fullName(".*exec.*").caller.caller.caller.caller.fullName.p 
org.apache.commons.io.FileSystemUtils.freeSpaceOS:long(java.lang.String,int,boolean,long)
org.apache.commons.io.FileSystemUtils.freeSpaceOS:long(java.lang.String,int,boolean,long)
org.apache.commons.io.FileSystemUtils.freeSpaceOS:long(java.lang.String,int,boolean,long)
org.apache.commons.io.FileSystemUtils.freeSpaceOS:long(java.lang.String,int,boolean,long)

[..]

ocular> cpg.method.fullName(".*exec.*").caller.caller.caller.caller.caller.caller.caller.fullName.p 
org.apache.commons.io.FileSystemUtils.freeSpaceKb:long()
org.apache.commons.io.FileSystemUtils.freeSpaceKb:long()
org.apache.commons.io.FileSystemUtils.freeSpaceKb:long()
org.apache.commons.io.FileSystemUtils.freeSpaceKb:long()


ocular> cpg.method.fullName(".*exec.*").caller.caller.caller.caller.caller.caller.caller.caller.fullName.p
[no results]
```

Even within a small JAR, seven steps are required to find the
"beginning" of the call stack, where no caller is present.
Though it is clear that the data is coming from somewhere along the call stack, it is unknown if the data can be controlled.

Looking into every method in the call stack, and checking if it
consumes the right parameter, is very time consuming.
Therefore the steps `repeat`, `until` and `emit` are introduced. The
following query starts from a method named `exec` and repeats the
method.caller step until it finds a method that is public and
consumes a parameter of type `java.lang.String`.

```
ocular>  cpg.method.name("exec").repeat(method=>method.caller).until(method=> method.isPublic.parameter.evalType("java.lang.String")).emit().fullName.p 
org.apache.commons.io.FileSystemUtils.openProcess:java.lang.Process(java.lang.String[])
org.apache.commons.io.FileSystemUtils.performCommand:java.util.List<java.lang.String>(java.lang.String[],int,long)
org.apache.commons.io.FileSystemUtils.freeSpaceUnix:long(java.lang.String,boolean,boolean,long)
org.apache.commons.io.FileSystemUtils.freeSpaceWindows:long(java.lang.String,long)
org.apache.commons.io.FileSystemUtils.freeSpaceOS:long(java.lang.String,int,boolean,long)
org.apache.commons.io.FileSystemUtils.freeSpaceOS:long(java.lang.String,int,boolean,long)
org.apache.commons.io.FileSystemUtils.freeSpaceOS:long(java.lang.String,int,boolean,long)
org.apache.commons.io.FileSystemUtils.freeSpaceOS:long(java.lang.String,int,boolean,long)
org.apache.commons.io.FileSystemUtils.freeSpaceKb:long(java.lang.String,long)
org.apache.commons.io.FileSystemUtils.freeSpace:long(java.lang.String)
org.apache.commons.io.FileSystemUtils.freeSpaceKb:long(java.lang.String,long)
org.apache.commons.io.FileSystemUtils.freeSpace:long(java.lang.String)
org.apache.commons.io.FileSystemUtils.freeSpaceKb:long(java.lang.String,long)
org.apache.commons.io.FileSystemUtils.freeSpace:long(java.lang.String)
org.apache.commons.io.FileSystemUtils.freeSpaceKb:long(java.lang.String,long)
org.apache.commons.io.FileSystemUtils.freeSpace:long(java.lang.String)
```

This query answers the question "Where is the data
coming from?". And because the methods are public, the data is
potentially controllable. Still, it is unclear if the data
actually flows from the parameter of the methods to the `exec` method.
In order to mark the results of the above query as source, exchange `.fullName.p` with `.parameter`, while the sink is
our `exec` method.

```
ocular> val source = cpg.method.name("exec").repeat(m=>m.caller).until(m=> m.isPublic.parameter.evalType("java.lang.String")).emit().parameter 

ocular> val sink = cpg.method.name("exec").parameter 

ocular> sink.reachableBy(source).flows.p 
```

`org.apache.commons.io.FileSystemUtils.freeSpace:long(java.lang.String)` is found to be publically available, and a data flow between this parameter and `exec` exists.

```
------ Flow with 15 elements ------
path 	 142 	 freeSpace 	 org/apache/commons/io/FileSystemUtils.java
path 	 143 	 freeSpace 	 org/apache/commons/io/FileSystemUtils.java
path 	 259 	 freeSpaceOS 	 org/apache/commons/io/FileSystemUtils.java
path 	 269 	 freeSpaceOS 	 org/apache/commons/io/FileSystemUtils.java
path 	 381 	 freeSpaceUnix 	 org/apache/commons/io/FileSystemUtils.java
path 	 401 	 freeSpaceUnix 	 org/apache/commons/io/FileSystemUtils.java
param1 	  	 <operator>.assignment 	 N/A
param0 	  	 <operator>.assignment 	 N/A
cmdAttribs[2] 	 401 	 freeSpaceUnix 	 org/apache/commons/io/FileSystemUtils.java
cmdAttribs 	 398 	 freeSpaceUnix 	 org/apache/commons/io/FileSystemUtils.java
cmdAttribs 	 473 	 performCommand 	 org/apache/commons/io/FileSystemUtils.java
cmdAttribs 	 484 	 performCommand 	 org/apache/commons/io/FileSystemUtils.java
cmdAttribs 	 537 	 openProcess 	 org/apache/commons/io/FileSystemUtils.java
cmdAttribs 	 538 	 openProcess 	 org/apache/commons/io/FileSystemUtils.java
param0 	  	 exec 	 java/lang/Runtime.java
```
