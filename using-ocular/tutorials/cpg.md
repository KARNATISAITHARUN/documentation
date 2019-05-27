# Using ShiftLeft Ocular with the Code Property Graph (CPG)

This tutorial teaches the basics of navigating the CPG. The tutorial demonstrates how ShiftLeft Ocular tooling can be adapted and extended to suit your specific needs to:

* interactively query the CPG to uncover attack surface
* formulate ad-hoc queries to identify vulnerabilities

ShiftLeft Ocular provides the tooling that vulnerability researchers
require to explore code bases to determine vulnerability
patterns, formulate these patterns in concise and expressive
languages, and persist them. As a result, code can be automatically
scanned for these patterns in the future. 

## Prerequisite

Install ShiftLeft Ocular into your local directory `$shiftleft`. Refer to 
[Installing ShiftLeft Ocular](../installation.md) for more information.

## Running the Hello-ShiftLeft Demo Java Application

This tutorial is based on the demo Java application [Hello-ShiftLeft](https://github.com/ShiftLeftSecurity/HelloShiftLeft). 
Hello-ShiftLeft is a Spring-based Web application that contains
different sample vulnerabilities, including typical injection
vulnerabilities and leakages of sensitive information. 

Focusing on an object deserialization vulnerability in the
`AdminController` 

```java
...
@Controller
public class AdminController {
...
@RequestMapping(value = "/admin/login", method = RequestMethod.POST)
public String doPostLogin(
  @CookieValue(value = "auth", defaultValue = "notset") String auth,
  @RequestBody String password, HttpServletResponse response,
  HttpServletRequest request) throws Exception {
...
if (!auth.equals("notset")) {
   if(isAdmin(auth)) {
     request.getSession().setAttribute("auth",auth);
     return succ;
   }
 }
 ...
}
...
private boolean isAdmin(String auth) {
try {
	ByteArrayInputStream bis = new ByteArrayInputStream(
  	Base64.getDecoder().decode(auth));
	ObjectInputStream objectInputStream = new ObjectInputStream(bis);
	Object authToken = objectInputStream.readObject();
  return ((AuthToken) authToken).isAdmin();
	} catch (Exception ex) {
   	System.out.println(" cookie cannot be deserialized: "
                      +ex.getMessage());
   	return false;
	}
}
...
```

In this code fragment, a cookie is received via HTTP and eventually
deserialized to create a Java object, an optimistic practice that can
often be exploited by attackers for arbitrary code execution. 

## Generating CPGS

Generate a CPG for the `hello-shiftleft.jar`

```bash
cd $shiftleft
./java2cpg.sh subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar -o cpg.bin.zip
```

This command creates a file named `cpg.bin.zip` containing the CPG in a binary format.

# Uncovering Attack Surface with the CPG

The CPG contains information about the processed code
on different levels of abstraction: from dependencies, to type
hierarchies, control flow, data flow, and instruction-level
information. Like the Security Profile, the CPG can be queried interactively using ShiftLeft Ocular or through non-interactive scripts. 

The CPG is loaded via the interactive `loadCpg` command

```scala
loadCpg("cpg.bin.zip")
```

This command creates an object named `cpg`, which provides access to the CPG. To explore the program dependencies

```scala
cpg.dependency.name.l
```

The result is a list of all dependency names. Functional
combinators are supported. For example, to output (name, version) pairs, use
the expression

```scala
cpg.dependency.map(x => (x.name, x.version)).l
```

which yields

```scala
List[(String, String)] = List(
  ("zt-exec", "1.9"),
  ("httpclient", "4.3.4"),
  ("lombok", "1.16.6"),
  ("commons-io", "2.5"),
  ("joda-time", "unknown"),
  ("jasypt", "1.9.2"),
  ("jackson-databind", "unknown"),
  ("spring-boot-starter-web", "unknown"),
  ("jasypt-spring-boot-starter", "1.11"),
  ("spring-boot-starter-test", "unknown"),
  ("spring-web", "unknown"),
  ("hsqldb", "unknown"),
  ("jackson-mapper-asl", "1.5.6"),
  ("spring-boot-starter-actuator", "unknown"),
  ("spring-boot-starter-data-jpa", "unknown"),
  ("logback-core", "1.1.9"),
  ("spring-web", "4.3.6.RELEASE"),
  ("tomcat-embed-websocket", "8.5.11"),
  ...
)
```

It is also possible to process CPG subgraphs using external programs, by
exporting them to JSON. For example 

```scala
cpg.dependency.toJson |> "/tmp/dependencies.json"
```

dumps dependency information into the file /tmp/dependencies.json in
JSON format. Fields of the CPG can be queried using regular
expressions. For example, to determine whether an application uses the
spring framework, a quick query is

```scala
cpg.dependency.name(".*spring.*").l.nonEmpty
=> true
```

Since the application uses Spring, it makes sense to look for the
typical Java annotations that indicate attacker-controlled variables

```scala
cpg.annotation.name(".*(CookieValue|PathVariable).*").l
```

From annotations, jump to parameters using these annotations

```scala
cpg.annotation.name(".*(CookieValue|PathVariable).*").parameter.name.l
```

which yields

```scala
List[String] = List("customerId", "customerId", "customerId", "accountId", "accountId", "accountId", "accountId", "auth", "auth")
```

Now track these attacker-controlled variables to see all data flows originating at them. To do this, first define the set of sinks to be all parameters annotated by CookieValue or PathVariable

```scala
val sources = cpg.annotation.name(".*(CookieValue|PathVariable).*").parameter
```

Then define the set of sinks to be all parameters

```scala
val sinks = cpg.method.parameter
```

Finally, enumerate all flows from sources to sinks

```scala
sinks.reachableBy(sources).flows.p
```

The flows can be examined manually or automatically. For example, determine controlled parameters as a result of data flows as 

```scala
sinks.reachableBy(sources).flows.sink.parameter.l
```

This query determines sinks reachable by sources and examines the corresponding data flows. The last flow element is extracted of each flow via the `pathElemens.last` directive, and the corresponding parameter is retrieved. The result of the query can be stored in a variable for further processing, which comes in handy when determining a large number of data flows

```scala
val controlled = sinks.reachableBy(sources).flows.sink.parameter.l
```

Now retrieve the parameter index ("ast child number" and method full name)

```scala
ocular> controlled.map(x => s"Controlling parameter ${x.order} of ${x.start.method.fullName.l.head}").filterNot(_.contains("<operator>")).sorted
```

yielding

```scala
  "Controlling parameter 0 of java.io.ObjectInputStream.readObject:java.lang.Object()",
  "Controlling parameter 0 of java.lang.String.equals:boolean(java.lang.Object)",
  "Controlling parameter 1 of io.shiftleft.controller.AdminController.isAdmin:boolean(java.lang.String)",
  "Controlling parameter 1 of io.shiftleft.repository.AccountRepository.findOne:java.lang.Object(java.io.Serializable)",
  "Controlling parameter 1 of io.shiftleft.repository.AccountRepository.findOne:java.lang.Object(java.io.Serializable)",
  "Controlling parameter 1 of io.shiftleft.repository.AccountRepository.findOne:java.lang.Object(java.io.Serializable)",
  "Controlling parameter 1 of io.shiftleft.repository.AccountRepository.findOne:java.lang.Object(java.io.Serializable)",
  "Controlling parameter 1 of io.shiftleft.repository.CustomerRepository.delete:void(java.io.Serializable)",
  "Controlling parameter 1 of io.shiftleft.repository.CustomerRepository.exists:boolean(java.io.Serializable)",
  "Controlling parameter 1 of io.shiftleft.repository.CustomerRepository.exists:boolean(java.io.Serializable)",
  "Controlling parameter 1 of io.shiftleft.repository.CustomerRepository.findOne:java.lang.Object(java.io.Serializable)",
  "Controlling parameter 1 of java.io.ByteArrayInputStream.<init>:void(byte[])",
  "Controlling parameter 1 of java.io.ObjectInputStream.<init>:void(java.io.InputStream)",
  "Controlling parameter 1 of java.lang.Long.valueOf:java.lang.Long(long)",
  "Controlling parameter 1 of java.lang.Long.valueOf:java.lang.Long(long)",
  "Controlling parameter 1 of java.lang.Long.valueOf:java.lang.Long(long)",
  "Controlling parameter 1 of java.lang.Long.valueOf:java.lang.Long(long)",
  "Controlling parameter 1 of java.util.Base64$Decoder.decode:byte[](java.lang.String)",
  "Controlling parameter 2 of javax.servlet.http.HttpSession.setAttribute:void(java.lang.String,java.lang.Object)"
...
```

In particular, notice that the instance parameter (with an index of 0)
of the method `ObjectInputStream.readObject` is controlled, that is,
the deserialization vulnerability exists. This shows a more
exploratory way of identifying the vulnerability.
