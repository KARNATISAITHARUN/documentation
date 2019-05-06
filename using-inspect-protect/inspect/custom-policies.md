# Using Custom Security Policies with ShiftLeft Inspect

Security policies specify how your application communicates with the outside world, which transformations exist on data, and which information flows should be considered security violations. ShiftLeft provides a database of default security policies. In addition you can create your own custom security policy to exclude parts of the default policy that do not apply to your application, or to introduce additional knowledge about your application.

A common use case for using a custom security policy is the ability to provide application- or organization-specific rules  that more accurately reflect the state of sensitive data variables. This use case is used here as an illustrative example. 

Generally, most custom security policies can be created and managed using the [ShiftLeft CLI](using-cli/using-cli.md) and a limited set of commands. However, more complex use cases are possible that may require additional features and/or integration with other services. [Contact us](https://www.shiftleft.io/contact/) if you want to implement advanced use cases using custom security policies.

## Use Case: Application-Specific Sensitive-Data Dictionary

Every time ShiftLeft Inspect runs, the infrastructure infers the correct security policy to use, based on the source language. By default such a policy uses a generic set of sensitive variable names, which directly affect the security results you see.

The process of creating and using a custom security policy for an application-specific sensitive-data dictionary is:

1. [Create a new security policy](#creating-a-new-security-policy).
2. [Provide an application-specific dictionary](#providing-an-application-specific-dictionary).
3. [Validate the new security policy file](#validating-the-new-security-policy-file).
4. [Inspect the ShiftLeft default dictionary](#inspecting-the-shiftLeft-default-dictionary).
5. [Upload the custom security policy to the ShiftLeft policy repository](#uploading-the-custom-security-policy-to-the-shiftLeft-policy-repository).
6. [Use the new security policy](#using-the-new-security-policy).
7. [Identify a custom security policy as the default policy](#identifying-a-custom-security-policy-as-the-default-policy).
8. [List and delete a security policy assignment](#listing-and-deleting-a-security-policy-assignment)

## Creating a New Security Policy

You can create a new security policy file, with no default dictionary, from a template using the following command

```
sl policy create no-dictionary my-app-dictionary.policy
```

where `my-app-dictionary.policy` is your current directory, for this example named my-app-dictionary.policy. 

The command imports default definitions, as declared by ShiftLeft, except for the sensitive-data dictionary.

## Providing an Application-Specific Dictionary

Create an application-specific dictionary by editing the `my-app-dictionary.policy` file with your own unique sensitive-data directives.

As defined in the [Security Policy Language](../../policies/spl.md), sensitive-data directives have the form

```
DATA $group = VAR $term1, ..., $term_n
```
where 

`$group` is a sensitive-data group such as "internal-secrets"

`$term1 to $termn` are keywords. 

For example, the default policy contains these directives to characterize highly sensitive data:


```
DATA highly-sensitive = VAR master key, cvv num, cvv, cvc num, cvc, encrypt key, crypt key

```

The sensitive-data engine looks for exact matches,  as well as variations and combinations, of these terms.

For example, you are only interested in a limited number of data-sensitive categories to define Personal Identifying Information that includes email, phone number and (people) names. And you want to ignore any other categories. To get that information, append the following sample snippet to the `my-app-dictionary.policy` file:

```
# PII
DATA pii = VAR first name, last name, middle name, middle initials, full name, maiden name, player name, family name
DATA pii = VAR email, email addr, email address, alternate email
DATA pii  = VAR phone number, phone, mobile, landline number, home phone number, home phone num, office phone number, office phone num, alternate phone num, alternate phone number, phone number extension
```

So that the policy rule is

```
IMPORT io.shiftleft/default

# PII
DATA pii = VAR first name, last name, middle name, middle initials, full name, maiden name, player name, family name
DATA pii = VAR email, email addr, email address, alternate email
DATA pii  = VAR phone number, phone, mobile, landline number, home phone number, home phone num, office phone number, office phone num, alternate phone num, alternate phone number, phone number extension
```

## Validating the New Security Policy File

ShiftLeft’s repository only contains a curated list of security policies. Creating a custom security policy can result in  accidental mistakes, even if carefully reviewed. Therefore you must verify if the new security policy is valid by using the following command:

```
sl policy validate my-app-dictionary.policy
```

This command returns a non-zero exit status code if there is a problem in either syntax and/or semantics of the new policy.

## Inspecting the ShiftLeft Default Dictionary

You can inspect the ShiftLeft default dictionary to compare your custom security policy. To retrieve the default dictionary, named `defaultdict` and stored it in the `io.shiftleft` domain, run the command

```
sl policy pull io.shiftleft/defaultdict
```

The returned information contains the entire default policy, as defined in ShiftLeft’s official policy repository. Security policies are stored under organization-bound domains; you can retrieve any security policy with the policy pull command, if authorized

## Uploading the Custom Security Policy to the ShiftLeft Policy Repository

A security policy can only be used by ShiftLeft Inspect if it has been uploaded to the ShiftLeft policy repository. To upload your custom security policy, use the command

```
sl policy push myNewDictionary my-app-dictionary.policy
```

If successful, the command returns the full name under which the security policy is available, eg., `ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e/myNewDictionary:latest`, where 

`ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e` represents the organization ID from which the security policy was submitted.

`latest` is the tag assigned to the policy (added since it was absent). 

You can always use a specific tag, if needed

```
sl policy push myNewDictionary:0.0.1 my-app-dictionary.policy
```

Make sure to verify that the new security policy is present in the ShiftLeft policy repository by running the command 

```
sl policy info myNewDictionary
```

This command lists all policies uploaded with the `myNewDictionary` label that are available to you. Note that you must  provide the complete policy name, such as `ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e/myNewDictionary:latest`, which narrows the list of returned results. Since security policy filenames are overridable, multiple security policy entries may be returned for the same label and/or tag.

## Using the New Security Policy

For testing purposes, use one of the available open-source projects with known vulnerabilities, such as [Webgoat](https://webgoat.github.io/WebGoat/) or one of your organization’s own applications.

ShiftLeft Inspect for Java is run using the command

```
sl analyze --app my-application ~/path/to/file.jar
```

ShiftLeft automatically infers the default security policy to be used with `my-application`. For a Java application, the default security policy is `io.shiftleft/helloshiftleft`.

To instead use the example custom security policy with ShiftLeft Inspect, run

```
sl analyze --policy ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e/myNewDictionary --app my-application ~/path/to/file.jar
```

As expected, in this case ShiftLeft Inspect finds less vulnerabilities for exactly the same jar, since the dictionary of sensitive-data categories has been significantly reduced in the custom security policy. 

## Identifying a Custom Security Policy as the Default Policy

Explicit use of a custom security policy ID can be difficult to manage, especially if the process is frequently repeated. Therefore, ShiftLeft allows you to identify a custom security policy as the default security policy for your organization using the command 

```
sl policy assignment set ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e/myNewDictionary
```

or for a specific application

```
sl policy assignment set --project my-application ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e/myNewDictionary:latest
```

As a result, both ShiftLeft Inspect analysis commands

```
sl analyze --app my-application
```

and

```
sl analyze --app my-application --policy-id ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e/myNewDictionary:latest
```

return the same results.

## Listing and Deleting a Default Security Policy Assignment

To list the existing security policy assignments, use

```
sl policy assignment list
```

The command returns a list of the default security policies, including a custom security policy that was identified as the default policy for the application. So for example, the command returns

```
Policy assignments:
  [0] OrgID/ProjectID: 'ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e/my-application',   
      PolicyID: 'ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e/myNewDictionary:latest'
```

To delete a default security policy assignment, use

```
sl policy assignment remove
```

or for a per-project security policy assignment

```
sl policy assignment remove --project my-application
```
