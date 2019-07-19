# ShiftLeft Ocular Memory Size Recommendations

By default, disk overflow is enabled for Code Property Graphs (CPG). Disk overflow moves cache entries to disk only when the number of entries in memory exceed the maximum memory allocation. Based on the query, ShiftLeft Ocular identifies the parts of the CPG needed, which are loaded into RAM. All other sections of the CPG are temporarily stored to disk. 

However, when analyzing large applications, disk overflow can slow down performance. Instead, you can optimize ShiftLeft Ocular performance through the appropriate combination of memory and disk sizes, either by automating the provision of your servers or by doing it manually. 

Note that estimating the necessary amount of memory is non-trivial. 

## Automating the Provision of Your Servers

Before [starting ShiftLeft Ocular](../getting-started/starting.md) to examine a specific application or application version, use the following script to estimate the required memory size

```
./ocular.sh --script scripts/memory-recommendation.sc --params artifactPath=subjects/(<inputPath>
```

where `<inputPath>` is the absolute path of the target application.

ShiftLeft Ocular returns, in the last line of the output, the recommended heap size in megabytes that you need in order to run ShiftLeft Ocular. For example

```
...
script finished successfully
3633
```

In this example, ShiftLeft Ocular has estimated a required heap size of 3633 megabytes. It is suggested that you use a heap size that includes an additional 20% to ensure that you have sufficient physical memory on your server for other requirements, like Java. So for this example, you would want to use a heap size of approximately 4000 megabytes.

Then each time you start ShiftLeft Ocular, append the command `-J-Xmx<heapsize>`, so for example

```
./ocular.sh -J-Xmx4000m
```

## Manually Provisioning Your Servers

ShiftLeft Ocular determines the amount of memory needed and presents the following (example) information when you [create a CPG](create-cpg.md) or [load a CPG](working-with-cpg.md): 

```
createCpg("someBigApp.jar")
// ...
// debug output from java2cpg
// ...
The cpg.bin.zip you are loading is 63MB on disk and requires approximately 12712MB heap, but the current maximum heap size is 455MB. It is suggested that you provide a larger `-J-Xmx` setting, e.g. `ocular.sh `-J-Xmx14g` to ensure that your machine has sufficient free physical memory available. Otherwise, disk overflow is automatically enabled, slowing performance.
```
You can cancel the loading of the current CPG using `Ctrl-c`, close ShiftLeft Ocular, and then restart it with more memory, e.g. `./ocular.sh -J-Xmx14g` for 14GB heap.

If the available heap isn't sufficient, the following (example) information is displayed, which indicates that parts of the CPG are stored on disk (slowing performance):

```
INFO heap usage (after GC) was 0.87 -> scheduled to clear 100000 references (asynchronously)
INFO attempting to clear 100000 references
```
