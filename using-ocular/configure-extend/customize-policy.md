# Customizing a Policy

You use the Ocular Query Language (OQL) to [formulate queries to scan for and identify data flow vulnerabilities](../using-ocular/common-queries/data-flows.md). These queries can be automatically executed using a [non-interactive script](modes.md). However, a better approach is to create a custom Policy. Policies define methods that introduce data into the application, sensitive operations, and data flow.

ShiftLeft Ocular Policies are located in the directory ~/bin/ocular/policy.

## Example

This example shows how the default Policy is specified in order to identify deserialization vulnerabilities. In ShiftLeft's dynamic Policy find the following lines

```
// [~/.shiftleft/policy/dynamic/java/io/ObjectInputStream.policy:]

IO deserializer = METHOD -f "java.io.ObjectInputStream.readObject:java.lang.Object()" { INST "SINK" }
```

These lines state that the instance parameter of the method `readObject` should be considered as a data sink of a deserializer, that is, the instance parameter is deserialized.

This following rule specifies that all parameters tagged with the Spring annotations `CookieValue`, `PathVariable`, and a few others are to be tagged as "attacker-controlled". 

```
// ~/.shiftleft/policy/dynamic/org/springframework/exposed.policy:

TAG "DATA_TYPE" -v "attacker-controlled" METHOD -a r"org\.springframework\.web\.bind\.annotation\.(Request|Get|Post|Put|Delete|Patch)Mapping" PAR -a r"org\.springframework\.web\.bind\.annotation\.(CookieValue|PathVariable|RequestParam|RequestBody)"
```

This rule specifies that flows of attacker-controlled data into deserializers are worth reporting

```
// ~/.shiftleft/policy/static/execute.policy:

CONCLUSION attacker-to-deserializer = FLOW DATA (attacker-controlled) -> IO (deserializer)
WHEN CONCLUSION attacker-to-deserializer => EMIT {
    title: "Attacker controlled data to deserialization",
    description: "Attacker controlled data is deserialized in this flow. ...",
    category: "a1-injection",
    score: "8.0"
}
```

For this example, the Policy also allows data transformations and checks to be specified in order to report flows of data, for data that does not undergo validation. 
