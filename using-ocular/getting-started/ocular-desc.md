# About ShiftLeft Ocular

ShiftLeft Ocular is a set of command line tools that provide a static analysis framework for application security. Developers and security researchers use ShiftLeft Ocular to explore code bases to determine vulnerabilities. This is done by  formulating queries in the Ocular Query Language (OQL), a concise and expressive language. Queries can be persisted, allowing code to be automatically scanned for the same vulnerabilities in the future. And ShiftLeft Ocular can be adapted and extended to suit your specific needs.

You can download a [trial version (Java-only)](https://go.shiftleft.io/ocular-free-trial) of ShiftLeft Ocular. The trial  version has a limited feature set.

ShiftLeft Ocular provides the following core features:

* **Generation of Code Property Graphs (CPG).** (Included in the ShiftLeft Ocular trial version). Based on your application code, ShiftLeft Ocular generates a CPG that can be used as the basis of custom static analysis. CPGs provide an extensible, multi-layered representation of each unique code version. 

* **Extensibility**. ShiftLeft Ocular is highly extensible through the use of custom queries and [Policies](../../policies/spl.md), by [modifying the OQL language](../tutorials/sp.md#customizing-the-dsl-using-the-extend-my-library-pattern), and by [creating a CPG pass](https://ocular.shiftleft.io/api/io/shiftleft/passes/index.html) and introducing new layers into the CPG.

* **[Interactive and Non-Interactive Modes](../interactive-noninteractive-modes.md)**. (Included in the ShiftLeft Ocular trial version). CPGs and Security Profiles can be interactively explored using a REPL, an interactive shell with support for the OQL. And user-provided REPL scripts can be executed non-interactively to perform automated custom scans for security issues. 

* **Automatic Code Scanning through Security Profiles.** Security Profiles are summaries of vulnerabilities and data leaks present in the code. The Security Profile is a [layer](https://ocular.shiftleft.io/api/io/shiftleft/repl/cpgcreation/Overlays$.html) of the CPG, and can be explored and processed using the REPL, and managed in your workspace.

* **Framework and Library Support through Policies.** ShiftLeft Ocular includes Policies, which are annotations for common Java frameworks and libraries and are used to create Security Profiles from the CPG. Possibly attacker-controlled data sources and interesting sinks are automatically tagged in the CPG, and flow descriptions exist to scan for common vulnerabilities. You can provide additional annotations to extend supported frameworks and libraries or encode additional vulnerability patterns.
  
* **Workspaces.** When you install ShiftLeft Ocular, your workspace is automatically created. A workspace is a directory containing all data persisted by ShiftLeft Ocular. Workspaces help you manage Code Property Graphs (CPGs) and their layers, simultaneously work with multiple CPGs, and combine queries from multiple CPGs.
