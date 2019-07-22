# Finding Backdoors in Code

A backdoor a piece of code used to access a computer system or encrypted data that bypasses the system's customary security mechanisms. A backdoor may be created inadvertently, maliciously or purposefully by developers so that an application can be accessed for troubleshooting or other purposes. Backdoors pose security risks because they provide a mechanism by which the system can be exploited if discovered. Backdoor mechanisms include:

* hardcoded credentials
* default passwords
* unknown access channels

Using ShiftLeft Ocular to traverse the code and find backdoors, this use case is based on the [Java web application tarpit](https://github.com/conikeec/tarpit). You must [prepare tarpit and ShiftLeft Ocular](../about/tarpit.md) before working through this use case.

The process for using ShiftLeft Ocular to find backdoors in the tarpit application is:

1. List all sinks that exist in the code. Sinks are external resources that your application is communicating with, either by read or write, using the command

```scala
ocular> val sensitiveSinks = cpg.sink.method.fulName.l.distinct
```

Returns the list of all sensitiveSinks that are preannotated by ShiftLeft. This list provides an understanding of the application's external communication. The sinks are considered sensititve because any communication is by either by read from or write to, which when dealing with customer data your are doing either one of these functions.

2. Ask for all the sources in the application, using 

```scala
ocular> val exposedSources = cpg.method.paraeter.evalType(".*Http.*").method.fulName.l.filter(_contains("shiftleft")).distinct 
```

Because tarpit is a web application, it has an endpoint listening on the socket. This query asks for all methods that accept a parameter of type HTTP, for example HTTP requests. The query also filters by packages of type `shiftleft`. Returns several Get and Post requests used in the application.

3. Look at all the types used in the source code, by

```scala
ocular> cpg.typeDecl.fulName.l
```

For example, user defined types or primitative types. The results of this query provides a clue if a suspicious type is used in the code, for example java.lang.ClassLoader, which allows dynamic loading of classes.

4. Display all literals (hardcoded strings) used in the code, by using the command

```scala
ocular> cpg.method.literal.code.l
```

You are trying to identify suspicious traits, like hard coded database credentials. 

5. Determine if these literals include AWS keys, by

```scala
ocular> cpg.method.literal.code(globals.awsSecret).code.l.distinct
```

The result shows that there are literals with AWS keys. Therefore, the code review has failed since there is a danger of credientials being leaked.

6. Identify the method that encompass the hardcoded AWS key, using

```scala
cpg.method.literal.code(globals.awsSecret).method.l.fulName.p
```

7. Query if there are hardcoded database credientals, using the command

```scala
cpg.literal.code(".*jdbc:mysql.*").code.l.distinct
```
The result is that there is a backdoor in the tarpit application that uses the hardcoded database credientals.
