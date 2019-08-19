# Enhancing the Ocular Query Language (OQL)

The OQL is used to query Code Property Graphs (CPGs) and Security Profiles. OQL results can be integrated into your security tools and for sharing data across the SDLC (link to integrating article).

The OQL can be enhanced for use with your specific code base or environment, to represent queries that you use over and over again, or to combine complex and multiple queries into a single query. 

Use the "Enhance my Library Pattern". For example, a method named `whatICareAbout` is added to a query, which is subsequently  evaluated just like built-in language elements when the query is used.

```
import io.shiftleft.passes.findings.steps.Finding

class MyMethods(finding : Finding) {
    <b>def whatICareAbout = finding.filterOnEnd(x => x.score >= 8 && x.categories.contains("a1-injection"))</b>
}

implicit def conv(findings: Finding) = new MyMethods(findings)

@main def exec(spFilename: String, outFilename: String) = {
    loadCpgWithOverlays("cpg.bin.zip", spFilename)
    sp.findings.whatICareAbout.l |> outFilename
}
```
