# Quick Start for ShiftLeft Ocular

You use ShiftLeft Ocular to navigate and interactively query the **Code Property Graph (CPG)** to analyze and identify your application's attack surface and vulnerabilities.

The philosophy behind ShiftLeft's solution is that creating a "one-fits-all" vulnerability scanner borders on the impossible. Instead, ShiftLeft Ocular can be adapted and extended to suit your specific needs. ShiftLeft Ocular provides vulnerability researchers with the ability to explore code bases to determine vulnerability patterns, formulate these patterns in a concise and expressive language and persist them. Code can then be automatically scanned for these patterns in the future. 

The process for quickly starting with ShiftLeft Ocular is:

1. [Install ShiftLeft Ocular](#installing-ocular).
   
2. [Specify the target application](#specifying-the-target-application).

3. [Generate the CPG](#generating-the-cpg).

4. [Uncover Attack Surface](#uncovering-attack-surface).

The demo Java application [HelloShiftLeft](https://github.com/ShiftLeftSecurity/HelloShiftLeft) is used in this Quick Start. It is a Spring-based Web application that contains different sample vulnerabilities, including typical injection
vulnerabilities and leakages of sensitive information. The focus of the Quick Start is an object deserialization vulnerability in the`AdminController`.

ShiftLeft Ocular can also be used to navigate the Security Profile, which is part of the CPG as an overlay. 

## Installing Ocular

ShiftLeft Ocular runs on top of the Java Virtual Machine (JVM). Please make sure you have a Java Runtime Environment >= 1.8 installed.

Begin by decompressing the provided ZIP file `ocular-distribution.zip`. This creates the directory `ocular-distribution`.

```bash
unzip ocular-distribution.zip
cd ocular-distribution
```

Run the installer and follow the prompts:

```bash
bash ./install.sh
```

The install script:

* asks you for the install location (defaults to `~/bin/ocular`).
* checks if there is an existing installation, and if so, offers to delete it.
* unpacks the ShiftLeft dynamic policy to `~/.shiftleft/policy/dynamic`, and if it already exists, offers to delete it.
* unpacks the ShiftLeft static policy to `~/.shiftleft/policy/static`, and if it already exists, offers to delete it.

Do not make any changes outside the installation and policy directories.

### Additional Configuration for Large Projects

Code analysis can require a large amount of memory, and unfortunately the JVM does not automatically allocate the necessary amount of memory. While tuning Java memory usage is a discipline in its own right, it is usually sufficient to specify the maximum available amount of heap memory via the Java virtual machine's `-Xmx` flag. The easiest way to achieve this globally is by setting the environment variable `_JAVA_OPTS` as:

```bash
export _JAVA_OPTS="-Xmx$NG"
```
where `$N` is the amount of memory in gigabytes. You can add this line to your shell startup script, e.g., `~/.bashrc` or `~/.zshrc`.

**Note** This option affects all subsequent JVM instances started on the machine.

In order to restrict the JVM option to only the parts of ShiftLeft Ocular that you intend to use, you can also provide the memory allocation individually. For example, to set 12 GB available heap memory to `java2cpg` or `ocular`, use this option directly:

```bash
./ocular.sh -J-Xmx12g
./java2cpg.sh -J-Xmx12g <...>
```

## Specifying the Target Application

After installing ShiftLeft Ocular, you must specify the target application. In this tutorial, we are using the Demo Application Hello ShiftLeft, which contains this code in particular:

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

## Generating the CPG

To generate the CPG for the `hello-shiftleft-0.0.1-SNAPSHOT.jar`:

```bash
cd $shiftleft
./ocular.sh
createCpg("subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar")
```

This command creates a CPG for the HelloShiftLeft application and stores it in the workspace. Issuing the command `workspace` shows a workspace entry for the newly created CPG.

```
workspace
 | name                              | overlays                 | loaded|
 |======================================================================|
 | hello-shiftleft-0.0.1-SNAPSHOT.jar| semanticcpg(l),tagging(l)| true  |
```

As the workspace overview shows, the CPG is automatically loaded on creation. The CPG that was loaded last is accessible using the variable `cpg`.

## Uncovering Attack Surface

The CPG contains information about the processed code on different levels of abstraction: dependencies, type
hierarchies, control flow, data flow, and instruction-level information. The CPG can be queried interactively using 
ShiftLeft Ocular or via non-interactive scripts. Using interactive querying, explore the program dependencies by:

```scala
cpg.dependency.name.l
```

A complete list of all dependency names is returned. ShiftLeft Ocular supports functional combinators. For example, to output (name, version) pairs, use the following expression:

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

It is also possible to process CPG subgraphs using external programs by exporting them to JSON. For example, the command 

```scala
cpg.dependency.toJson |> "/tmp/dependencies.json"
```

dumps complete dependency information into the file "/tmp/dependencies.json" in JSON format. 

Fields of the CPG can be queried using regular expressions. So, to determine whether an application uses the
Spring framework, a quick query is

```scala
cpg.dependency.name(".*spring.*").l.nonEmpty
=> true
```

Since the HelloShiftLeft application uses Spring, it makes sense to look for the typical Java annotations that indicate attacker-controlled variables 

```scala
cpg.annotation.name(".*(CookieValue|PathVariable).*").l
```

From annotations, you can look at the parameters using

```scala
cpg.annotation.name(".*(CookieValue|PathVariable).*").parameter.name.l
```

which yields

```scala
List[String] = List("customerId", "customerId", "customerId", "accountId", "accountId", "accountId", "accountId", "auth", "auth")
```

Tracking these attacker-controlled variables shows all data flows originating at them. To do this, first define the set of sinks to be all parameters annotated by CookieValue or PathVariable

```scala
val sources = cpg.annotation.name(".*(CookieValue|PathVariable).*").parameter
```

then define the set of sinks to be all parameters

```scala
val sinks = cpg.method.parameter
```

Finally, enumerate all flows from sources to sinks

```scala
sinks.reachableBy(sources).flows.p
```

The flows can be examined manually or automatically. For example, to determine parameters controlled as a result of data flows

```scala
sinks.reachableBy(sources).flows.sink.parameter.l
```

The query determines those sinks reachable by sources and examines the corresponding data flows. The last flow element is extracted of each flow through the `pathElemens.last` directive, and the corresponding parameter is retrieved. The result of the query can be stored in a variable for further processing, which is useful when determining a large number of data flows

```scala
val controlled = sinks.reachableBy(sources).flows.sink.parameter.l
```

Now retrieve the parameter index ("ast child number" and method full name)

```scala
ocular> controlled.map(x => s"Controlling parameter ${x.order} of ${x.start.method.fullName.l.head}").filterNot(_.contains("<operator>")).sorted
``

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

In particular, notice that the instance parameter (with an index of 0) of the method `ObjectInputStream.readObject` is controlled, that is, the deserialization vulnerability exists. This shows a more exploratory way of identifying the vulnerability.
