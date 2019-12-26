# ShiftLeft December 2019 Updates

The following is a list of changes made to ShiftLeft during December 2019.

## What's New

* **The Check Environment Flag**: ShiftLeft's `sl check-environment` [command](../using-cli/authenticating.md#checking-the-environment) can be run to make sure that your ShiftLeft configuration exists, that your network connection is functional, and see the artifacts currently installed on your machine. You can add an additional flag (e.g., `--jvm` or `--dotnet`) to check your language-specific options.

* **GitHub Authentication**: You can now [register](https://www.shiftleft.io/register) and [log in](https://www.shiftleft.io/login) to ShiftLeft using your GitHub credentials.

* **Self-Serve Portal**: The [self-serve portal](https://www.shiftleft.io/register) allows you to use ShiftLeft Inspect without first reaching out to the ShiftLeft team to request access. This version of Inspect, which is available for free, allows up to five users to perform 300 scans annually on 200,000 lines of code or less. It supports applications written in Java, C#, Golang and Scala, and allows developers to run two scans concurrently and analyze an infinite number of dependencies and frameworks.

## Bug Fixes

* The [Dashboard](../using-inspect-protect/using-dashboard/vulnerability-dashboard) now displays line numbers correctly when presenting data regarding vulnerabilities found in .NET applications.
