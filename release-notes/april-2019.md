
# ShiftLeft Release Notes - April 2019

* **Update to ShiftLeft's Terms of Service**. The ShiftLeft [Terms of Service](https://www.shiftleft.io/terms/) have been updated for post-termination obligations (4d). 

## ShiftLeft Ocular v0.3.20

* **[Overlays API](https://ocular.shiftleft.io/api/io/shiftleft/repl/cpgcreation/Overlays$.html)**. The Security Profile is now part of the CPG, as an overlay. This new feature unifies the Ocular Query Language (OQL) for the CPG and Security Profile and removes the need for using `cpg2sp.sh` to create a Security Profile. This means that all Security Profile functionality is now part of the CPG. 

* **Integrated CPG and Security Profile Generation**. CPG and Security Profile generation can now be performed from inside ShiftLeft Ocular, with both CPGs and their overlays managed in a workspace. This new feature allows you to effectively work with multiple CPGs at once. [Refer to the API](https://ocular.shiftleft.io/api/io/shiftleft/repl/Workspace.html) for additional information. Older APIs and functionality (e.g. `loadCpg` and `loadSp`) are now backwards compatible.

* **Workspaces**. ShiftLeft Ocular now includes workspaces for easy management of CPGs and overlays. [Refer to the API](https://ocular.shiftleft.io/api/io/shiftleft/repl/Workspace.html) for additional information. 

* **Load Multiple CPG Queries**. You can now load more than one CPG in a given workspace and then combine queries. [Refer to the API](https://ocular.shiftleft.io/api/io/shiftleft/repl/Console.html) for additional information.

* **Deprecated `sp` Object**. The functionality of the deprecated `sp` object has been transferred to `cpg` object.

* **Dependency List Outputs Complete Dependencies**. The Dependency List has been fixed to now output complete dependencies.

* **Renamed `newLocation` Step**. The `newLocation` step has been renamed to location.

* **`.flows` Behavior Shows Single Flow**. `.flows` behavior has been fixed to show only a single flow, thereby resolving printing issues. Use `.allFlows` to show the complete flows list.

## ShiftLeft Inspect and ShiftLeft Protect

* **Enhancements to ShiftLeft Inspect for .NET**. The current version of ShiftLeft Inspect for .NET includes significant performance improvements in both processing time and memory consumption. In addition, numerous bug fixes have been made.

* **JSP Support Added for ShiftLeft Inspect**. You can now use ShiftLeft Inspect to analyze your JSP pages for vulnerabilities and data leakage. Note that this support does not include JSP Expression Language.

* **Stability Improvements to ShiftLeft Protect**. Stability improvements have been made in the ShiftLeft Protect for Java Microagent.

* **Automatic Updates for the ShiftLeft Command Line Interface (CLI)**. The ShiftLeft CLI now automatically updates so that you don't have to reinstall the CLI whenever there are new features or fixes. Refer to [CLI Reference](../using-inspect-protect/using-cli/cli-reference.md) for more information.
