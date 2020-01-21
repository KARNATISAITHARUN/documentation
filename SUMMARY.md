# Summary

* [What is ShiftLeft?](README.md)

## Quickstart

  * [For Linux and macOS Users](/quickstarts/linux-macos.md)
  * [For Windows Users](/quickstarts/windows.md)

## Inspect

* [Getting Started](/inspect/getting-started/README.md)
  * [Linux/macOS](/inspect/getting-started/linux-macos.md)
  * [Windows](/inspect/getting-started/windows.md)

### Analyzing Applications

* [C#](/inspect/analyzing-applications/c-sharp.md)

* Languages
  * [Supported Languages](languages/language-support.md)
  * [C](languages/c.md)
  * [C++](languages/c-plus-plus.md)
  * [C#](languages/c-sharp.md)
  * [Golang](languages/golang.md)
  * [Java](languages/java.md)
  * [JSP](languages/jsp.md)
  * [LLVM](languages/llvm.md)
  * [Scala](languages/scala.md)
* Introduction
  * [ShiftLeft Products](introduction/products.md)
  * [Compliance](introduction/compliance.md)
  * [Requirements](introduction/requirements.md)
  * [The HelloShiftLeft Sample Application](introduction/helloshiftleft.md)
  * [Understanding the CPG](introduction/understanding-cpg.md)
* Using the ShiftLeft CLI
  * [Installing the CLI](using-cli/install-cli.md)
  * [Authenticating with ShiftLeft](using-cli/authenticating.md)
  * [CLI Reference](using-cli/cli-reference.md)
    * [Global Options](using-cli/options/global-options.md)
    * [sl analyze Options](using-cli/options/analyze-options.md)
    * [sl config-file Options](using-cli/options/config-file-options.md)
    * [sl policy Options](using-cli/options/policy-options.md)
    * [sl run Options](using-cli/options/run-options.md)
* Using ShiftLeft Ocular
  * About ShiftLeft Ocular
    * [ShiftLeft Ocular Features](using-ocular/about/ocular-features.md)
    * [Memory Size Recommendations](using-ocular/about/ocular-memory-size.md)
    * [Interactive and Non-Interactive Modes](using-ocular/about/modes.md)
    * [Understanding Layers](using-ocular/about/layers.md)
    * [Deep-Dive: The CPG](using-ocular/about/cpg-deep-dive.md)
  * Getting Started
    * [Quick Start](using-ocular/getting-started/ocular-quick-start.md)
    * [Installing and Updating ShiftLeft Ocular](using-ocular/getting-started/installation.md)
    * [Starting ShiftLeft Ocular](using-ocular/getting-started/starting.md)
    * [Creating and Working with a CPG](using-ocular/getting-started/create-cpg.md)
    * [Generating and Working with Security Profiles](using-ocular/getting-started/generate-sp.md)
    * [Managing Your Workspace](using-ocular/getting-started/manage-workspace.md)
    * [Querying the CPG and Security Profiles](using-ocular/getting-started/query-cpg.md)
  * Common Queries
    * [Examining Dependencies](using-ocular/common-queries/dependency-analysis.md)
    * [Scanning for Types and Packages](using-ocular/common-queries/types-packages-analysis.md)
    * [Examining Method Parameters](using-ocular/common-queries/parameters-analyze.md)
    * [Investigating Methods](using-ocular/common-queries/methods-analyze.md)
    * [Uncovering Data Flows](using-ocular/common-queries/data-flows.md)
  * Use Cases
    * [Detecting 0-Day Vulnerabilities](using-ocular/use-cases/detect-0-day.md)
    * [Discovering HTTP Cookie Poisoning](using-ocular/use-cases/http-cookie-poisoning.md)
    * [Identifying Call Chains](using-ocular/use-cases/call-chains.md)
    * [Tracking Non Atomic Data Types](using-ocular/use-cases/tracking-non-atomic.md)
    * [Uncovering the Attack Surface](using-ocular/use-cases/attack-surface.md)
  * Tutorials
    * [Examining a Java Application for Deserialization Sinks](using-ocular/tutorials/deserialization.md)
    * [Investigating a C Language Application](using-ocular/tutorials/c-language.md)
    * [Identifying Incorrect or Zero Memory Allocation Bugs in C](using-ocular/tutorials/c-allocation-bugs.md)
    * [Using ShiftLeft Ocular with MISRA C Applications](using-ocular/tutorials/misra-c.md)
    * [Java Vulnerable Lab](using-ocular/tutorials/java-vuln.md)
    * [Using ShiftLeft Ocular with JSP](using-ocular/tutorials/ocular-jsp.md)
    * [Debugging ShiftLeft Ocular Scripts with JDB](using-ocular/tutorials/debug-with-jdb.md)
  * Configuring and Extending ShiftLeft Ocular
    * [Identifying Application Code and Dependencies](using-ocular/configure-extend/identify-code-dependencies.md)
    * [Enhancing the OQL](using-ocular/configure-extend/enhance-oql.md)
    * [Executing Code and Scripts at Startup](using-ocular/configure-extend/execute-code.md)
  * [Ocular API](https://ocular.shiftleft.io/api/)
* Using ShiftLeft Inspect and ShiftLeft Protect
  * [Quick Start](using-inspect-protect/inspect-protect-quick-start.md)
  * [Installing on Windows](using-inspect-protect/windows-installer.md)
  * Analyzing Your Code
    * [Analyzing Applications Using ShiftLeft Inspect](using-inspect-protect/inspect/analyzing-applications.md)
    * [Identifying Branch Names](using-inspect-protect/inspect/identify-branches.md)
    * [Failing a Build Based on Analysis Results](using-inspect-protect/inspect/fail-build.md)
  * Protecting Runtime Applications
    * [Securing Your Applications Using ShiftLeft Protect](using-inspect-protect/protect/securing-applications.md)
    * [Running ShiftLeft Protect](using-inspect-protect/protect/run-protect.md)
    * [The ShiftLeft JSON File](using-inspect-protect/protect/json-file.md)
    * ShiftLeft Protect for Java
      * [Configuring the Microagent](using-inspect-protect/protect/protect-java/configuring-the-microagent.md)
      * [Tomcat Integration](using-inspect-protect/protect/protect-java/tomcat-integration.md)
      * [Websphere Configuration](using-inspect-protect/protect/protect-java/websphere-configuration.md)
      * [Wildfly Configuration](using-inspect-protect/protect/protect-java/wildfly-configuration.md)
  * Using the ShiftLeft Dashboard
    * [The Application List](using-inspect-protect/using-dashboard/app-list.md)
    * [The Vulnerabilities Dashboard](using-inspect-protect/using-dashboard/vulnerability-dashboard.md)
    * [Viewing Analysis Results](using-inspect-protect/using-dashboard/view-results.md)
    * [Filtering Analysis Results](using-inspect-protect/using-dashboard/filter-results.md)
  * Workflow Integrations
    * [Integrating with Jira](using-inspect-protect/integrating-with-shiftleft/jira-integration.md)
    * [Integrating with Jenkins](using-inspect-protect/integrating-with-shiftleft/integrating-jenkins-builds/integrating-jenkins-builds.md)
      * [Configure Final Build Step](using-inspect-protect/integrating-with-shiftleft/integrating-jenkins-builds/configure-final-build-step.md)
      * [Configure Post Build Task](using-inspect-protect/integrating-with-shiftleft/integrating-jenkins-builds/configure-post-build-task.md)
      * [Verify Jenkins Integration](using-inspect-protect/integrating-with-shiftleft/integrating-jenkins-builds/verify-jenkins-integration.md)
    * [Integrating with CircleCI](using-inspect-protect/integrating-with-shiftleft/integrating-circleci.md)
    * [Integrating with Bamboo](using-inspect-protect/integrating-with-shiftleft/integrating-bamboo-builds.md)
    * [Integrating with Docker](using-inspect-protect/integrating-with-shiftleft/integrating-docker.md)
    * [Integrating with GoCD](using-inspect-protect/integrating-with-shiftleft/integrating-gocd-builds.md)
    * [Integrating with TeamCity](using-inspect-protect/integrating-with-shiftleft/integrating-teamcity-builds.md)
    * [Integrating with Travis](using-inspect-protect/integrating-with-shiftleft/integrating-travis-builds.md)
  * Application Programming Interfaces (APIs)
    * [Identifying Code Vulnerabilities using the API](using-inspect-protect/api/vulnerabilities_api.md)
* ShiftLeft Policies
  * [About Policies](policies/about-policy.md)
  * [Creating a Custom Policy](policies/custom-policy.md)
  * [Policy Language](policies/policy-language.md)
  * [Use Case: Identifying Sensitive Data Variables](policies/policy-sensitive-data.md)
  * [Use Case: Excluding Vulnerabilities with a Sanitization Function](policies/policy-sanitization-function.md)
* Monthly Updates
  * [December 2019](release-notes/december-2019.md)
  * [November 2019](release-notes/november-2019.md)
  * [October 2019](release-notes/october-2019.md)
  * [September 2019](release-notes/september-2019.md)
  * [August 2019](release-notes/august-2019.md)
  * [July 2019](release-notes/july-2019.md)
  * [June 2019](release-notes/june-2019.md)
  * [May 2019](release-notes/may-2019.md)
  * [April 2019](release-notes/april-2019.md)
  * [March 2019](release-notes/march-2019.md)
