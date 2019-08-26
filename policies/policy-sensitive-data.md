# Use Case: Creating a Custom Policy to Identify Sensitive Data Variables

A common use case for using a custom Policy is to provide application- or organization-specific rules that more accurately reflect the state of sensitive data variables. This article illustrates how to create just such a custom policy for an application-specific sensitive-data dictionary, using an example ` my-app-dictionary.policy` Policy.

The process is:

1. [Create the new Policy](#creating-the-new-policy).
2. [Develop the application-specific dictionary](#developing-the-application-specific-dictionary).
3. [Validate the new Policy file](#validating-the-new-policy-file).
4. [Upload the custom Policy to the Policy repository](#uploading-the-custom-policy-to-the-policy-repository).
5. [Assign the new Policy](#assigning-the-new-policy).


For additional information, refer to the articles:

* [About ShiftLeft Policies](about-policy.md)
* [ShiftLeft Policy Language](policy-language.md)
* [Creating a Custom Policy](custom-policy.md)

## Creating the New Policy

You create the new Policy file using the `no-dictionary` template, using the following command

```
sl policy create no-dictionary my-app-dictionary.policy
```

where `my-app-dictionary.policy` is the location and name of the new Policy file.  

The command creates the new Policy file and imports default definitions, as declared by ShiftLeft, except for the sensitive-data dictionary. 

## Developing the Application-Specific Dictionary

Developing your application-specific dictionary by editing the `my-app-dictionary.policy` file with your own unique sensitive-data directives.

As defined in the ShiftLeft [Policy Language](policy-language.md), sensitive-data directives have the form

```
DATA $group = VAR $term1, ..., $term_n
```
where 

`$group` is a sensitive-data group such as "internal-secrets"

`$term1 to $termn` are keywords. 

The default Policy contains these directives to characterize highly sensitive data:

```
DATA highly-sensitive = VAR master key, cvv num, cvv, cvc num, cvc, encrypt key, crypt key

```

The sensitive-data engine looks for exact matches,  as well as variations and combinations, of these terms.

For example, since you are only interested in a limited number of data-sensitive categories to define Personal Identifying Information that includes email, phone number and (people) names. And you want to ignore any other categories. To get that information, append the following sample snippet to the `my-app-dictionary.policy` file:

```
# PII
DATA pii = VAR first name, last name, middle name, middle initials, full name, maiden name, player name, family name
DATA pii = VAR email, email addr, email address, alternate email
DATA pii  = VAR phone number, phone, mobile, landline number, home phone number, home phone num, office phone number, office phone num, alternate phone num, alternate phone number, phone number extension
```

So that the Policy rule is

```
IMPORT io.shiftleft/default

# PII
DATA pii = VAR first name, last name, middle name, middle initials, full name, maiden name, player name, family name
DATA pii = VAR email, email addr, email address, alternate email
DATA pii  = VAR phone number, phone, mobile, landline number, home phone number, home phone num, office phone number, office phone num, alternate phone num, alternate phone number, phone number extension
```

## Validating the New Policy File

Verify that your new Policy is valid by running

```
sl policy validate my-app-dictionary.policy
```

This command returns a non-zero exit status code if there is a problem in either syntax and/or semantics of the new Policy.

## Uploading the Custom Policy to the Policy Repository

Upload your custom Policy with the command

```
sl policy push myNewDictionary my-app-dictionary.policy
```

If successful, the command returns the full name under which the Policy is available, eg., `ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e/myNewDictionary:latest`, where 

`ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e` represents the organization ID from which the Policy was submitted.

`latest` is the tag assigned to the Policy (added by default). 

## Assigning the New Policy

To assign the new Policy to your application, when using ShiftLeft Inspect run the command

```
sl analyze --policy ebad68cf-b1bf-4b00-b524-8d41c6b4ff7e/myNewDictionary --app my-application ~/path/to/file.jar
```

As expected, using your custom Policy, ShiftLeft Inspect identifies less vulnerabilities for your application than if using the default Policy. This is because the dictionary of sensitive-data categories has been significantly reduced in the custom Policy. 
