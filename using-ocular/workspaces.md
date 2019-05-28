# Using Workspaces 

ShiftLeft Ocular uses workspaces to help you manage Code Property Graphs (CPGs) and their overlays, simultaneously work with multiple CPGs, and combine queries from multiple CPGs.

Within a workspace you can create CPGs, augment CPGs with overlays, load CPGs into the workspace, remove (unload) CPGs from the workspace, and delete CPGs. By default, all operations are executed on the CPG that was last loaded into the workspace.

The active, last loaded CPG is accessible through the `cpg` variable; all CPGs loaded in the workspace are accessible using the `cpgs` variable.

In order to create CPGs that contain the application code but do not include the code of libraries, ShiftLeft Ocular performs a smart inspection of the input file(s) to determine application code vs dependencies. Refer to the [API for information on the methods used to gain visibility into this process: `namespaces`, `appNamespaces` and `depNamespaces`](https://ocular.shiftleft.io/api/io/shiftleft/repl/Console.html).

ShiftLeft Ocular offers the following features to use workspaces:

* [Creating and Loading CPGs and Security Profiles](#creating-and-loading-cpgs-and-security-profiles)
* [Managing a Workspace](#managing-a-workspace)
* [Managing Overlays](#managing-overlays)
* [Identifying Application Code and Dependencies](#identifying-application-code-and-dependencies)

## Creating and Loading CPGs and Security Profiles

There are three ways to create CPGs and Security Profiles, illustrated using the Java Vulnerable Lab benchmarking application for vulnerability discovery tools.
 
 1. Via shorthand.
 
 ```
 createCpgAndSp("subjects/JavaVulnerableLab.war")
 ```

2. First creating the CPG, then creating the Security Profile.

```
createCpg("subjects/JavaVulnerableLab.war")
createSp
```

3. By explicitly specifying the overlays.

```
createCpg("subjects/JavaVulnerableLab.war", "semanticcpg", "tagging", "securityprofile")
```

## Managing a Workspace

Managing a workspace involves loading and removing (unloading CPG and running queries. The commands to manage a workspace are illustrated using the HelloShiftLeft demo Java application.

* `workspace`. Load all of an application's CPGs in the workspace. The last entry is the active CPG.

* `unloadCpg`. Remove (unload) the newly created CPG from the workspace.

* `loadCpg("hello-shiftleft-0.0.1-SNAPSHOT.jar")`. Load a specific CPG, by name, in the workspace. Once loaded, this CPG is  active.

* `cpg.method.fullName.l`. Run query on the active CPG in the workspace.

* `cpgs.flatMap(_.method.fullName.l)`. Run query on all CPGs in the workspace and join results.

## Managing Overlays

You manage overlays by showing all overlay creators, loading a CPG only with a `semanticcpg` overlay and adding a tagging overlay. The commands to manage overlays are illustrated using the Java Vulnerable Lab application.

* `overlays`. Show all available overlay creators.

* `loadCpg("JavaVulnerableLab.war", "semanticcpg")`. Load a CPG only with the `semanticcpg` overlay.

* `addOverlay("tagging")`. Add the tagging overlay.

## Identifying Application Code and Dependencies

Identifying application code and dependencies is done by showing all namespaces and showing only either application namespaces or dependency namespaces. The commands to identify application code and dependencies are illustrated using the HelloShiftLeft demo Java application.

* `namespaces("subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar")`. Show all namespaces of the application.

* `appNamespaces("subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar")`. Show application namespaces.

* `depNamespaces("subjects/hello-shiftleft-0.0.1-SNAPSHOT.jar")`. Show dependency namespaces.
