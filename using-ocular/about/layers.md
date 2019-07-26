# Understanding Layers

The Code Property Graph (CPG) is a multi-layered representation of each unique code version of your application. The most important layers are the `base` layer, the two default layers `semanticcpg` and `tagging`, and the `securityprofile` layer.

Additional information on CPG layers is provided in the [API](https://ocular.shiftleft.io/api/io/shiftleft/repl/cpgcreation/Overlays$.html). Layers are also frequently referred to as "overlays".

### `base` Layer

For each language supported by ShiftLeft Ocular, the [language frontend component](https://ocular.shiftleft.io/api/io/shiftleft/repl/cpgcreation/LanguageFrontend.html) translates code written in that language into a CPG through the `base` layer. For example, the JavaLanguageFrontend translates a JAR into a CPG. 

The `base` layer is the foundation of the CPG, but is not sufficiently expressive for the purpose of examining data flows and vulnerabilities.

## Default Layers

The default layers are automatically created and loaded into memory when you create a CPG, or load an existing CPG.

### `semanticcpg` Layer

The `semanticcp` layer links together the methods and type declaration. Using the `base` layer and semantic information, the `semanticcp` layer

* introduces additional information based on framework specific passes.
* adds data flow semantics to the CPG as specified by [Policies](../../policies/spl.md).
      
### `tagging` Layer

The `tagging` layer identifies possible attacker-controlled data sources and interesting sinks as specified by [Policies](../../policies/spl.md).
      
## The `securityprofile` Layer

The `securityprofile` layer is dervied from [Policies](../../policies/spl.md), and summarizes the vulnerabilities and data leaks present in your code. Specifically, it describes: 

* Read operations
* Write operations
* Transformations
* Security-relevant data flows
* Information flows
* Findings

Unlike the `base` and default layers, because the Security Profile is so expensive, it must be intentially created and loaded. 
