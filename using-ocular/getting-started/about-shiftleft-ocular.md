# About ShiftLeft Ocular

ShiftLeft Ocular is a set of command line tools that provide a static analysis framework for application security. Developers and security researchers use ShiftLeft Ocular to explore code bases to determine vulnerabilities. This is done by formulating queries in the Ocular Query Language (OQL), a concise and expressive language. Queries can be persisted, allowing code to be automatically scanned for the same vulnerabilities in the future. And ShiftLeft Ocular can be adapted and extended to suit your specific needs.

You can download a [trial version (Java-only)](https://go.shiftleft.io/ocular-free-trial) of ShiftLeft Ocular. The trial  version has a limited feature set.

ShiftLeft Ocular provides the following core features:

* **Generation of [Code Property Graphs (CPG)](../../introduction/understanding-cpg.md).** (Included in the ShiftLeft Ocular trial version). Based on your application code, ShiftLeft Ocular generates a CPG that can be used as the basis of custom static analysis. CPGs provide an extensible, multi-layered representation of each unique code version.

* **[Framework and Library Support through Policies](../../policies/spl.md).** ShiftLeft Ocular includes Policies, which are annotations for common Java frameworks and libraries. Possibly attacker-controlled data sources and interesting sinks are automatically tagged in the CPG, and flow descriptions exist to scan for common vulnerability patterns. You can provide additional annotations to extend supported frameworks and libraries or encode additional vulnerability patterns.

* **Extensibility**. ShiftLeft Ocular is highly extensible through the use of custom queries and Policies, by modifying the OQL language, and by [creating a CPG pass](https://ocular.shiftleft.io/api/io/shiftleft/passes/index.html) and introducing new layers into the CPG.

* **[Interactive and Non-Interactive Modes](modes.md)**. (Included in the ShiftLeft Ocular trial version). CPGs and Security Profiles can be interactively explored using a REPL, an interactive shell with support for the OQL. And user-provided REPL scripts can be executed non-interactively to perform automated custom scans for security issues.

* **Automatic Code Scanning through Security Profiles.** Security Profiles are summaries of vulnerabilities and data leaks present in the code. The Security Profile is part of the CPG, as a [layer](https://ocular.shiftleft.io/api/io/shiftleft/repl/cpgcreation/Overlays$.html). Security Profiles can be explored and processed using the REPL and managed in a workspace.
  
* **Workspaces for Managing CPGs and Security Profiles.** A workspace is a container for managing CPGs and SPs, containing all data persisted by ShiftLeft Ocular. Workspaces allow you to easily manage CPGs and Security Profiles, load and simultaneously work with more than one CPG in a given workspace, and combine queries. 
