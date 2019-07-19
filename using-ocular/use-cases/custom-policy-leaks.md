# Detecting Data Leaks using a Customized Policy

ShiftLeft Ocular includes sensitive data leak detection using variable names. The default dictionary of indicative terms can be extented, so that you can specify the variables to be treated as sensitive for your application. These directives have the form

```
DATA <group> = VAR <term1, ..., termn>
```

where 

`<group>` is a sensitive-data group such as "internal-secrets"
`<term1>` to `<termn>` are keywords. 

For example, the default Policy contains the following directive to characterize highly sensitive data:

```
DATA highly-sensitive = VAR master key, cvv num, cvv, cvc num, cvc, encrypt key, crypt key
```

To find specific sensitive data leaks, edit the directive to reflect the appropriate group and term keywords. The sensitive-data engine looks for exact matches of these terms and variations and combinations of these terms.

For more information, refer to [Customizing a Policy](../about/customize-policies.md) and [Policy Language](../../policies/spl.md).
