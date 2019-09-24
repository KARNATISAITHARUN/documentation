# About ShiftLeft Policies

Policies are used by ShiftLeft to identify how your application communicates with the outside world, which transformations exist on data, and which information flows should be considered security violations. Policies are also used as the basis for Security Profiles, which automate code analysis by summarizing the vulnerabilities and data leaks present in code.

ShiftLeft includes default Policies that specify the most common types of vulnerabilities. In addition, you can [create custom Policies](custom-policy.md) to introduce desired security semantics, such as additional knowledge about, or to exclude parts of a default Policy that does not apply to, your application.

Policies are written in the ShiftLeft [Policy Language](policy-language.md).

## Types of ShiftLeft Policies

There are two types of Policies:
 - Library Policies
 - Application Policies

Library Policies are applied as needed, when ShiftLeft ascertains that a particular library is used in your application. Library Policies define generic information flow that is independent of the application code.

Application Policies define application-specific security violations or transformations. ShiftLeft either selects a default Application Policy based on the code language, or uses the custom Policy you have specified.

## Policies and ShiftLeft Ocular

Policies are identified by the filename extension `.policy` and are stored in the ShiftLeft Ocular directory

```
~/bin/ocular/policy
```

When you [create and work with a CPG using ShiftLeft Ocular](../using-ocular/getting-started/create-cpg.md), the appropriate Library and Application Policies are automatically loaded and assigned.

Policies are stored based on their namespace information. For example, a Policy for
a class from a Java Standard Library, `java.io.ObjectInputStream`, is available as

```
~/bin/ocular/policy/dynamic/java/io/ObjectInputStream.policy
```

## Policies and ShiftLeft Inspect

Policies for ShiftLeft Inspect are located in the ShiftLeft repository. Using the [Command Line Interface (CLI)](../using-inspect-protect/using-cli/cli-reference.md#sl-policy-commands), you can

* View a Policy
* Upload a Policy
* Manage default Policies

Policies are named in the repository based on the namespace information. For example, a Policy for
a class from a Java Standard Library, `java.io.ObjectInputStream`, is available in ShiftLeft's repository as

```
java.io/ObjectInputStream
```

## The Default ShiftLeft Policy

The default ShiftLeft Policy contains directives that define and identify:

* Exposed methods, interface interactions and transformations

* Information flows, particularly data leaks

* Taint semantics

Policies use a generic set of sensitive variable names, which directly affect the security results you see. To illustrate the default Policy, take a look at how deserialization vulnerabilities are identified. The following directive

```
// A policy for java.io.ObjectInputStream class

IO deserialization = METHOD -f "java.io.ObjectInputStream.readObject:java.lang.Object()" { INST "SINK" }
```

specifies that the instance parameter of the method `readObject` should be considered as a data sink of a deserializer, that is, the instance parameter is deserialized.

This directive determines that all parameters tagged with the Spring annotations `CookieValue`, `PathVariable`, and a few others are to be tagged as "attacker-controlled". 

```
// A generic policy for org.springframework.web.bind.annotation.* classes

TAG "DATA_TYPE" -v "attacker-controlled" METHOD -a r"org\.springframework\.web\.bind\.annotation\.(Request|Get|Post|Put|Delete|Patch)Mapping" PAR -a r"org\.springframework\.web\.bind\.annotation\.(CookieValue|PathVariable|RequestParam|RequestBody)"
```

And this directive specifies that flows of `attacker-controlled` data into `deserializers` are worth reporting

```
// An Application Policy describing execution vulnerabilities

CONCLUSION attacker-to-deserializer = FLOW DATA (attacker-controlled) -> IO (deserializer)
WHEN CONCLUSION attacker-to-deserializer => EMIT {
    title: "Attacker controlled data to deserialization",
    description: "Attacker controlled data is deserialized in this flow. ...",
    category: "a1-injection",
    score: "8.0"
}
```

In this directive, the Policy also allows data transformations and checks to be identified in order to report flows of data, for data that does not undergo validation. 

Refer to the ShiftLeft [Policy Language](policy-language.md) for additional information and examples.
