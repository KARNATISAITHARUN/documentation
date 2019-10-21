# ShiftLeft Ocular Features

ShiftLeft Ocular is a set of command line tools that provide a static analysis framework for application security. Developers and security researchers use ShiftLeft Ocular to explore code bases to determine vulnerabilities. This is done by formulating queries in the Ocular Query Language (OQL), a concise and expressive language. Queries can be persisted, allowing code to be automatically examined for the same vulnerabilities in the future. And ShiftLeft Ocular can be adapted and extended to suit your specific needs.

You can download a [trial version (Java-only)](https://go.shiftleft.io/ocular-free-trial) of ShiftLeft Ocular. The trial  version has a limited feature set.

ShiftLeft Ocular provides the following core features:

* **[Creation of Code Property Graphs (CPG)](../getting-started/create-cpg.md)** (Included in the ShiftLeft Ocular trial version). Based on your application code, ShiftLeft Ocular creates a CPG that can be used as the basis of custom static analysis. CPGs provide an extensible, multi-layered representation of each unique code version.

* **[Framework and Library Support through Policies](../../policies/about-policy.md).** ShiftLeft Ocular includes Policies, which are annotations for common frameworks and libraries. Policies are used to find data flows in your application code, and to [generate and work with Security Profiles](../getting-started/generate-sp.md). You can customize Policies to extend supported frameworks and libraries or to encode additional vulnerability patterns.

* **Extensibility**. ShiftLeft Ocular is highly extensible through the use of [custom queries](../getting-started/query-cpg.md) and [Policies](../../policies/custom-policy.md), by [enhancing the OQL](../configure-extend/enhance-oql.md), and by creating a CPG pass and introducing new layers into the CPG.

* **[Interactive and Non-Interactive Modes](modes.md)** (Included in the ShiftLeft Ocular trial version). CPGs and Security Profiles can be interactively explored using a REPL, an interactive shell with support for the OQL. And user-provided REPL scripts can be executed non-interactively to perform automated custom scans for security issues.

* **[Automatic Code Scanning through Security Profiles](../getting-started/generate-sp.md).** Security Profiles are dervied from Policies, and summarize the  vulnerabilities and data leaks present in the code. The Security Profile is part of the CPG, as a [layer](layers.md). 
  
* **[Workspaces for Working with CPGs and Security Profiles](../getting-started/manage-workspace.md).** A workspace is a container for all data persisted by ShiftLeft Ocular. Workspaces allow you to easily work with CPGs and Security Profiles, load and simultaneously work with more than one CPG, and combine queries. 
