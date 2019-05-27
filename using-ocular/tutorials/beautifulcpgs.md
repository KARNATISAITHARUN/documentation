# Constructing Beautiful Code Property Graphs (CPGs) from Java Archives

This tutorial demonstrates how to create CPGs from 
Java archives (JAR or WAR files). Using the ShiftLeft Ocular
toolset, you create efficient CPGs to improve your results and speed up 
the analysis of your application. 

ShiftLeft Ocular is shipped with a Java front-end tool named `java2cpg`. `java2cpg` creates CPGs from Java projects, 
and can be customize to your specific needs. 

To illustrate the impact of parameterizing `java2cpg`, a Jenkins WAR package is used. The shell-commands 
for downloading the WAR package from the Jenkins site, with a size 
of around 73M, is

```
	cd ~/bin/ocular/subjects
	wget http://mirrors.jenkins.io/war/latest/jenkins.war
	cd ..	
```

In order to analyze the WAR file, use the `java2cpg` **dry-run** feature instead 
of simply running `java2cpg` on `subjects/jenkins.war`. This feature determines
which Java packages `java2cpg` are **included** (Including) and **skiped** (Skipping) 
for CPG construction, without actually performing time and memory intense calculation.

`-dr` is the flag used to make `java2cpg` perform a dry-run. 

```
	$ time ./java2cpg.sh subjects/jenkins.war -dr | egrep '(Including|Skipping)' > packages.txt
	$ wc -l packages.txt
	1303 packages.txt
	$ grep 'Including' packages.txt| wc -l
	240
	$ grep 'Skipping' packages.txt| wc -l
	1063
```

Notice that `java2cpg` has recursively unpacked the WAR to detect a
total of 1303 packages, 240 of which were automatically selected for inclusion
in the CPG, and 1063 packages were discarded via the default
blacklist. The next step is to review the skipped packages to see if any relevant packages have been excluded.

```
grep 'Skipping' packages.txt| less
```

The results indicate that no relevant packages have been excluded, as the default blacklist contains only names of common libraries and frameworks such as `spring` or `log4j`. Next, examine the list of included packages

```
grep 'Included' packages.txt| less
```

Among the packages `jenkins`, `lib.jenkins`, `org.jenkins`, and `org.jenkinsci`, libraries such as `gnu.crypt` and `winstone` are apparently not on the blacklist and would therefore be included in the CPG. This is not desirable, since the goal is to find vulnerabilities in Jenkins, and not in one of its dependencies. In this particular case, a good choice is therefore to simply use a whitelist that includes Jenkins and Hudson packages (Hudson is Jenkin's predecessor).

```
time ./java2cpg.sh subjects/jenkins.war -nb -w jenkins,org.jenkins,org.jenkinsci,lib.jenkins,hudson
```

The resulting CPG is 44 MB in size, in contrast to the 70MB CPG generated using the default blacklist. Subsequent analysis of this CPG focuses on the Jenkins code and skips libraries such as `hudson`, and `gnu.crypt`, which were included originally only due to lack of blacklisting.

However, by not telling `java2cpg` which classes are of interest, the resulting 
CPGs may contain too little code, or too much. `java2cpg` does offer
capabilities for you to quickly select Java packages of
interest. 

It is important to note that blacklisting a package is **not** removing the calls to this package. Blacklisting just avoids an analysis of the package and while still adding the content of the package to the CPG. In other words, if Hudson is blacklisted, calls to this dependency are still shown in the CPG, but not what is inside of these calls. 
