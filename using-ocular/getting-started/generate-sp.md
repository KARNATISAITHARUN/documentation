# Generating and Working with Security Profiles

Security Profiles automate code analysis by summarizing the vulnerabilities and data leaks present in code. It is a high-level abstraction of information flow and can be thought of as a fingerprint of the security posture of your application. Specifically, the Security Profile describes:

* Read operations
* Write operations
* Transformations
* Security-relevant data flows
* Information flows
* Findings

Security Profiles are generated from your application's default [Policy](../../policies/about-policy.md) and [Code Property Graph (CPG)](../about/cpg-deep-dive.md), and are represented as a CPG layer. Security Profiles are updated by queries and [custom Policies](../../policies/custom-policy.md. 

Unlike other default layers, the Security Profile must be explicitly generated. [Security Profiles can be investigated  using ShiftLeft Ocular](query-cpg.md).

The process of working with Security Profiles is:

1. [Generate the Security Profile](#generating-security-profiles).
2. [Load the Security Profile into memory, if needed](#loading-the-security-profile).
3. [Investigate the Security Profile](#investigating-security-profiles).

## Generating Security Profiles

There are three ways to generate a security profile. The Security Profile is created in your workspace and automatically loaded into memory. The `x_securityprofile` file contains the Security Profile in a binary format.


First [create the CPG](create-cpg.md), and then generate the Security Profile, using the command 

```scala
ocular> createSp(cpg)
```

You can also generate the Security Profile at the same time you create your application's CPG, by using the command

```scala
ocular> createCpgAndSp(<inputPath>)
```

Or you can explicitly create the Security Profile as a layer when you create your application's CPG, by

```scala
ocular> createCpg(<inputPath>, List(<layer>))
```

where `<layer>` is one or more layers you want to create; ; multiple layers are separated by a comma. For example, `createCpg(subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar", List("semanticcpg", "tagging", "securityprofile"))`.

## Loading the Security Profile

When you first generate a Security Profile using either the `createSp` or `createCpgAndSp` commands, the Security Profile is automatically loaded into memory. Otherwise, you need to load the externally generated SP in order to work with it, by using the command

```scala
ocular> loadSp(<app.sp>)
```

Once loaded, the Security Profile can be accessed through the CPG. You can determine if a Security Profile is loaded into memory by [viewing the contents of your workspace](manage-workspace.md).

## Investigating Security Profiles

Security Profiles can be investigated [interactively and non-interactively](../about/modes.md) with ShiftLeft Ocular. The Security Profile must be loaded into memory in order for you to investigate it. 
 
You use the Ocular Query Language (OQL) to investigate Security Profiles, using 

```scala
ocular> cpg.finding.p.<finding>
```

Refer for additional information to [Querying the CPG and Security Profiles](query-cpg.md).
