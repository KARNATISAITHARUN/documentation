# ShiftLeft Policy Language

The ShiftLeft Policy Language is used to create, customize and manage Policies. The Policy Language is made up of three types of directives:

* **[Tagging](#tagging-directives).** Exposed methods, interface interactions and transformations are tagged in the [Code Property Graph (CPG)](../introduction/understanding-cpg.md) based on syntax-patterns. The Policy contains tagging directives to encode these patterns.

* **[Flow Description](#flow-description-directives).** The Policy specifies patterns for information flows that should be reported as possible instances of vulnerabilities, particularly data leaks.

* **[(Advanced) Taint Semantics](#taint-semantics-directives).** At the lowest level of abstraction, Policies define taint semantics. These directives map between method input and output parameters that express propagation of taint. This information is stored in the ICFG and can subsequently be accessed by static taint tracking algorithms.

Specifying each type of directive is documented in detail, with examples of how they are employed in the default Policy.

Note that for more complex Policy use cases, that may require additional features and/or integration with other services, [contact us](https://www.shiftleft.io/contact/).

## Tagging Directives 

### Tagging Directives to Mark IO Endpoints, Transformations and Exposed Methods

As a result of invoking library methods, data may be read from the outside world or written to it. Tagging directives are  used to inform ShiftLeft about these methods. These directives result in tagging the CPG.

The Policy Language provides the IO, TRANSFORMER and EXPOSED tagging directives to for IO endpoints, transformers and exposed methods, respectively. The directives all follow the format

```
$command label = METHOD -f "$fullName" [{ (PAR -i $i|RET|INST) "(SOURCE|SINK|DESCRIPTOR|DESCRIPTOR_USE)" }]
```

where 

* `$command` Either IO, TRANSFORMER or EXPOSED
* `$fullName` Method signature 
* `$i` Parameter number. 

### IO Tagging Directives

Employed to describe the effects of calls to external libraries. An example is the following directive from the default Policy for "java.io.FileInputStream":

```
IO file = METHOD -f "java.io.FileInputStream.read:int(byte[])" { PAR -i 1 "SOURCE" }
```

This directive specifies that, upon invoking the  method "java.io.FileInputStream.read:int(byte[]), its first parameter serves as a data source, and that this is data read from a file. Similarly,  the method "java.io.FileInputStream.read:int()"  introduces an integer read from a file into the program. This can be encoded via the Policy directive

```
IO file = METHOD -f "java.io.FileInputStream.read:int()" { RET "SOURCE" }
```

Library methods can also write data to the outside. Analogously to FileInputStream, annotate FileOutputStream as follows

```
IO file = METHOD -f "java.io.FileOutputStream.write:void(byte[])" { PAR -i 1 "SINK" }
```

This directive indicates that data which reaches the first parameter of the method "java.io.FileOutputStream.write:void(byte[])" is written to a file.

### DESCRIPTOR Flows Tagging Directives

Usually, the single data flow (primary) of an IO flow does not include less relevant objects creations. However, sometimes these object creations are important if the flow is actually vulnerable. For this reason, descriptor flows of source and sink can provide additional clues about the primary data flow.

An example when descriptors are required 

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

To describe those descriptors instances, tag (`DESCRIPTOR`) the creation of the descriptors instances to augment the information of an IO flow summary is

```
IO fileStream = METHOD -f "java.io.FileOutputStream.<init>:void()" { INST "DESCRIPTOR" }
```

Finding Descriptor flows is done recursively, so even a descriptor creation can make use of other descriptors. This is the case especially in constructors parameters

```
IO fileStream = METHOD -f "java.io.FileOutputStream.<init>:void(java.io.File)" { INST "DESCRIPTOR", PAR -i 1 "DESCRIPTOR_USE" }
```

N.B. parameters can have different IO names and tags depending on the actual context and only the correct one is considered.

```
IO fileStream = METHOD -f "java.io.FileOutputStream.<init>:void(java.lang.String)" { INST "DESCRIPTOR", PAR -i 1 "DESCRIPTOR_USE" }
IO filePath = METHOD -f "java.io.FileOutputStream.<init>:void(java.lang.String)" { PAR -i 1 "SINK" }
```

### TRANSFORMER Tagging Directives

Allows you to specify which methods transform data or may be considered data validation routines. As an example, consider the method "encodeBase64", which takes an input string as its first argument and returns a base64-encoded version of that string. This behavior is captured by the following directive

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

specifies that, on invocation of the base64 method, the tag "encoded" is added to the output tag. Similarly, the tag "encoded" can be removed via the directive

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
CONCLUSION label = FLOW (DATA | IO) $expr1 [-> (DATA | IO) $expr2]
```

where `$expr1` and `$expr2` are Boolean expressions over tags in accordance with the grammar

```
expr := tag
      | tag OR expr
      | tag AND expr
      | not expr
```

Examples for valid expressions are `http`, `http OR ftp` and `http AND NOT sensitive`. The simplest flow expressions only characterize tags at the end of an information flow. For example, to capture all flows that reach the outside world and are encrypted, the flow description is formulated as

```
CONCLUSION encrypted-to-outside = FLOW DATA encrypted
```

Similarly, to capture all flows to http methods, use the description

```
CONCLUSION to-http = FLOW IO http
```

It is also possible to constrain both data tags and method tags. For example, to capture unencrypted data sent out using  HTTP, formulate the directive

```
CONCLUSION unencrypted-to-http = FLOW DATA (NOT encrypted) -> IO http
```

Moreover, it is possible to restrict tags at the data source. So all flows where attacker-controlled data enters the application via HTTP and remains attacker-controlled throughout the entire flow can be captured as 

```
CONCLUSION attacker-controlled-from-http = FLOW IO http -> DATA attacker-controlled
```

Both source and sink tags can be restricted, e.g., to capture flows from files to http, using the directive

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

## Taint Semantics Directives

For advanced use only.

Library methods may also simply propagate taint without performing any transformations on the data that change its security properties. For standard libraries, these propagation rules are already provided by the default Policy. However, for exotic libraries unavailable in code to ShiftLeft, these rules can also be specified manually via MAP directives. These directives specify how taint is propagated from the input parameters of a library method to its output parameters. MAP directives follow the form

```
MAP -[override|preserve] -d (RET | INST | PAR -i $i) -s (INST | PAR -i $i) METHOD -f "$fullName"
```

where 
`$i` is a parameter number 
`$fullName` is the full name of a method. 

MAP statements associate a source parameter of a given method with a destination parameter. As a result, ShiftLeft is informed that, if the source parameter is tainted, then the destination parameter is tainted after execution of the library method. 

As an example, if the method "java.lang.String.concat:java.lang.String(java.lang.String)" of the Java standard library has a tainted instance parameter, then so is the return value of the library call. This directive is specify using the MAP directive

```
MAP -override -d RET -s INST METHOD -f "java.lang.String.concat:java.lang.String(java.lang.String)"
```

The directive indicates that, if the instance parameter (INST) is tainted, then the return parameter (RET) is tainted after execution of the method. The flag "-preserve" indicates to additionally add a mapping from the destination to itself.

```
MAP -preserve -d INST -s PAR -i 1 METHOD -f "java.lang.StringBuffer.append:java.lang.StringBuffer(java.lang.StringBuffer)"
```
	
is the shorthand of

```
MAP -override -d INST -s PAR -i 1 METHOD -f "java.lang.StringBuffer.append:java.lang.StringBuffer(java.lang.StringBuffer)"
MAP -override -d INST -s INST METHOD -f "java.lang.StringBuffer.append:java.lang.StringBuffer(java.lang.StringBuffer)"
```
