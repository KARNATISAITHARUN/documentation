Based on Chetan's Webinar "How To Find Malicious Backdoors and Business Logic Vulnerabilities in Your Code"

# Locating Insider Threats

Insider threats are caused by trusted individuals placing certain patterns in the code to serve their malicious intent. 
Insider threats are not only the most common cause of cybersecurity risk, but also the costliest and hardest to detect.

Using tarpit

## Logic Bomb

Malicious code that executes when specific trigger conditions are met, for example a date and time. When a logic bomb is triggered, it may perform a denial of service such as crashing the system, deleting critical data or degrading system response time.

1. Investigating for a Logic Bomb
2. Identifying all the patterns leading to its execution
3. Automatically creating an issue in GitHub
4. Encaptsulating all information into a script and appending it into your CICD pipeline


1. Find all sinks in code

ocular> cpg.sink.method.fulName.l.distinct

Find sinks with hard coded literal with the pattern decode

cpg.sink.method.fulName.l.distinct.filter(_.contains("decode"))

results is that tarpit is using decode function two types.

2. Find out where the decode function is used.

cpg.method.name("decode").caller.fulName.p

Result is used in the function ticking with base 64 decoding

3. Use ShiftLeft convenience function to identify what is being decoded

ocular> globals.getSuspiciousLiterals(cpg)

result type and value that indicates there is a suspicious shell script that has been provisioned on the host.

Li90 ...

4. Identify which method encompases this literal

ocular> cpg.literal.code(".*Li90...*").method.fulName.l

result - there is another super method which is exposed called doGet on HTTP route that is encompasing the ticking function. Indicates that this logic bomb is triggered by the doGet request (which is initiated every time a user interacts with the application). doGet is calling ticking, comparing for a date function, and then triggering the execution of the shell script.

5. Use the source sink transformation paradim, which the higher level information flow provided by ShiftLeft that allows you identify patterns in your code. Assign the literal to a variable source

ocular> val source = cpg.literal.code(".*Li90...*")

6. Identify if this source is connected to a sink that is a decode function, that has a parameter of evalType String

ocular> val sink = cpg.method.name(".*decode.*")parameter.evalType("java.lang.String")

7. Conduct high-level flow anaysis by contecting the source and the sink together to illustrate how this information flows

ocular> sink.reachableBy(source).flows.p

result is a view of how information is traversing from the source to the sink. Evident that the literal Li90... is tracked at doGet via the ticking function which is encompasing the decode function. Proves that a hardcoded literal is being decoded.

8. Idenify if there are time (calendar) functions in the code which are being compared to enable the triggering of this logic bomb

cpg.method.fulName(".*Calendar.*set.*").caller.fulName.p

result - ticking is using calendar.set

9. Mark this as source to see if ticking comparing between calendar.set and calendar.after 

val source = cpg.method.fulName(".*Calender.*set.*").parameter
val sink = cpg.method.fulName(".*Calender.*after.*").parameter

10. Determine if there is a control loop in the code which is accepting this hardcoded encoded literal and checking for a condition and then executing that logic bomb shell script

sink.reachableBy(source).flows.p

result - it is evident that there is a calendar function

These two symptoms are indicating that this piece of code is suspicious.

11. Is there a connection between that hardcoded literal to a command injection function that enables execution of a remote command

val source = cpg.literal.code(".*Li90...*")
val sink = cpg.method.fulName(globals.commandInjectionSinks). parameter.evalType("java.lang.String")

12. Identify the flows using source and sink paradim

sink.reachableBy(source).flows.p

result pattern and trace show there are three conditions that indicate a logic bomb

13. Use convenience function takes flow and creates a JSON file representation

val flowString = globals.getFlowTrace(sink.reachableBy(source).flows)

14. Export JSON information to code review system. For example to create an issue in GitHub

globals.createIssueInGitHub(flowString, globals.access,Token, globals.owner, "tarpit", "Logic bomb detected. Fix before it triggers")

15. Encapsulate this entire investigation into a script and append it to your CRCD pipeline, so that the issue can be measured across releases

Use this command in your build systems (non-interactive mode)

./ocular.sh --scripts/automate.sc --params jarFile=/Users/absolute filename,outFile=out.log

results are piped into out.log. Information includes entire result set with a clear explanation of what has happened
