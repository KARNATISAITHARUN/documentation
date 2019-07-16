# Quick Start for ShiftLeft Ocular

You use ShiftLeft Ocular to examine the software elements and flows in your application to identify complex business logic vulnerabilities that can't be scanned for automatically. 

The process for quickly starting with ShiftLeft Ocular is:

1. [Install ShiftLeft Ocular](#installing-shiftleft-ocular).
   
2. [Start ShiftLeft Ocular](#starting-shiftleft-ocular).

3. [Create the Code Property Graph (CPG)](#creating-the-code-property-graph).

4. [Explore Your Workspace](#exploring-your-workspace).

5. [Uncover the Attack Surface](#uncovering-the-attack-surface).

Make sure you have met all [ShiftLeft Ocular requirements](../../introduction/requirements.md).


### The HelloShiftLeft Demo Application

The demo Java application [HelloShiftLeft](https://github.com/ShiftLeftSecurity/HelloShiftLeft) is used as an example in this Quick Start. It is included in the ShiftLeft Ocular distribution. When you install ShiftLeft Ocular, HelloShiftLeft is automatically placed in the `subjects` subdirectory. The file is `hello-shiftleft-0.0.1-SNAPSHOT.jar`.

HelloShiftLeft is a Spring-based Web application that contains different sample vulnerabilities, including typical injection
vulnerabilities and leakages of sensitive information. The focus of the Quick Start is an object deserialization vulnerability in the`AdminController`:

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

In this code fragment, a cookie is received via HTTP and eventually deserialized to create a Java object, an optimistic practice that is often exploited by attackers for arbitrary code execution.

## Installing ShiftLeft Ocular

Refer to the article [Installing ShiftLeft Ocular](installation.md) for details.

## Starting ShiftLeft Ocular

Information on [Starting ShiftLeft Ocular](starting.md) for each supported operating system is provided.

## Creating the Code Property Graph

To create the CPG for HelloShiftLeft, use the command

```
ocular> createCpg("subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar")
```

where `subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar` is the location and filename of the target application.

The CPG for the application is created and loaded into your workspace.

## Exploring Your Workspace

Workspaces help you manage CPGs and their layers, simultaneously work with multiple CPGs, and combine queries from multiple CPGs. 

When you create a CPG, it is automatically loaded into your workspace. Existing CPGs can also be loaded into your workspace, and removed or deleted from your workspace. By default, all operations are executed on the CPG that was last loaded into the workspace; this is the active CPG.

Issuing the command `workspace` shows a workspace entry for the newly created CPG.

```
workspace
 | name                              | overlays                 | loaded|
 |======================================================================|
 | hello-shiftleft-0.0.1-SNAPSHOT.jar| semanticcpg(l),tagging(l)| true  |
 ```

## Uncovering the Attack Surface

The CPG contains information about the processed code on different levels of abstraction: dependencies, type
hierarchies, control flow, data flow, and instruction-level information. The CPG can be queried using 
ShiftLeft Ocular in either [interactive or non-interactive modes](../about/modes.md). 

Using interactive querying, explore the program dependencies by

```scala
ocular> cpg.dependency.name.l
```

A complete list of all dependency names is returned. Since the Ocular Query Language (OQL) is a Scala-based DSL, it also  supports functional combinators. For example, to output (name, version) pairs, use the following expression

```scala
ocular> cpg.dependency.map(x => (x.name, x.version)).l
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

It is also possible to process CPG subgraphs using external programs by exporting them to JSON. For example, the command 

```scala
ocular> cpg.dependency.toJson |> "/tmp/dependencies.json"
```

dumps complete dependency information into the file "/tmp/dependencies.json" in JSON format. 

Fields of the CPG can be queried using regular expressions. So, to determine whether an application uses the
Spring framework, a quick query is

```scala
ocular> cpg.dependency.name(".*spring.*").l.nonEmpty
=> true
```

Since the HelloShiftLeft application uses Spring, it makes sense to look for the typical Java annotations that indicate attacker-controlled variables 

```scala
ocular> cpg.annotation.name(".*(CookieValue|PathVariable).*").l
```

From annotations, look at the parameters using

```scala
ocular> cpg.annotation.name(".*(CookieValue|PathVariable).*").parameter.name.l
```

which yields

```scala
List[String] = List("customerId", "customerId", "customerId", "accountId", "accountId", "accountId", "accountId", "auth", "auth")
```

Tracking these attacker-controlled variables shows all data flows originating at them. To do this, first define the set of sinks to be all parameters annotated by CookieValue or PathVariable

```scala
ocular> val sources = cpg.annotation.name(".*(CookieValue|PathVariable).*").parameter
```

then define the set of sinks to be all parameters

```scala
ocular> val sinks = cpg.method.parameter
```

Finally, enumerate all flows from sources to sinks

```scala
ocular> sinks.reachableBy(sources).flows.p
```

The flows can be examined manually or automatically. For example, to determine parameters controlled as a result of data flows

```scala
ocular> sinks.reachableBy(sources).flows.sink.parameter.l
```

The query determines those sinks reachable by sources and examines the corresponding data flows. The last flow element is extracted of each flow through the `pathElemens.last` directive, and the corresponding parameter is retrieved. The result of the query can be stored in a variable for further processing, which is useful when determining a large number of data flows

```scala
ocular> val controlled = sinks.reachableBy(sources).flows.sink.parameter.l
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
```

In particular, notice that the instance parameter (with an index of 0) of the method `ObjectInputStream.readObject` is controlled, that is, the deserialization vulnerability exists. This shows a more exploratory way of identifying the vulnerability.
