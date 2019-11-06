# Creating a Custom Policy

ShiftLeft Ocular users can create custom Policies, for example to exclude parts of a default Policy that does not apply to, and to introduce additional knowledge about, your application. Custom policies are especially useful in investigating data flows

A common use of custom Policies is to use ShiftLeft Ocular to examine and investigate your code, create a custom Policy tuned to your application, and then automatically analyze and report on these vulnerabilities using ShiftLeft Inspect. For example, to [identify sensitive data variables](policy-sensitive-data.md).

Policies are written in the [ShiftLeft Policy Language](policy-language.md). Policies are stored under organization-bound domains.

A new custom Policy is stored in the file defined by the `path-to-policy-file` argument, or printed to standard output if the argument is omitted.

## Custom Policies for ShiftLeft Ocular

ShiftLeft Ocular Policies are located in the directory

```
~/bin/ocular/policy
```

Custom Policies should be saved in a unique directory in `ocular/policy/static`. 

Note that if you edit a Policy file, you must [reload your application's Code Property Graph (CPG)](../using-ocular/getting-started/create-cpg.md) for the changes to take effect. 

## Custom Policies for ShiftLeft Inspect

The process for creating a custom Policy for ShiftLeft Inspect is:

1. [Create a new Policy file](#creating-a-new-policy-file).
2. [Specify the Policy](#specifying-the-policy-definition).
3. [Validate the new Policy](#validating-the-new-policy).
4. [Upload the new Policy to the Repository](#uploading-the-new-policy-to-the-repository).
5. [Assign the new Policy](#assign-the-new-policy).
6. (Optional) [Identify a custom Policy as the default Policy](#identifying-a-custom-policy-as-the-default-policy).
7. (Optional) [Edit a custom Policy](#editing-a-custom-policy).

You use the [ShiftLeft Command Line Interface (CLI)](../using-cli/cli-reference.md) to create a custom Policy for ShiftLeft Inspect.

For testing purposes, use one of the available open-source projects with known vulnerabilities, such as [Webgoat](https://webgoat.github.io/WebGoat/) or one of your organization’s own applications.

#### Default ShiftLeft Inspect Policy Templates

There are two default Policy templates available, which you use as the basis for creating a new custom Policy: `default` and `no-dictionary`. The `default` template creates a Policy that imports all standard definitions as used by ShiftLeft, including the generic dictionary of sensitive-data variables. 

The `no-dictionary` template  excludes these standard definitions.

### Creating a New Policy File

Use the following command to create a new Policy file

```
sl policy create [default|no-dictionary] [<path-to-policy-file>]
```

where

`default|no-dictionary`. Option to specify which [default policy template](#default-shiftleft-inspect-policy-templates) to use.

`<path-to-policy-file>`. The name and location for the new Policy. Policy files end in the `.policy` filename extension.

### Specifying the Policy Definition

Using a text editor, open the new Policy file. Write new Policy definitions, or edit those imported if you used the [`default` template](#default-shiftleft-inspect-policy-templates), by [using the ShiftLeft Policy Language](policy-language.md).

### Validating the New Policy

ShiftLeft’s repository contains a curated list of Policies. Creating a custom Policy can result in accidental mistakes, even if carefully reviewed. Therefore, make sure you validate your new Policy by using the command

```
sl policy validate [<path-to-policy-file>]
```

where

`<path-to-policy-file>`. The name and location for the new Policy. Policy files end in the `.policy` filename extension.

This command returns a non-zero exit status code if there is a problem in either syntax and/or semantics of the new Policy.

### Uploading the New Policy to the Repository

A Policy can only be used by ShiftLeft Inspect if it is located in the Policy repository. To upload a custom Policy, use the command

```
sl policy push <policy-label>[:<policy-tag>] <path-to-policy-file>

```

where

`policy-label`. The name of the new Policy.

`policy-tag`. (Optional) The Policy version (for example 0.0.1 in mypolicy:0.0.1). Returns either OK or a bad request with detailed error information.

`<path-to-policy-file>`. The name and location for the new Policy.

If successful, the command returns the full name of the Policy, made up of the organization ID from which the Policy was submitted and tag, for example `ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e/myNewDictionary:latest`. 

Check that the new Policy is present in the repository by running the command 

```
sl policy info [<policy-label>[:<policy-tag>]]
```

where

`policy-label`. The name of the Policy on which you want information. If this is omitted, meta-information of all Policies available to your organization is returned. 

`policy-tag`. (Optional) The Policy version. If omitted, the response includes all authorized Policies.

The `info` command lists all Policies uploaded with the specified label that are available to you. Note that you must  provide the complete Policy name, such as `ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e/myNewDictionary:latest`, which narrows the list of returned results. Since Policy filenames are can be overridden, multiple Policy entries may be returned for the same label and/or tag.

### Assign the New Policy

To assign the new Policy to your application, when using ShiftLeft Inspect run the command

```
sl analyze --policy [<policy-label>] --app <name> ~<[<path-to-policy-file>]
```

where 

`policy-label`. The name of the Policy you want to use. 

`name`. The application for which you want to use the Policy.

`<path-to-policy-file>`. The name and location for the new Policy. 


### Identifying a Custom Policy as the Default Policy

Frequent use of a custom Policy can be difficult to manage. Therefore, ShiftLeft allows you to identify a custom Policy as the default Policy for your organization using the command 

```
sl policy assignment set [<policy-label>]
```

where 

`policy-label`. The name of the Policy you want to use. 

You can also set a custom Policy as the default policy for a specific application or application version, instead of across your organization, by using

```
sl policy assignment set --project <name> [<policy-label>[:<policy-tag>]]
```

where 

`policy-label`. The name of the new Policy.

`policy-tag`. (Optional) The Policy version.

`name`. The application for which you want to use the Policy.

### Editing a Custom Policy

Once you have created a custom Policy, you can edit it by [changing the Policy definitions](#specifying-the-policy-definition), and then [validating the updates](#validating-the-new-policy). For example, if you created a new Policy using the [`default` template](#default-shiftleft-inspect-policy-templates), you may want to edit the standard definitions imported into your new file to create a custom Policy
