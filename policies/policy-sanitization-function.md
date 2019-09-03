# Use Case: Creating a Custom Policy to Exclude Vulnerabilities with a Sanitization Function

Developers often use sanitization functions in their application source code to sanitize user input for common attacks such as SQL Injection (SQLi). If you use sanitization functions, you want ShiftLeft to ignore vulnerabilities that have a sanitization function in the dataflow when analyzing your application. 

The process is:

1. [Create the new Policy](custom-policy.md#creating-a-new-policy-file).
2. [Specify the Policy](#specifying-the-policy).
3. [Validate the new Policy file](custom-policy.md#validating-the-new-policy).
5. [Upload the custom Policy to the Policy repository](custom-policy.md#uploading-the-new-policy-to-the-repository).
6. [Assign the new Policy](custom-policy.md#assign-the-new-policy).

For additional information, refer to the articles:

* [About ShiftLeft Policies](about-policy.md)
* [ShiftLeft Policy Language](policy-language.md)
* [Creating a Custom Policy](custom-policy.md)
* [Use Case: Creating a Custom Policy to Identify Sensitive Data Variables](policy-sensitive-data.md)

## Specifying the Policy

Add the following directive to the new Policy file 

```
IMPORT io.shiftleft/default
IMPORT io.shiftleft/defaultdict

TAG "CHECK" METHOD -f "javax.servlet.http.HttpServletRequest.setAttribute:void(java.lang.String,java.lang.Object)"
```

where `setAttribute` is the actual method signature of the sanitization function.

The last line instructs ShiftLeft to add a `check` tag to all flows which contain a function that matches the method signature defined with the `-f` option.
