# Scanning for Deserialization Sinks in Java

OWASP provides a cheat sheet for typical deserialization sources:

https://www.owasp.org/index.php/Deserialization_Cheat_Sheet#Java

Search for references of these methods by

```
ocular> val sinkMethods = cpg.method.or(
      _.fullName(".*(XMLdecoder|ObjectInputStream).*readObject.*"),
      _.fullName(".*XStream.*fromXML.*"),
      _.fullName(".*readObjectNodData|readResolve|readExternal.*"),
      _.fullName(".*ObjectInputStream.*readUnshared.*"))

ocular> sinkMethods.calledBy(cpg.method).newCallChain.p
```
The OWASP guide also suggests hardening classes that derive from
`Serializable`. Use ShiftLeft Ocular to identify classes that directly
inherit from `Serializable`

```
ocular> cpg.typeDecl.name("Serializable").derivedTypeDecl.fullName.l
```

For classes that inherit from `Serializable` either directly or
indirectly, use the query
```
ocular> cpg.typeDecl.name("Serializable").derivedTypeDeclTransitive.fullName.l
```

In the context of deserialization vulnerabilities, it may also be
interesting to review the versions of libraries. As an example, to
determine the version of the "XStream" library, issue the query

```
ocular> cpg.dependency.name(".*xstream.*").version.l
```
