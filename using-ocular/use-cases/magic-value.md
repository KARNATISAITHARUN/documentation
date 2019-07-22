# Finding a Magic Value

Developer intentionally places a piece of code (Easter Egg) which is triggered based on a magic value entered in form field or REST API. In order to bypass compliance for debugging production data or preparation for attack. Place a hardcoded value into a field of the form, and using that hardcoded value, for example redirect to another page which exposes the database. 

1. Is there an HTTP source

val source = cpg.method.parameter.evalType(globals.httpSources)

2. Does it use commandInjectionSinks? A request that is emerging from the source is fetching a parameter of type tracefn and then evaluating the control flow to see if the trace function is holding a literal of type magic value and then executing a command injection type function.

val source = cpg.method.parameter.evalType(globals.commandInjectionSink).parameter

3. If there is communication between source and sink, is there a condition in between. Filter on argument set with order 1

val conditionSource = cpg.call.name("equals").filter(_.argument.order(1).literal.typ.name("String"))

4. Expand source/sink flow to introduce a new contruct branchPointReachableBy. Can a source touch a sink with a branch point or a control flow, or any other construct that is evaluating for a certain condition between the source and sink

globals.printFlows(sink.reachableBy(source).flows.branchPointReachableBy(conditionSource))

Result map of detailed trace of entire flow. There is a parameter that fetched from the HTTP request that eventually touches Runtime.java.exec. There is hardcoded compaisons in your code flow, which indicates magic value. 
