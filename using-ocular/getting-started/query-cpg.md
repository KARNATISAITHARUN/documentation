# Querying the Code Property Graph (CPG) and Security Profiles

The strength of ShiftLeft Ocular is the ability to examine and investigate your code through the use of queries, especially custom queries which improve the quality of your results.  There are two types of queries: default queries and custom queries.

Additional information is provided in the following articles:

* [Common Queries](https://docs.shiftleft.io/shiftleft/using-shiftleft-ocular/common-queries)
* [Use Cases](https://docs.shiftleft.io/shiftleft/using-shiftleft-ocular/use-cases)
* [Tutorials](https://docs.shiftleft.io/shiftleft/using-shiftleft-ocular/tutorials)
* [Enhancing the OQL](../configure-extend/enhance-oql.md)
* [Policies](https://docs.shiftleft.io/shiftleft/shiftleft-policies)

## Default Queries

[Policies](../../policies/about-policy.md) are made up of default queries written in the [ShiftLeft Policy Language](../../policies/policy-language.md). Policies are used by ShiftLeft to identify how your application communicates with the outside world, which transformations exist on data, and which information flows should be considered security violations. These default queries are executed and the results are incorporated into the [Security Profile](generate-sp.md), which automates code analysis by summarizing the vulnerabilities and data leaks present in code.

## Custom Queries

You use custom queries to improve the quality of your results. Custom queries are written using the [Ocular Query Language (OQL)](https://ocular.shiftleft.io/api/) and running these queries either [interactively or non-interactively](../about/modes.md), or using the ShiftLeft Policy Language to [edit or create custom Policies](../../policies/custom-policy.md).

With OQL custom queries you can:

* [Query the Code Property Graph (CPG)](#querying-the-cpg)
* [Query Security Profiles](#querying-security-profiles)
* [Browse query results](#browsing-query-results)
* [Write query results to a file](#writing-query-results-to-a-file)

### Querying the CPG

Using the OQL, you can run a query on either the active CPG or on all CPGs loaded into memory in your workspace. And you can combine queries from multiple CPGs.

The active CPG is the CPG most recently loaded into your workspace. You query the active CPG using the command 
`cpg`, for example 

```scala
ocular> cpg.method.fullName.l
```

To run a query on all CPGs in your workspace and join the results, use the command `cpgs`, for example

```scala
ocular> cpgs.flatMap(_.method.fullName.l)
```

To combine queries from multiple CPGs use

```scala
cpgs.flatMap{cpg => cpg.method.l }
```

## Querying Security Profiles

The Security Profile must be loaded into memory in order for you to query it using the OQL. Use the commands 

```scala
ocular> cpg.finding.p.<finding>
```
 or 
 
```scala
ocular> cpg.finding.toJsonPretty
``` 

## Browsing Query Results

If your query results are extensive, you can use the following command to open a less-like application (pager) and scroll through the results

```
browse(<query>)
```

where <query> is the actual query whose results you want to browse.

## Writing Query Results to a File

ShiftLeft Ocular uses `.l` (**not** `.p`) to list query results.

Use either of the following commands to write query results to a file, where <query> is the actual query whose results you want to output, for example `cpg.namespace.name`.
  
To write results and create a new file at the same time, use

```
<query>.l |> "out.txt"
```

To append results to an already existing file, use

```
<query>.l |>> "out.txt"
```
