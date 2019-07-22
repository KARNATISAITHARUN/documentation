Based on Chetan's Webinar "How To Find Malicious Backdoors and Business Logic Vulnerabilities in Your Code"

# Attack Using Compiler API

DevOps bake in images with JDK (not hardened) rather than just the JRE. This makes Java compiler API available to compile an 
attack class. Attack class encoded to escape code review.

1. 

globals.getSuspiciousLiterals(cpg)

result literal representing decoded package. This suspicious because why is Java code being encaptsulated as a encoded literal in the code base.

2. Mark encoded literal as a source

val source = cpg.literal.code(".*CGF ...*")

3. File sync, because a particlar file of Java type is being created on the file system and this decoded representation of this encoded literal is persisted.

val sink = cpg.method.fulName(globals.fileSink).parameter

4. 

sinkreachableBy(source).flows.p

flow shows that there is a suspicious pattern

5. If any compiler type construct is used.

cpg.typeDecl.fulName.l.filter(_.contains("Compiler"))

JDK enabled javax.tools.JavaCompiler is being used.

5. Mark type file sync as a source

val source = cpg.method.fulName(globals.fileSink).methodReturn
val sink = cpg.method.fulName(globals.compilerSink).parameter

6. What is the full name of the compiler sync

sink.reachableBy(source).flows.p

Result flow which has a 2nd suspicious element 

7. If this pass is saved, is it being compiled and instantiated

val source = cpg.method.fulName(".*URLClassLoader.newInstance.*").methodReturn
val source = cpg.method.fulName(".*Class.forName.*").parameter
sink reachableBy(source).flows.p

result flow which has a third suspicious patterm

By using Ocular you have separately inquired about each suspicious pattern and then binded them together to see all three conditions had flows with an intrinsic connection.
