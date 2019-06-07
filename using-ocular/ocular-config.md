# ShiftLeft Ocular Memory Size Recommendations

By default, disk overflow is enabled for Code Property Graphs (CPG). Disk overflow moves cache entries to disk only when the number of entries in memory exceed the maximum memory allocation. Based on the query, ShiftLeft Ocular identifies the parts of the CPG needed, which are loaded into RAM. All other sections of the CPG are temporarily stored to disk. 

However, when analyzing large applications, disk overflow can slow down performance. Instead, you can optimize ShiftLeft Ocular performance through the appropriate combination of memory and disks sizes. 

Note that estimating the necessary amount of memory is non-trivial. ShiftLeft Ocular determines the amount of memory need and presents the following (example) information when you load or [create a CPG](../tutorials/cpgcreate.md): 

```
createCpg("someBigApp.jar")
// ...
// debug output from java2cpg
// ...
The cpg.bin.zip you're trying to load is 63MB on disk and (based on a very rough calculation) will require ~12712MB heap, but the current max heap size is 455MB.
We suggest to provide a large enough `-J-Xmx` setting, e.g. `ocular.sh -J-Xmx14g`
Please make sure that your machine has sufficient free physical memory available.
If you choose to disregard this message, the loading of the cpg might take longer, as the disk will be used to store part of the graph.
```
You can cancel the loading of the current CPG using `Ctrl-c`, close ShiftLeft Ocular, and then restart it with more memory, e.g. `./ocular.sh -J-Xmx14g` for 14GB heap.

If the available heap isn't sufficient, the following (example) information is displayed, which indicates that parts of the CPG are stored on disk (slowing performance):

```
INFO heap usage (after GC) was 0.87 -> scheduled to clear 100000 references (asynchronously)
INFO attempting to clear 100000 references
```
