# ShiftLeft Ocular Memory Size Recommendations

By default, disk overflow is enabled for Code Property Graphs (CPG). Disk overflow moves cache entries to disk only when the number of entries in memory exceed the maximum memory allocation. Based on the query, ShiftLeft Ocular identifies the parts of the CPG needed, which are loaded into RAM. All other sections of the CPG are temporarily stored to disk. 

However, when analyzing large applications, disk overflow can slow down performance. Instead, you can optimize ShiftLeft Ocular performance through the appropriate combination of memory and disk sizes. 

Note that estimating the necessary amount of memory is non-trivial. ShiftLeft Ocular determines the amount of memory needed and presents the following (example) information when you [create a CPG](create-cpg.md) or [load a CPG](working-with-cpg.md): 

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
