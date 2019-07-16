# ShiftLeft Products

ShiftLeft is an application security platform with three solutions: 

* **[ShiftLeft Ocular](../using-ocular/about/ocular-features.md)**. Helps examine the software elements and flows in your application to identify complex business logic vulnerabilities that can't be scanned for automatically. ShiftLeft Ocular gives code auditors and reviewers the ability to construct and tune powerful, highly customized queries for interactive interrogation of your unique code bases and environments.

* **[ShiftLeft Inspect](../using-inspect-protect/inspect/analyzing-applications-in-ci.md)**. A next-generation static application security testing (SAST) solution. In just minutes, ShiftLeft Inspect provides an accurate and exhaustive exploration and analysis of your unique code and identifies complex vulnerabilities and sensitive data leakage.

* **[ShiftLeft Protect](../using-inspect-protect/protect-java/jvm-based-environments.md)**. ShiftLeft Protect secures applications against exploitation of vulnerabilities by leveraging "code informed" insights. ShiftLeft Protect creates runtime security specifications based on identified vulnerabilities discovered during analysis of code. This approach allows ShiftLeft Protect to operate with minimal footprint and overhead. 

Components of the ShiftLeft solution are:

* **[Code Property Graph (CPG)](understanding-cpg.md)**. All ShiftLeft solutions leverage the CPG. In a single graph, the CPG provides an extensible, multi-layered representation of each code version, including the various levels of abstraction. The unique insights from the CPG provide all ShiftLeft solutions with granular detail and a deep understanding of data flows.

* **[ShiftLeft Command Line Interface (CLI)](../using-inspect-protect/using-cli/using-cli.md)**. The interface to ShiftLeft Inspect and ShiftLeft Protect via the command line. ShiftLeft CLI is used with ShiftLeft Inspect to submit applications for analysis and with ShiftLeft Protect to monitor your applications in runtime. 

* **[ShiftLeft Dashboard](../using-inspect-protect/using-workflow/vulnerability-dashboard.md)**. Your visual interface with ShiftLeft Inspect and ShiftLeft Protect. Shows information on the vulnerabilities of analyzed applications and the monitoring of the security of these applications in production. It also allows you to manage your ShiftLeft account.

* **[ShiftLeft Microagent](../using-inspect-protect/protect-java/configuring-the-microagent.md)**. ShiftLeft Protect deploys a ShiftLeft Microagent in-memory alongside your runtime application. The Microagent protects the residual issues in production and is customized to your application's specific shape and weaknesses through the use of a Security Profile for Runtime (SPR). Each Microagent connects to a ShiftLeft Proxy server to download the SPR and to send events and metrics to the Dashboard. 

* **[Ocular Query Language (OQL)](https://ocular.shiftleft.io/api/)**. Use to query CPGs and SPRs. OQL results are available and exportable via standard JSON format for easy integration into the security tools in use by your organization and for sharing data across the SDLC.
