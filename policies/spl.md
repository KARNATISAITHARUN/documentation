# Policy Language

ShiftLeft's Policy Language provides support for core language features, libraries, and frameworks through the use of policy files. Policy files specify how the application communicates with the outside world, which transformations exist on data, and which information flows should be considered security violations. ShiftLeft provides a database of default policy rules, and additionally allows you to create your own custom policy. Custom policy rules can exclude parts of the default policy that do not apply to the application, or may introduce additional knowledge about the application.


The default policy is found in the directory

```
~/.shiftleft/policy/
```

and is automatically loaded and applied when loading CPGs in the REPL using the `loadCpg` command. If you edit the policy, invoke`loadCpg` again to apply any policy changes.

Policies are specified using a Policy Language, which offers four types of directives:

* **[Tagging Directives](#tagging-directives).** Exposed methods, interface interactions and transformations are determined by tagging the CPG based on syntax-patterns. The policy contains tagging directives to encode these patterns.

* **[Flow Description Directives](#flow-description-directives).** The policy specifies patterns for information flows that, when observed, should be reported as possible instances of vulnerabilities, particularly data leaks.

* **[Sensitive-Data Directives](#sensitive-data-directives).** ShiftLeft implements natural language processing techniques to identify sensitive data elements based on variable, field and parameter naming. Names specific to the application can be provided using sensitive data directives.

* **[Taint Semantics (Advanced) Directives](#taint-semantics-directives).** At the lowest level of abstraction, policies define taint semantics. These directives map between method input and output parameters that express propagation of taint. This information is stored in the ICFG and can subsequently be accessed by static taint tracking algorithms.

Using each of these directives is documented in detail, with examples of how they are employed in the default policy.

## Tagging Directives 

### Tagging Directives to Mark IO Endpoints, Transformations and Exposed Methods

As a result of invoking library methods, data may be read from the outside world or written to it. Tagging directives can be used to inform ShiftLeft Ocular about these methods. These directives result in tagging of the CPG with a set of predefined tags that ShiftLeft Ocular can use.

The Policy Language provides the IO, TRANSFORMER and EXPOSED directives to tag IO endpoints, transformers and exposed methods, respectively. The directives all follow the format

```
$command label = METHOD -f "$fullName" [{ (PAR -i $i|RET|INST) "(SOURCE|SINK|DESCRIPTOR|DESCRIPTOR_USE)" }]
```

where 
* `$command` is either IO, TRANSFORMER or EXPOSED
* `$fullName` is a method signature 
* `$i` is a parameter number. 

### IO Tagging Directives

Employed to describe the effects of calls to external libraries. An example is the following line from the default policy for "java.io.FileInputStream":

```
IO file = METHOD -f "java.io.FileInputStream.read:int(byte[])" { PAR -i 1 "SOURCE" }
```

This line specifies that, upon invoking the  method "java.io.FileInputStream.read:int(byte[]), its first parameter serves as a data source, and that this is data read from a file. Similarly,  the method "java.io.FileInputStream.read:int()"  introduces an integer read from a file into the program. This can be encoded via the policy line

```
IO file = METHOD -f "java.io.FileInputStream.read:int()" { RET "SOURCE" }
```

Library methods can also write data to the outside. Analogously to FileInputStream, annotate FileOutputStream as follows

```
IO file = METHOD -f "java.io.FileOutputStream.write:void(byte[])" { PAR -i 1 "SINK" }
```

This line indicates that data which reaches the first parameter of the method "java.io.FileOutputStream.write:void(byte[])" is written to a file.

### DESCRIPTOR Flows Tagging Directives

Usually, the single data flow (primary) of an IO flow does not include less relevant objects creations. However, sometimes these object creations are important if the flow is actually vulnerable. For this reason, descriptor flows of source and sink can provide additional clues about the primary data flow.

An example when descriptors are required:

```
File f1 = new File(HttpRequest.read());
FileOutputStream fs1 = FileOutputStream(f1);

File f2 = new File("static.txt");
FileInputStream fs2 = FileInputStream(f2);

String res = fs2.read();
fs1.write(res);
```

In this example, you explicitly see a read operation from a file (SOURCE) to a write operation to another file (SINK).
These operations are common and most of the time intended in the primary data flow (from SOURCE to SINK); you do not notice anything attacker controllable. In this case descriptor flows help to trace back the creation of the files and identify if the file name is controllable from outside, which is the case for `f1` controlled from HTTP.

Considering the IO flow, a more detailed flow is specified during the write/read operation using `DESCRIPTOR_USE`. In this case, the stream where the data is written is the instance.

```
IO file = METHOD -f "java.io.FileOutputStream.write:void(byte[])" { PAR -i 1 "SINK", INST "DESCRIPTOR_USE" }
```

To describe those descriptors instances, tag (`DESCRIPTOR`) the creation of the descriptors instances to augment the information of an IO flow summary.

```
IO fileStream = METHOD -f "java.io.FileOutputStream.<init>:void()" { INST "DESCRIPTOR" }
```

Finding Descriptor flows is done recursively, so even a descriptor creation can make use of other descriptors. This is the case especially in constructors parameters.

```
IO fileStream = METHOD -f "java.io.FileOutputStream.<init>:void(java.io.File)" { INST "DESCRIPTOR", PAR -i 1 "DESCRIPTOR_USE" }
```

N.B. Parameters can have different IO names and tags depending on the actual context and only the correct one is considered.

```
IO fileStream = METHOD -f "java.io.FileOutputStream.<init>:void(java.lang.String)" { INST "DESCRIPTOR", PAR -i 1 "DESCRIPTOR_USE" }
IO filePath = METHOD -f "java.io.FileOutputStream.<init>:void(java.lang.String)" { PAR -i 1 "SINK" }
```

### TRANSFORMER Tagging Directives

Allows you to specify which methods transform data or may be considered data validation routines. As an example, consider the method "encodeBase64", which takes an input string as its first argument and returns a base64-encoded version of that string. This behavior is captured by the following directive:

```
TRANSFORM base64 = METHOD -f "org.apache.commons.codec.binary.Base64.encodeBase64:byte[](byte[])" { PAR -i 1 "SINK", RET "SOURCE" }
```

The first parameter is a data sink, and the return value acts as a data source. Transformer directives can also be used to model validations of input arguments. So you may wish to specify that a string is considered to be validated if it passes through a string comparison. To achieve this, transformations such as the following can be defined:

```
TRANSFORM string-compare = METHOD -f "java.lang.String.compareTo:int(java.lang.String)" { PAR -i 1 "SINK", RET "SOURCE"}
TRANSFORM string-compare = METHOD -f "java.lang.String.contains:boolean(java.lang.CharSequence)" { INST "SINK", RET "SOURCE" }
TRANSFORM string-compare = METHOD -f "java.lang.String.matches:boolean(java.lang.String)" { INST "SINK", RET "SOURCE" }
TRANSFORM string-compare = METHOD -f "java.lang.String.startsWith:boolean(java.lang.String)" { INST "SINK", RET "SOURCE" }
...
```

### WHEN Tagging Directives

Describes the effects of transformers. For example, a base64 encoder generates encoded data. The Policy Language allows arbitrary tags to be added or removed as a result of a transformation. So 

```
WHEN TRANSFORM base64 => DATA +encoded
```

specifies that, on invocation of the base64 method, the tag "encoded" is added to the output datag. Similarly, the tag "encoded" can be removed via the directive

```
WHEN TRANSFORM base64-decode => DATA -encoded
```

assuming that a transformer with the "base64-decode" label is specified.


### EXPOSED Tagging Directives

Used to mark methods that can be triggered by attackers from the outside. For example, for the application "com.mycustomer.MyClass.httpHandler:java.lang.String(java.lang.String)", you know that the handler can be called by an attacker from the outside and that the first parameter is attacker-controlled. Moreover, the return value of the method is passed back to the attacker. This can be captured with the directive

```
EXPOSED http = METHOD -f "com.mycustomer.MyClass.httpHandler:java.lang.String(java.lang.String)" { PAR -i 1 "SOURCE", RET "SINK" }
```

Note to experts: EXPOSED directives perform the same actions as IO directives, with the small difference that SOURCE parameters and SINK parameters correspond to output and input parameters respectively for IO directives, and to input and output parameters for EXPOSED directives.

## Flow Description Directives

With data sources, sinks, descriptors and transformers tagged, these are now combined in flow descriptions to describe flows of interest. A flow description has the form

```
CONCLUSION label = (DATA | IO) $expr1 [-> (DATA | IO) $expr2]
```

where `$expr1` and `$expr2` are Boolean expressions over tags in accordance with the grammar:

```
expr := tag
      | tag OR expr
      | tag AND expr
      | not expr
```

Examples for valid expressions are "http", "http OR ftp", "http AND NOT sensitive". The simplest flow expressions only characterize tags at the end of an information flow. For example, to capture all flows that reach the outside world and are encrypted, the flow description is formulated as

```
CONCLUSION encrypted-to-outside = FLOW DATA encrypted
```

Similarly, to capture all flows to http methods, use the description

```
CONCLUSION to-http = FLOW IO http
```

It is also possible to constrain both data tags and method tags. For example, to capture unencrypted data sent out using  HTTP, formulate the rule

```
CONCLUSION unencrypted-to-http = FLOW DATA (NOT encrypted) -> IO http
```

Moreover, it is possible to restrict tags at the data source. So all flows where attacker-controlled data enters the application via HTTP and remains attacker-controlled throughout the entire flow can be captured as 

```
CONCLUSION attacker-controlled-from-http = FLOW IO http -> DATA attacker-controlled
```

Finally, both source and sink tags can be restricted, e.g., to capture flows from files to http, using the rule

```
CONCLUSION file-to-http = FLOW IO file -> IO http
```

and finally, to additionally monitor only unencrypted data, use

```
CONCLUSION file-to-http-unencrypt = FLOW IO file -> DATA (NOT encrypted) -> IO http
```

Propagated descriptor flow tags are prefixed with $ to have a more granular matching

```
CONCLUSION file-to-http = FLOW IO ($fileStream AND read) -> IO ($http AND write)
```

Upon matching a "CONCLUSION" statement, a message is emitted that characterizes the finding:

```
CONCLUSION file-to-http = FLOW IO file -> IO http
WHEN CONCLUSION file-to-http => EMIT {
    title: "File to http",
    description: "The contents of a file is sent out via http",
    category: "a6-sensitive-data-exposure",
    score: "1.0"
}
```


## Sensitive-Data Directives

ShiftLeft Ocular includes sensitive data detection based on heuristics. This process makes use of variable names. So that you can specify variables that are to be treated as sensitive for their applications, the default dictionary of indicative terms is allowed to be extended using sensitive-data directives. These directives have the form

```
DATA $group = VAR $term1, ..., $term_n
```

where 
`$group` is a sensitive-data group such as "internal-secrets"
`$term1` to `$termn` are keywords. For example, the default policy contains the following directive to characterize highly sensitive data:

```
DATA highly-sensitive = VAR master key, cvv num, cvv, cvc num, cvc, encrypt key, crypt key
```

The sensitive-data engine looks for exact matches of these terms as well as variations and combinations of these terms.

## Taint Semantics Directives

For advanced use only.

Library methods may also simply propagate taint without performing any transformations on the data that change its Security Properties. For standard libraries, these propagation rules are already provided by the ShiftLeft default policy. However, for exotic libraries unavailable in code to ShiftLeft Ocular, these rules can also be specified manually via MAP directives. These directives specify how taint is propagated from the input parameters of a library method to its output parameters. MAP directives follow the form

```
MAP -[override|preserve] -d (RET | INST | PAR -i $i) -s (INST | PAR -i $i) METHOD -f "$fullName"
```

where 
`$i` is a parameter number 
`$fullName` is the full name of a method. 

MAP statements associate a source parameter of a given method with a destination parameter. As a result, ShiftLeft Ocular is informed that, if the source parameter is tainted, then the destination parameter is tainted after execution of the library method. 

As an example, if the method "java.lang.String.concat:java.lang.String(java.lang.String)" of the Java standard library has a tainted instance parameter, then so is the return value of the library call. This rule is specify using the MAP directive

```
MAP -override -d RET -s INST METHOD -f "java.lang.String.concat:java.lang.String(java.lang.String)"
```

The rule indicates that, if the instance parameter (INST) is tainted, then the return parameter (RET) is tainted after execution of the method. The flag "-preserve" indicates to additionally add a mapping from the destination to itself.

```
MAP -preserve -d INST -s PAR -i 1 METHOD -f "java.lang.StringBuffer.append:java.lang.StringBuffer(java.lang.StringBuffer)"
```
	
Is the shorthand of:

```
MAP -override -d INST -s PAR -i 1 METHOD -f "java.lang.StringBuffer.append:java.lang.StringBuffer(java.lang.StringBuffer)"
MAP -override -d INST -s INST METHOD -f "java.lang.StringBuffer.append:java.lang.StringBuffer(java.lang.StringBuffer)"
```
