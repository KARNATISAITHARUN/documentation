# Policy Command Options

## `sl policy create` Options
`sl policy create` Command Options | Description
--- | ---
`[default\|no-dictionary] [<path-to-policy-file>]` | [Create a custom Policy from the specified default template](../../policies/custom-policy.md). A new custom Policy is stored in the file defined by the `path-to-policy-file` argument, or printed to standard output if the argument is omitted.

## `sl policy info` Options
`sl policy info` Command Options | Description
--- | ---
`[<policy-label>[:<policy-tag>]]` | Return meta-information about the Policy with the name specified by the `<policy-label>` argument. If the Policy name is omitted, meta-information of all Policies available to your organization is returned. The `policy-tag` element is optional; if omitted the response includes all authorized Policies.
`--public` | If set, fetch information about Policy from the public domain.

## `sl policy pull` Options
`sl policy pull` Command Options | Arguments | Description
--- | --- | ---
`--hash <policy-hash>` | `[<path-to-policy-file>]` | Fetch the Policy, with the hash specified by  the `policy-hash` argument, from the ShiftLeft infrastructure as a text file or a bad request with detailed error information. If the `path-to-policy-file` argument is missing, the Policy is printed to the console. Must be authorized to use this command.
| | `<policy-label>[:<policy-tag>] [<path-to-policy-file>]` | Fetch the Policy, with the name specified by the `policy-label` argument, from the ShiftLeft infrastructure as a text file or a bad request with detailed error information.. Use of the Policy tag is optional (for example `0.0.1` in `mypolicy:0.0.1`); by default the value is the latest Policy. If the `path-to-policy-file` argument is missing, the Policy is printed to the console. Must be authorized to use this command.

## `sl policy push` Options
`sl policy push` Command Options | Description
--- | ---
`<policy-label>[:<policy-tag>] <path-to-policy-file>` | Upload a custom Policy, with the name specified by the `policy-label` argument, to the ShiftLeft repository. Use of the Policy tag is optional (for example `0.0.1` in `mypolicy:0.0.1`); by default the value is the latest Policy. Returns either OK or a bad request with detailed error information.

## `sl policy validate` Options
`sl policy validate` Command Options | Description
--- | ---
`<path-to-policy-file>` | Verify a custom Policy to check for syntactic and semantic errors. Returns either OK or a bad request with detailed error information.
