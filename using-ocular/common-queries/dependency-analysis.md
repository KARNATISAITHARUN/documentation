# Examining Dependencies

This example describes using ShiftLeft Ocular to examine application dependencies. It uses [HelloShiftLeft](../../introduction/helloshiftleft.md) to illustrate the steps and results.

To understand the HelloShiftLeft application dependencies (internal packages as well as external open-source dependencies), query the application's Code Property Graph (CPG) 

```
ocular> cpg.dependency.map(x => (x.name, x.version)).l
res2: List[(String, String)] = List( ("jackson-core", "2.8.6"),
("spring-context", "4.3.6.RELEASE"), ("spring-core", "4.3.6.RELEASE"), ("javax.transaction-api", "1.2"),
("javassist", "3.21.0-GA"), ("spring-boot-starter-data-jpa", "1.5.1.RELEASE"), ("spring-boot-actuator", "1.5.1.RELEASE"), ("jasypt-spring-boot", "1.11"), ("jasypt-spring-boot-starter", "1.11")
..
.. )
```

Notice the snippet of the dependency list, which can be further investigated against known CVEs from vulnerability databases such as NVD.
